---
title: IO之流的操作！
date: 2020-09-03 14:59:47
tags: [java基础,IO]
---

> 字节到字符--解码
<!--more-->
> 字符到字节--编码

### 编码： 字符串-->字节

        package com.yishan.io;

        import java.io.UnsupportedEncodingException;

        /**
        * 编码： 字符串-->字节
        * @author yishan
        */
        public class ContentEncode {
            public static void main(String[] args) throws UnsupportedEncodingException {
                String msg = "性命生命使命";
                //编码：字节数组
                byte[] datas = msg.getBytes();  //默认使用工程的字符集
                System.out.println(datas.length);

                //编码：其他字符集
                datas = msg.getBytes("UTF-16LE");
                System.out.println(datas.length);

                datas = msg.getBytes("GBK");
                System.out.println(datas.length);
            }
        }

### 解码：字节-->字符串

        package com.yishan.io;

        import java.io.UnsupportedEncodingException;

        /**
        *解码：字节-->字符串
        * @author yishan
        */
        public class ContentDecode {
            public static void main(String[] args) throws UnsupportedEncodingException {
                String msg = "性命生命使命";
                //编码：字节数组
                byte[] datas = msg.getBytes();  //默认使用工程的字符集

                //解码：字符串
                msg = new String(datas,0,datas.length,"UTF-8");
                System.out.println(msg);

                //乱码：
                //1.字节数不够
                msg = new String(datas,0,datas.length-4,"UTF-8");
                System.out.println(msg);

                msg = new String(datas,0,datas.length-1,"UTF-8");
                System.out.println(msg);

                //2.字符集不统一
                msg = new String(datas,0,datas.length-1,"GBK");
                System.out.println(msg);

            }
        }

### 第一个程序：理解操作步骤

        package com.yishan.io2;

        import java.io.*;

        /**
        * 第一个程序：理解操作步骤
        * 1.创建源
        * 2.选择流
        * 3.操作
        * 4.释放资源
        * @author yishan
        */
        public class IOTest01 {
            public static void main(String[] args) {
                //1.创建源
                File src = new File("F:/My direction/java SE/IO/src/com/yishan/abc.txt");
                //2.选择流
                try {
                    InputStream is = new FileInputStream(src);
                    //3.操作（读取）
                    int data1 = is.read();
                    int data2 = is.read();
                    int data3 = is.read();
                    int data4 = is.read();
                    int data5 = is.read();
                    System.out.println((char) data1);
                    System.out.println((char)data2);
                    System.out.println((char)data3);
                    System.out.println((char)data4);
                    System.out.println((char)data5);
                    //4.释放资源
                    is.close();
                } catch (FileNotFoundException e) {
                    e.printStackTrace();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

### 标准操作步骤

        package com.yishan.io2;

        import java.io.*;

        /**
        * 第一个程序：理解操作步骤  标准操作步骤
        * 1.创建源
        * 2.选择流
        * 3.操作
        * 4.释放资源
        * @author yishan
        */
        public class IOTest02 {
            public static void main(String[] args) {
                //1.创建源
                File src = new File("F:/My direction/java SE/IO/src/com/yishan/abc.txt");
                //2.选择流
                InputStream  is = null;
                try {
                    is = new FileInputStream(src);
                    //3.操作（读取）
                    int temp;
                    while ((temp=is.read())!=-1){
                        System.out.println((char)temp);
                    }
                    //4.释放资源
                } catch (FileNotFoundException e) {
                    e.printStackTrace();
                } catch (IOException e) {
                    e.printStackTrace();
                }finally {

                    try {
                        if (null != is){
                            is.close();
                        }
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }
        }

### 四个步骤：  分段读取    文件字节输入流

        package com.yishan.io2;

        import java.io.*;

        /**
        * 四个步骤：  分段读取
        * 1.创建源
        * 2.选择流
        * 3.操作
        * 4.释放资源
        * @author yishan
        */
        public class IOTest03 {
            public static void main(String[] args) {
                //1.创建源
                File src = new File("F:/My direction/java SE/IO/src/com/yishan/abc.txt");
                //2.选择流
                InputStream is = null;
                try {
                    is = new FileInputStream(src);
                    //3.操作（读取）
                    byte[] flush = new byte[3];//缓冲容器
                    int len = -1;
                    while ((len=is.read(flush))!=-1){
                        //字节数组-->字符串（解码）
                        String str = new String(flush,0,len);
                        System.out.println(str);
                    }
                    //4.释放资源
                } catch (FileNotFoundException e) {
                    e.printStackTrace();
                } catch (IOException e) {
                    e.printStackTrace();
                }finally {
                    if (null != is){
                        try {
                            is.close();
                        } catch (IOException e) {
                            e.printStackTrace();
                        }
                    }
                }
            }

        }

#### 四个步骤：  分段读取   文件字节输出流

        package com.yishan.io2;

        import java.io.*;

        /**
        * 四个步骤：  分段读取   文件字节输出流
        * 1.创建源
        * 2.选择流
        * 3.操作（写出内容）
        * 4.释放资源
        * @author yishan
        */
        public class IOTest04 {
            public static void main(String[] args) {
                //1.创建源
                File src = new File("dest.txt");
                //2.选择流
                OutputStream os = null;
                try {
                    os = new FileOutputStream(src,true);
                    //3.操作（写出）
                    String msg = "change!\r\n";
                    byte[] datas = msg.getBytes();//字符串-->字节数组（编码）
                    os.write(datas,0,datas.length);
                    os.flush();
                }catch (FileNotFoundException e){
                    e.printStackTrace();
                }catch (IOException e){
                    e.printStackTrace();
                }finally {
                    //4.释放资源
                    try {
                        if (null!=os){
                            os.close();
                        }
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }

            }
        }

### 利用文件字节输入流和输出流实现文件的拷贝

        package com.yishan.io2;

        import java.io.*;

        /**
        * 文件拷贝：文件字节输出流和输出流
        * 1.创建源
        * 2.选择流
        * 3.操作（写出内容）
        * 4.释放资源
        * @author yishan
        */
        public class Copy {
            public static void main(String[] args) {
        copy("F:/My direction/java SE/IO/src/com/yishan/io2/Test.java","copy.txt");
            }

            public static void copy(String srcPath,String destPath){
                //1.创建源
                File src = new File(srcPath);//源头
                File dest = new File(destPath);//目的地
                //2.选择流
                InputStream is = null;
                OutputStream os = null;
                try {
                    is = new FileInputStream(src);
                    os = new FileOutputStream(dest);
                    //3.操作（写出）
                    byte[] flush = new byte[3];//缓冲容器
                    int len = -1;
                    while ((len=is.read(flush))!=-1){
                        os.write(flush,0,len);
                    }
                    os.flush();
                }catch (FileNotFoundException e){
                    e.printStackTrace();
                }catch (IOException e){
                    e.printStackTrace();
                }finally {
                    //4.释放资源 先打开的后关闭
                    try {
                        if (null!=os){
                            os.close();
                        }
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
                try {
                    if (null != is){
                        is.close();
                    }
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

