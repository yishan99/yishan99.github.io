---
title: IO开篇！
date: 2020-09-02 15:15:30
tags: [java基础,IO]
---
### 五个类 
- File  文件类
- InputStream   字节输入流   
- OutputStream  字节输出流
- Reader    字符输入流
- Writer    字符输出流
<!--more-->
### 三个接口
- Closeable 关闭流接口
- Flushable 刷新流接口
- Serializable  序列化接口

### 测试路径

```java
    package com.yishan.io;

    import java.io.File;

    /**
    * 测试路径
    * @author yishan
    */
    public class TestIoDemo01 {
        public static void main(String[] args) {
            String path = "F:\\My direction\\java SE\\IO\\src\\111.png";
            System.out.println(File.separatorChar);
            //建议
            path = "F:/My direction/java SE/IO/src/111.png";
            System.out.println(path);
            //常量拼接
            path = "F:"+File.separatorChar+"My direction"+File.separatorChar+"java SE"+File.separatorChar+"IO"+File.separatorChar+"src"+File.separatorChar+"111. png ";
            System.out.println(path);
        }
    }
```

### 测试构造器

```java
    package com.yishan.io;

    import sun.net.www.content.image.png;

    import java.io.File;

    /**
    * 测试构造器
    * @author yishan
    */
    public class FileDemo01 {
        public static void main(String[] args) {
            /**
            * 构建File对象
            */
        String path = "F:/My direction/java SE/IO/src/111.png";

        //1.构建File对象
            File src = new File(path);
            System.out.println(src.length());

            //2.构建File对象
            src = new File("F:/My direction/java SE/IO/src", "111.png ");
            System.out.println(src.length());
            src = new File("F:/My direction","java SE/IO/src/111.png ");
            System.out.println(src.length());

            //3.构建File对象
            src = new File(new File("F:/My direction/java SE/IO/src"),"111.png");
            System.out.println(src.length());
        }
    }
```

### 测试相对路径和绝对路径

```java
    package com.yishan.io;

    import java.io.File;

    /**
    * 测试相对路径和绝对路径
    * @author yishan
    */
    public class FileDemo02 {
        public static void main(String[] args) {
            /**
            * 构建File对象
            * 相对路径和绝对路径
            * 1.存在盘符：绝对路径
            * 2.不存在盘符：相对路径  当前目录：user.dir
            */
        String path = "F:/My direction/java SE/IO/src/111.png";
            //绝对路径
            File src = new File(path);
            System.out.println(src.getAbsolutePath());

            //相对路径
            System.out.println(System.getProperty("user.dir"));
            src = new File("111.png");
            System.out.println(src.getAbsolutePath());

            //构建一个不存在的文件
            src = new File("aaa.png");
            System.out.println(src.getAbsolutePath());
        }
    }
```

