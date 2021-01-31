---
title: IO之转换流、字符集
date: 2020-09-08 10:02:07
tags: [java基础,IO]
---
InputStreamReader/OutputStreamWriter:是字节流与字符流之间的桥梁，能将字节流转换为字符流，并且能为字节流指定字符集，可处理一个个的字符
<!--more-->

### InputStreamReader

        package com.yishan.io3;

        import java.io.*;

        /**
        * 转换流：InputStreamReader   OutputStreamWriter
        * 1.以字符集的形式操作字节流（纯文本的）
        * 2.指定字符集
        * @author : yishan
        * @date : 2020-09-08 10:28
        */
        public class ConvertTest {
            public static void main(String[] args) {
                //操作System.in 和 System.out
                try(
                    BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
                    BufferedWriter writer = new BufferedWriter(new OutputStreamWriter(System.out));) {
                //循环获取键盘的输入（exit退出），输出此内容
                    String msg = "";
                    while (!msg.equals("exit")){
                        msg = reader.readLine();//循环读取
                        writer.write(msg);//循环写出
                        writer.newLine();
                        writer.flush();//强制刷新
                    }
                }catch (IOException e){
                    System.out.println("操作异常！");
                }
            }
        }



        package com.yishan.io3;

        import java.io.*;
        import java.net.URL;

        /**
        * 转换流：InputStreamReader   OutputStreamWriter
        * 1.以字符集的形式操作字节流（纯文本的）
        * 2.指定字符集
        * @author : yishan
        * @date : 2020-09-08 10:28
        */
        public class ConvertTest02 {
            public static void main(String[] args) {
                //操作网络流 下载百度的源代码
                Reader in;
                try(BufferedReader reader =new BufferedReader( new InputStreamReader(new URL("http://www.baidu.com").openStream(),"UTF-8"));){
                    String msg;
                    while ((msg=reader.readLine())!=null){
                        System.out.print(msg);
                    }
                }catch (IOException e){
                    System.out.println("操作异常！");
                }
            }

            public static void test2(){
                //操作网络流 下载百度的源代码
                try(InputStreamReader is = new InputStreamReader(new URL("http://www.baidu.com").openStream(),"UTF-8");){
                    int temp;
                    while ((temp=is.read())!=-1){
                        System.out.print((char) temp);
                    }
                }catch (IOException e){
                    System.out.println("操作异常！");
                }
            }
            public static void test1(){
                //操作网络流 下载百度的源代码
                try(InputStream is = new URL("http://www.baidu.com").openStream();){
                    int temp;
                    while ((temp=is.read())!=-1){
                        System.out.print((char) temp);
                    }
                }catch (IOException e){
                    System.out.println("操作异常！");
                }
            }
        }


### OutputStreamWriter

        package com.yishan.io3;

        import java.io.*;
        import java.net.URL;

        /**
        * @author : yishan
        * @date : 2020-09-08 11:11
        */
        public class ConvertTest03 {
            public static void main(String[] args) {
                try(BufferedReader reader =
                            new BufferedReader
                                    ( new InputStreamReader
                                            (new URL("http://www.baidu.com").openStream(),"UTF-8"));){
                    BufferedWriter writer =
                            new BufferedWriter(
                                    new OutputStreamWriter(
                                            new FileOutputStream("baidu.html")
                                    )
                            );
                    String msg;
                    while ((msg=reader.readLine())!=null){
                        writer.write(msg);//字符集不统一出现乱码
                        writer.newLine();
                    }
                    writer.flush();
                }catch (IOException e){
                    System.out.println("操作异常！");
                }
            }
        }


