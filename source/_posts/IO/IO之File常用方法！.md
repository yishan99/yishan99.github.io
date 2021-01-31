---
title: IO之File常用方法！
date: 2020-09-02 22:34:23
tags: [java基础,IO]
---

### 基本信息
<!--more-->
        package com.yishan.io;

```java
    import java.io.File;

    /**
    * 测试File常用方法
    * getName():获取名称
    * getPath()：定义相对路径返回相对路径，定义绝对路径返回绝对路径
    * getAbsolutePath()：始终返回绝对路径
    * getParent()：获取上个路径 ，若不存在，返回null
    * @author yishan
    */
    public class FileDemo03 {
        public static void main(String[] args) {
            File src = new File("F:/My direction/java SE/IO/src/111.png");

            //基本信息
            System.out.println("名称："+src.getName());
            System.out.println("路径："+src.getPath());
            System.out.println("绝对路径："+src.getAbsolutePath());
            System.out.println("父路径："+src.getParent());
            System.out.println("父对象："+src.getParentFile().getName());
        }
    }
```

### 文件状态

```java
    package com.yishan.io;

    import java.io.File;

    /**
    * 文件状态
    * 1.不存在：exists
    * 2.存在
    *      文件：isFile
    *      文件夹：isDirectory
    * @author yishan
    */
    public class FileDemo04 {
        public static void main(String[] args) {
            File src = new File("src/111.png");
            System.out.println(src.getAbsolutePath());
            System.out.println("是否存在："+src.exists());
            System.out.println("是否是文件："+src.isFile());
            System.out.println("是否是文件夹："+src.isDirectory());

            System.out.println("-----------------------------------");

            src = new File("111.png");
            System.out.println("是否存在："+src.exists());
            System.out.println("是否是文件："+src.isFile());
            System.out.println("是否是文件夹："+src.isDirectory());

            System.out.println("-----------------------------------");

            src = new File("F:/My direction/java SE/IO/src");
            System.out.println("是否存在："+src.exists());
            System.out.println("是否是文件："+src.isFile());
            System.out.println("是否是文件夹："+src.isDirectory());

            //文件状态
            src = new File("xxx");
            if (null == src || !src.exists()){
                System.out.println("文件不存在");
            }else{
                if (src.isFile()){
                    System.out.println("文件操作");
                }else {
                    System.out.println("文件夹操作");
                }
            }
        }
    }
```

### 其他信息

        package com.yishan.io;


```java
    import java.io.File;

    /**
    * 其他信息
    * length():字节数
    * @author yishan
    */
    public class FileDemo05 {
        public static void main(String[] args) {

            File src = new File("F:/My direction/java SE/IO/src/111.png");
            System.out.println("长度："+src.length());

            src = new File("F:/My direction/java SE/IO/src");
            System.out.println("长度："+src.length());

            src = new File("F:/My direction/java SE/IO/src2/111.png");
            System.out.println("长度："+src.length());
        }
    }
```

### 创建删除文件

```java
    package com.yishan.io;

    import java.io.File;
    import java.io.IOException;

    /**
    * 其他信息
    * createNewFile():不存在才创建，存在创建成功
    * delete():删除已经存在的文件
    * @author yishan
    */
    public class FileDemo06 {
        public static void main(String[] args) throws IOException {
            File src = new File("F:/My direction/java SE/IO/src/222.txt");
            boolean flag = src.createNewFile();
            System.out.println(flag);
            flag = src.delete();
            System.out.println(flag);

            System.out.println("------------------");

            //创建的不是文件夹
            src = new File("F:/My direction/java SE/IO/src3");
            flag = src.createNewFile();
            System.out.println(flag);
            flag = src.delete();
            System.out.println(flag);

            //con,com...操作系统的设备名，不能正确创建
            src = new File("F:/My direction/java SE/IO/src/com");
            flag = src.createNewFile();
            System.out.println(flag);

        }
    }
```
### 创建目录

