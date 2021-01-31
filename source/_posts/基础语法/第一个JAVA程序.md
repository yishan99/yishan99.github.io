---
title: 第一个JAVA程序
date: 2020-04-29 15:10:29
tags: java基础
---
# 示例程序
<!-- more -->
    public class Test{
        public static void main (String [args]){
            System.out.pringtln("Hello World!")；
        }
    }

1. class后面的称之为“类名”
2. public class 后面必须和文件名保持一致
3. 一个文件可以多个类（class），但是只能有一个公共类（public class）
4. System.out.println("Hello World!");输出语句
5. 每个语句以分号结尾（程序里面的一切符号都是 半角英文符号）
6. 大括号（各种括号）成对出现，注意缩进
7. 程序的入口就是main（）方法（其他语言称之为 函数）





    System.out.println();带回车的输出语句

    System.out.print();不带回车的输出语句

# 转义符  \

    \n:回车
System.out.println("aa\na");先执行（）中的各种操作，最后在ln回车

    \t：制表符
        补满一定位数：cmd中是八位

如果在.java中有汉字，并且在Javac编译中出现了错误提示“错误：编码GBK的不可映射字符”

解决：将文件的编码改为ANSI码

注意：java采用的默认 字符编码集是Unicode

# 注释
    单行注释 //
    多行注释 /*...*/
    文档注释 /**...*/ 
        将程序生成一个说明文档，则说明文档中的说明文字就是通过文档注释生成的

