---
title: JAVA基础入门
date: 2020-04-29 13:29:29
tags: java基础
---
# JAVA基础
<!-- more -->
    java是一门跨平台语言：一次编写，处处运行
    java能够跨平台的原因，是因为有各种类型的jvm,且各个jvm不跨平台
    写代码（java）-->编译（class，字节码，相当于二进制）-->执行class
    jvm:java虚拟机（java virtual machine）

    jre:jvm+核心类库（java runtime environment）:只能运行JAVA程序，但不能开发

    jdk:jre+运行环境工具（java devvelopment kit）：既能开发JAVA程序，又能开发

    jvm<jre<jdk

# 环境变量的配置

window配置环境变量（大小写不区分）

    新建java_home:jdk的根目录
    path(必须)：jdk的bin目录，执行命令时，就会在path找相应的软件
    新建（类路径）classpath：jdk的lib目录，比如：.D:\JAVA\jdk.8.0_71\lib

    代码分为两部分：自己写的. + 别人写的（排序、安全、算法等）
验证
    cmd(win+r)
    输入java -version 如果出现jdk版本信息，则说明配置成功
![版本信息](https://s1.ax1x.com/2020/04/29/JT3UW4.png)