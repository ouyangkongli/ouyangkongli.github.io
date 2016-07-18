---
layout: post
title: log4j 多线程同步问题
description:  log4j 多线程同步问题
tag: [java, log4j, synchronization ]
comments: true
categories: java
---

Apache log4j 官方API介绍类[DailyRollingFileAppender](https://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/DailyRollingFileAppender.html)，
有这样一段话`DailyRollingFileAppender has been observed to exhibit synchronization issues and data loss.`。

参考文章：[http://hellojavaer.iteye.com/blog/977599](http://hellojavaer.iteye.com/blog/977599)

### DailyRollingFileAppender 多线程同步问题

下面来说明一下log4j多线程的多线程同步问题.

<!-- more -->

在 log4j 的 DailyRollingFileAppender 类中：

```java

    void rollOver() throws IOException {

    /* Compute filename, but only if datePattern is specified */
    if (datePattern == null) {
      errorHandler.error("Missing DatePattern option in rollOver().");
      return;
    }

    String datedFilename = fileName+sdf.format(now);
    // It is too early to roll over because we are still within the
    // bounds of the current interval. Rollover will occur once the
    // next interval is reached.
    if (scheduledFilename.equals(datedFilename)) {
      return;
    }

    // close current file, and rename it to datedFilename
    this.closeFile();

    File target  = new File(scheduledFilename);
    if (target.exists()) {
      target.delete();
    }

    File file = new File(fileName);
    boolean result = file.renameTo(target);
    if(result) {
      LogLog.debug(fileName +" -> "+ scheduledFilename);
    } else {
      LogLog.error("Failed to rename ["+fileName+"] to ["+scheduledFilename+"].");
    }

    try {
      // This will also close the file. This is OK since multiple
      // close operations are safe.
      this.setFile(fileName, false, this.bufferedIO, this.bufferSize);
    }
    catch(IOException e) {
      errorHandler.error("setFile("+fileName+", false) call failed.");
    }
    scheduledFilename = datedFilename;
}

```

该方法意义：在滚动备份时间间隔到的时刻，将前一时间间隔的日志备份，同时以非追加模式将新日志打到新日志文件中；中间部分代码意思是：如果备份文件不存在，则备份，同时创建新日志文件；如果存在，则先删除掉，再备份；

问题出现了，假设A,B线程同时project.log写log，A线程先进行滚动备份：

1、 对于A线程：
    
    a. 先将project.log备份（renameTo()）为project.log.2016.07.18，然后创建project.log文件，并将日志写在project.log中；

    b. 此时A线程持有project.log的文件句柄；而B线程仍然持有project.log.2016.07.18的文件句柄（尽管被重命名，但句柄不变）；

2、 对于B线程：发现以project.log.2016.07.18为文件名的文件已经存在，则将其删除（前一时间段的所有日志全没了），并将以project.log为文件名的文件重命名为project.log.2016.07.18，然后创建project.log文件；

3、 此时A线程持有project.log.2016.07.18的文件句柄（被B线程重命名过的），而B线程持有最新创建的project.log；

4、结果导致：前一时间段日志丢失，A、B线程在不同的文件里打日志；


### 解决方案

改变 rollOver() 方法的实现方式：定义 TaskDailyRollingFileAppender 类，该类继承至 FileAppender ，它与 DailyRollingFileAppender 的主要区别在于以下方法：

```java

void rollOver() throws IOException {

    /* Compute filename, but only if datePattern is specified */
    if (datePattern == null) {
      errorHandler.error("Missing DatePattern option in rollOver().");
      return;
    }

    String datedFilename = fileName+sdf.format(now);
    // It is too early to roll over because we are still within the
    // bounds of the current interval. Rollover will occur once the
    // next interval is reached.
    if (scheduledFilename.equals(datedFilename)) {
      return;
    }

    // close current file, and rename it to datedFilename
    this.closeFile();

    File target  = new File(scheduledFilename);
    if (!target.exists()) {
        File file = new File(fileName);
        boolean result = file.renameTo(target);
        if (result) {
            LogLog.debug(fileName + " -> " + scheduledFilename);
        } else {
            LogLog.error("Failed to rename [" + fileName + "] to [" + scheduledFilename + "].");
        }
    }

    try {
        // This will also close the file. This is OK since multiple
        // close operations are safe.
        this.setFile(fileName, true, this.bufferedIO, this.bufferSize);
    }
    catch(IOException e) {
      errorHandler.error("setFile("+fileName+", false) call failed.");
    }
    scheduledFilename = datedFilename;
}


```

改进后的 rollOver() 方法主要作用是：A进程先将日志重命名，然后创建新日志文件，B进程发现已经存在，则直接以追加模式切换到新的日志文件上去；