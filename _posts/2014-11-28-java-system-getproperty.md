---
layout: post
title: java的System.getProperty()方法
category: [java,javaweb]
tags: [javaweb, 学习笔记]
date: 2014-11-28
---

<table>
  <thead>
    <tr>
      <th>参数</th>
      <th>意义</th>
    </tr>
  </thead>
  <tbody>
<tr>
        <td>java.version</td>
        <td>Java 运行时环境版本</td>
    </tr>
    <tr>
        <td>java.vendor</td>
        <td>Java 运行时环境供应商</td>
    </tr>
    <tr>
        <td>java.vendor.url</td>
        <td>Java 供应商的 URL</td>
    </tr>
    <tr>
        <td>java.home</td>
        <td>Java 安装目录</td>
    </tr>
    <tr>
        <td>java.vm.specification.version</td>
        <td>Java 虚拟机规范版本</td>
    </tr>
    <tr>
        <td>java.vm.specification.vendor</td>
        <td>Java 虚拟机规范供应商</td>
    </tr>
    <tr>
        <td>java.vm.specification.name</td>
        <td>Java 虚拟机规范名称</td>
    </tr>
    <tr>
        <td>java.vm.version</td>
        <td>Java 虚拟机实现版本</td>
    </tr>
    <tr>
        <td>java.vm.vendor</td>
        <td>Java 虚拟机实现供应商</td>
    </tr>
    <tr>
        <td>java.vm.name</td>
        <td>Java 虚拟机实现名称</td>
    </tr>
    <tr>
        <td>java.specification.version</td>
        <td>Java 运行时环境规范版本</td>
    </tr>
    <tr>
        <td>java.specification.vendor</td>
        <td>Java 运行时环境规范供应商</td>
    </tr>
    <tr>
        <td>java.specification.name</td>
        <td>Java 运行时环境规范名称</td>
    </tr>
    <tr>
        <td>java.class.version</td>
        <td>Java 类格式版本号</td>
    </tr>
    <tr>
        <td>java.class.path</td>
        <td>Java 类路径</td>
    </tr>
    <tr>
        <td>java.library.path</td>
        <td>加载库时搜索的路径列表</td>
    </tr>
    <tr>
        <td>java.io.tmpdir</td>
        <td>默认的临时文件路径</td>
    </tr>
    <tr>
        <td>java.compiler</td>
        <td>要使用的 JIT 编译器的名称</td>
    </tr>
    <tr>
        <td>java.ext.dirs</td>
        <td>一个或多个扩展目录的路径</td>
    </tr>
    <tr>
        <td>os.name</td>
        <td>操作系统的名称</td>
    </tr>
    <tr>
        <td>os.arch</td>
        <td>操作系统的架构</td>
    </tr>
    <tr>
        <td>os.version</td>
        <td>操作系统的版本</td>
    </tr>
    <tr>
        <td>file.separator</td>
        <td>文件分隔符（在 UNIX 系统中是“/”）</td>
    </tr>
    <tr>
        <td>path.separator</td>
        <td>路径分隔符（在 UNIX 系统中是“:”）</td>
    </tr>
    <tr>
        <td>line.separator</td>
        <td>行分隔符（在 UNIX 系统中是“/n”）</td>
    </tr>
    <tr>
        <td>user.name</td>
        <td>用户的账户名称</td>
    </tr>
    <tr>
        <td>user.home</td>
        <td>用户的主目录</td>
    </tr>
    <tr>
        <td>user.dir</td>
        <td>用户的当前工作目录
        </td>
    </tr>
  </tbody>
</table>

代码示例：  
<!-- more -->

{% highlight java %}
public class TestProperty {
	public static void main(String args[]) {   
    System.out.println("java_vendor:" + System.getProperty("java.vendor"));   
    System.out.println("java_vendor_url:" + System.getProperty("java.vendor.url"));   
    System.out.println("java_home:" + System.getProperty("java.home"));   
    System.out.println("java_class_version:" + System.getProperty("java.class.version"));   
    System.out.println("java_class_path:" + System.getProperty("java.class.path"));   
    System.out.println("os_name:" + System.getProperty("os.name"));   
    System.out.println("os_arch:" + System.getProperty("os.arch"));   
    System.out.println("os_version:" + System.getProperty("os.version"));   
    System.out.println("user_name:" + System.getProperty("user.name"));   
    System.out.println("user_home:" + System.getProperty("user.home"));   
    System.out.println("user_dir:" + System.getProperty("user.dir"));   
    System.out.println("java_vm_specification_version:" + System.getProperty("java.vm.specification.version"));   
    System.out.println("java_vm_specification_vendor:" + System.getProperty("java.vm.specification.vendor"));   
    System.out.println("java_vm_specification_name:"  + System.getProperty("java.vm.specification.name"));   
    System.out.println("java_vm_version:" + System.getProperty("java.vm.version"));   
    System.out.println("java_vm_vendor:"  + System.getProperty("java.vm.vendor"));   
    System.out .println("java_vm_name:" + System.getProperty("java.vm.name"));   
    System.out.println("java_ext_dirs:" + System.getProperty("java.ext.dirs"));   
    System.out.println("file_separator:" + System.getProperty("file.separator"));   
    System.out.println("path_separator:" + System.getProperty("path.separator"));   
    System.out.println("line_separator:" + System.getProperty("line.separator"));   
}  
{% endhighlight %} 
