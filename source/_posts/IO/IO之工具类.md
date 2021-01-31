---
title: IO之工具类
date: 2020-09-06 15:31:22
tags: [java基础,IO]
---
### 工具类
<!--more-->
        package com.yishan.io2;

        import java.io.*;

        /**
        * 1.封装拷贝
        * 2.封装释放资源
        * @author : yishan
        * @date : 2020-09-06 15:35
        */
        public class FileUtils {

            public static void main(String[] args) {
                //文件到文件
                try {
                    InputStream is = new FileInputStream("yishan.txt");
                    OutputStream os = new FileOutputStream("yishan-c.txt");
                    copy(is,os);
                } catch (IOException e) {
                    e.printStackTrace();
                }

                //文件到字节数组
                byte[] datas = null;
                try {
                    InputStream is = new FileInputStream("111copy.png");
                    ByteArrayOutputStream os = new ByteArrayOutputStream();
                    copy(is,os);
                    datas = os.toByteArray();
                    System.out.println(datas.length);
                } catch (IOException e) {
                    e.printStackTrace();
                }

                //字节数组到文件
                try {
                    InputStream is = new ByteArrayInputStream(datas);
                    OutputStream os = new FileOutputStream("111copy-c.png");
                    copy(is,os);
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            /**
            * 对接输入输出流
            * @param is
            * @param os
            */
            public static void copy(InputStream is,OutputStream os){
                try {
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
                    close(is,os);
                }

            }

            /**
            * 释放资源
            * @param is
            * @param os
            */
            public static void close(InputStream is,OutputStream os){
                try {
                    if (null!=os){
                        os.close();
                    }
                } catch (IOException e) {
                    e.printStackTrace();
                }

                try {
                    if (null != is){
                        is.close();
                    }
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            /**
            * 释放资源
            * @param ios
            */
            public static void close(Closeable...ios){
                for (Closeable io:ios){
                    try {
                        if (null != io){
                            io.close();
                        }
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }
        }

### try...with...resource 释放资源

        public static void copy2(String srcPath,String destPath){
                //1.创建源
                File src = new File(srcPath);//源头
                File dest = new File(destPath);//目的地
                //2.选择流
                try(InputStream is = new FileInputStream(src);
                    OutputStream os = new FileOutputStream(dest);) {
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
                }
            }


