---
title: IO之字节缓冲流！
date: 2020-09-06 17:21:03
tags: [java基础,IO]
---
### 字节缓冲流（提高性能）

- is = new BufferedInputStream(new FileInputStream(src));
- os = new BufferedOutputStream(new FileOutputStream(src));
<!--more-->

        package com.yishan.io3;

        import java.io.*;

        /**
        * 四个步骤：  分段读取   文件字节输入流   加入缓冲流
        * 1.创建源
        * 2.选择流
        * 3.操作
        * 4.释放资源
        * @author yishan
        */
        public class BufferedTest01 {
            public static void main(String[] args) {

                //1.创建源
                File src = new File("F:/My direction/java SE/IO/src/com/yishan/abc.txt");
                //2.选择流
                InputStream is = null;
                try {
                    is = new BufferedInputStream(new FileInputStream(src));
                    //3.操作（读取）
                    byte[] flush = new byte[1024];//缓冲容器
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
                    try{
                        if (null!=is){
                            is.close();
                        }
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }

            }

            public static void Test1(){
                //1.创建源
                File src = new File("F:/My direction/java SE/IO/src/com/yishan/abc.txt");
                //2.选择流
                InputStream is = null;
                BufferedInputStream bis = null;
                try {
                    is = new FileInputStream(src);
                    bis = new BufferedInputStream(bis);
                    //3.操作（读取）
                    byte[] flush = new byte[1024];//缓冲容器
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
                    try{
                        if (null!=is){
                            is.close();
                        }
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                    try{
                        if (null!=bis){
                            bis.close();
                        }
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }
        }