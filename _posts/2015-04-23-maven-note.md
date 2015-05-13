---
layout: post
keywords: maven
description:
title: "maven note"
categories: [java]
tags: [java, maven, note]
group: archive
icon: file-alt
---
{% include oukongli/setup %}

{% highlight java %}
1. Pom.xml
	src/main/java
	src/test/java
			
2. Mvn compile
	Mvn test
	Mvn package
	Mvn clean 
	Mvn install

3. Mvn archetype:generate
	• ${basedir} 项目根目录
	• ${project.build.directory} 构建目录，缺省为target
	• ${project.build.outputDirectory} 构建过程输出目录，缺省为target/classes
	• ${project.build.finalName} 产出物名称，缺省为${project.artifactId}-${project.version}
	• ${project.packaging} 打包类型，缺省为jar
	• ${project.xxx} 当前pom文件的任意节点的内容

4. Unit test
	Dbutil 
	Easymock

5. Maven scope
	Test 测试范围有效，编译和打包时不会使用这个依赖;
	Compile 默认的范围，编译范围有效，在编译、测试、打包时都会将依赖存储;
	Provided 编译、测试有效，生成war包时不会加入：(servlet-api);
	Runtime 在运行的时候有效，编译时候不依赖;
	
6. Dependency
	- a.
		A --> L1.1 jar
		B --> L1.0 jar
		C --> A, B    ----------- C -->L1.1   jar（A在前）
	- b.
		A-->T-->L1.1 jar
		B-->L1.0 jar 
		C --> A, B -------- C-->L1.0 jar (离得最近)

7. Maven 聚合和继承
	<mojules> 
	<dependencyManagement>

{% endhighlight %}
<!-- more -->

## 参考视频
[孔浩老师Java视频](http://www.konghao.org)