```java
    package com.yishan.io;

    import java.io.File;

    /**
    * 创建目录
    * 1.mkdir():确保上级目录存在，不存在创建失败
    * 2.mkdirs():上级目录可以不存在，不存在一起创建
    * @author yishan
    */
    public class DirDemo01 {
        public static void main(String[] args) {
            File dir = new File("F:/My direction/java SE/IO/dir/test");

            //创建目录,推荐使用 mkdirs()
            boolean flag = dir.mkdirs();
            System.out.println(flag);

            dir = new File("F:/My direction/java SE/IO/dir/test2");
            flag = dir.mkdir();
            System.out.println(flag);

        }
    }
```


### 列出下级

```java
    package com.yishan.io;

    import java.io.File;

    /**
    * 列出下一级
    * 1.list():列出下级名称
    * 2.listFile():列出下级File名称
    *
    * @author yishan
    */
    public class DirDemo02 {
        public static void main(String[] args) {
            File dir = new File("F:/My direction/java SE");

            //列出下级名称 list
            String[] subNames = dir.list();
            for (String s:subNames){
                System.out.println(s);
            }

            //下级对象 listFiles()
            File[] subFiles = dir.listFiles();
            for (File s:subFiles){
                System.out.println(s.getAbsolutePath());
            }

            //所有盘符
            File[] roots = dir.listRoots();
            for (File r:roots){
                System.out.println(r.getAbsolutePath());
            }
        }
    }
```

### 递归

```java
    package com.yishan.io;

    import java.io.File;

    /**
    * 递归：方法自己调用自己
    * 递归头：何时结束递归
    * 递归体：重复调用
    * @author yishan
    */
    public class DirDemo03 {
        public static void main(String[] args) {
            File dir = new File("F:/My direction/java SE/IO/dir/test");

            printTen(1);
        }

        public static void printTen(int n){
            if (n>10){
                return;
            }
            System.out.println(n);
            printTen(n+1);

        }
    }
```

### 递归打印子孙级目录

```java
    package com.yishan.io;

    import java.io.File;

    /**
    * 递归：方法自己调用自己
    * 打印子孙级目录和文件的名称
    * @author yishan
    */
    public class DirDemo04 {
        public static void main(String[] args) {
            File src = new File("F:/My direction/java SE");
            printName(src,0);
        }

        // 打印子孙级目录和文件的名称
        public static void printName(File src,int deep){
            //控制前面的层次感
            for (int i = 0;i<deep;i++){
                System.out.print("-");
            }
            //打印名称
            System.out.println(src.getName());
            if(null == src || !src.exists()){ //递归头
                return;
            }else if(src.isDirectory()){    //目录
                for (File s:src.listFiles()){
                    printName(s,deep+1);    //递归体
                }
            }
        }
    }
```

### 统计文件夹的大小

```java
    package com.yishan.io;

    import java.io.File;

    /**
    * 统计文件夹的大小
    * @author yishan
    */
    public class DirDemo05 {
        public static void main(String[] args) {
            File src = new File("F:/My direction/java SE");
            count(src);
            System.out.println(len);
        }

        private static long len = 0;
        public static void count(File src){
            //获取大小
            if (null!=src && src.exists()){
                if (src.isFile()){
                    len+=src.length();
                }else{
                    for (File s:src.listFiles()){
                        count(s);
                    }
                }
            }
        }
    }
```

### 面向对象统计文件夹的大小

```java
    package com.yishan.io;

    import java.io.File;

    /**
    * 使用面向对象统计文件夹的大小
    * @author yishan
    */
    public class DirCount {
        //大小
        private long len;
        //文件夹
        private String path;
        //源
        private File src;
        //文件的个数
        private int FileSize;
        //文件夹的个数
        private int DirSize;

        public DirCount(String path){
            this.path = path;
            this.src = new File(path);
            count(src);
        }

        public static void main(String[] args) {
            DirCount dir = new DirCount("F:/My direction/java SE");
            System.out.println(dir.getLen()+"--"+dir.getDirSize()+"--"+dir.getFileSize());
        }

        //统计大小
        private  void count(File src){
            //获取大小
            if (null!=src && src.exists()){
                if (src.isFile()){
                    this.FileSize++;
                    len+=src.length();
                }else{
                    this.DirSize++;
                    for (File s:src.listFiles()){
                        count(s);
                    }
                }
            }
        }

        public long getLen() {
            return len;
        }

        public int getFileSize() {
            return FileSize;
        }

        public int getDirSize() {
            return DirSize;
        }
    }
```

