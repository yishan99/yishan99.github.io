---
title: IO之Commons-io
date: 2020-09-26 10:54:08
tags: [java基础,IO]
---
### IO之Commons-io
<!--more-->

    package com.yishan.io4;


    import org.apache.commons.io.FileUtils;

    import java.io.File;

    /**
    * FileUtils.sizeOf()方法 查看文件大小
    * @author : yishan
    * @date : 2020-09-26 08:14
    */
    public class CIOTest01 {
        public static void main(String[] args) {
            //文件大小
            long len = FileUtils.sizeOf(new File("F:/My direction/java SE/IO/src/com/yishan/io4/CIOTest01.java"));
            System.out.println(len);
            //目录大小
            len = FileUtils.sizeOf(new File("F:/My direction/java SE/IO/src/com/yishan"));
            System.out.println(len);
        }
    }


        package com.yishan.io4;


        import org.apache.commons.io.FileUtils;
        import org.apache.commons.io.filefilter.DirectoryFileFilter;
        import org.apache.commons.io.filefilter.EmptyFileFilter;
        import org.apache.commons.io.filefilter.FileFilterUtils;
        import org.apache.commons.io.filefilter.SuffixFileFilter;

        import java.io.File;
        import java.util.Collection;

        /**
        * 列出子孙级
        * @author : yishan
        * @date : 2020-09-26 08:14
        */
        public class CIOTest02 {
            public static void main(String[] args) {
                //列出该目录下的非空文件
                Collection<File> files =  FileUtils.listFiles(new File("F:/My direction/java SE/IO/src/com/yishan"),
                        EmptyFileFilter.NOT_EMPTY,null);
                for (File file : files){
                    System.out.println(file.getAbsolutePath());
                }
                System.out.println("-------------------");
                //列出该目录下的非空文件的子孙级
                Collection<File> filess =  FileUtils.listFiles(new File("F:/My direction/java SE/IO/src/com/yishan"),
                        EmptyFileFilter.NOT_EMPTY, DirectoryFileFilter.INSTANCE);
                for (File file : filess){
                    System.out.println(file.getAbsolutePath());
                }
                System.out.println("-------------------");
                //列出该目录下的后缀名为java文件的子孙级
                Collection<File> filesss =  FileUtils.listFiles(new File("F:/My direction/java SE/IO/src/com/yishan"),
                        new SuffixFileFilter("java"), DirectoryFileFilter.INSTANCE);
                for (File file : filesss){
                    System.out.println(file.getAbsolutePath());
                }
                System.out.println("-------------------");
                //列出该目录下的非空文件和后缀名为java文件的子孙级
                Collection<File> filessss =  FileUtils.listFiles(new File("F:/My direction/java SE/IO/src/com/yishan"),
                        FileFilterUtils.and(new SuffixFileFilter("java"),EmptyFileFilter.NOT_EMPTY), DirectoryFileFilter.INSTANCE);
                for (File file : filessss){
                    System.out.println(file.getAbsolutePath());
                }
                System.out.println("-------------------");
                //列出该目录下的后缀名为java文件或者后缀名为class文件的子孙级
                Collection<File> filesssss =  FileUtils.listFiles(new File("F:/My direction/java SE/IO/src/com/yishan"),
                        FileFilterUtils.or(new SuffixFileFilter("java"),new SuffixFileFilter("class")), DirectoryFileFilter.INSTANCE);
                for (File file : filesssss){
                    System.out.println(file.getAbsolutePath());
                }
            }
        }


        package com.yishan.io4;


        import org.apache.commons.io.FileUtils;
        import org.apache.commons.io.LineIterator;

        import java.io.File;
        import java.io.IOException;
        import java.util.List;

        /**
        * 读取文件
        * @author : yishan
        * @date : 2020-09-26 08:14
        */
        public class CIOTest03 {
            public static void main(String[] args) throws IOException {
                //读取文件
                String msg = FileUtils.readFileToString(new File("F:/My direction/java SE/IO/src/com/yishan/io4/emp.txt"),"UTF-8");
                System.out.println(msg);
                byte[] datas = FileUtils.readFileToByteArray(new File("F:/My direction/java SE/IO/src/com/yishan/io4/emp.txt"));
                System.out.println(datas.length);
                //逐行读取
                List<String> msgs = FileUtils.readLines(new File("F:/My direction/java SE/IO/src/com/yishan/io4/emp.txt"),"UTF-8");
                for (String string : msgs){
                    System.out.println(string);
                }
                File file;
                LineIterator it = FileUtils.lineIterator(new File("F:/My direction/java SE/IO/src/com/yishan/io4/emp.txt"),"UTF-8");
                while (it.hasNext()){
                    System.out.println(it.nextLine());
                }
            }
        }



            package com.yishan.io4;


            import org.apache.commons.io.FileUtils;

            import java.io.File;
            import java.io.IOException;
            import java.util.ArrayList;
            import java.util.Collection;
            import java.util.List;

            /**
            * 写出内容
            * @author : yishan
            * @date : 2020-09-26 08:14
            */
            public class CIOTest04 {
                public static void main(String[] args) throws IOException {
                    //写出文件
                    FileUtils.write(new File("happy.sxt"),"开开心心学习\r\n",true);
                    FileUtils.writeStringToFile(new File("happy.sxt"),"学习是辛苦的\r\n","UTF-8",true);
                    FileUtils.writeByteArrayToFile(new File("happy.sxt"),"学习是幸福的\r\n".getBytes("UTF-8"),true);

                    //写出列表
                    List<String> datas = new ArrayList<>();
                    datas.add("哈哈");
                    datas.add("嘻嘻");
                    datas.add("嘿嘿");

                    FileUtils.writeLines(new File("happy.sxt"), datas,"...",true);

                }
            }


            package com.yishan.io4;


            import org.apache.commons.io.FileUtils;
            import org.apache.commons.io.IOUtils;

            import java.io.File;
            import java.io.IOException;
            import java.net.URL;

            /**
            * 拷贝
            * @author : yishan
            * @date : 2020-09-26 08:14
            */
            public class CIOTest05 {
                public static void main(String[] args) throws IOException {
                    //复制文件
                    //FileUtils.copyFile(new File("F:/My direction/java SE/IO/src/com/yishan/io4/emp.txt"),new File("F:/My direction/java SE/IO/src/com/yishan/io4/emp-copy.txt"));
                    //复制文件到目录
                    //FileUtils.copyFileToDirectory(new File("F:/My direction/java SE/IO/src/com/yishan/io4/emp.txt"),new File("F:/My direction/java SE"));
                    //复制目录到目录
                    //FileUtils.copyDirectoryToDirectory(new File("F:/My direction/java SE/IO/src/com/yishan/io4"),new File("F:/My direction/java SE/IO/src/com/yishan/io4-copy"));
                    //复制目录
                    //FileUtils.copyDirectory(new File("F:/My direction/java SE/IO/src/com/yishan/io4"),new File("F:/My direction/java SE/IO/src/com/yishan/io4-copy"));
                    //拷贝URL的内容
                    String url = "https://www.baidu.com";
                    //FileUtils.copyURLToFile(new URL(url),new File("abc"));
                    String datas = IOUtils.toString(new URL(url),"utf-8");
                    System.out.println(datas);
                }
            }
