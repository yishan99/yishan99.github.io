---
title: IO之字符缓冲流！
date: 2020-09-06 19:21:42
tags: [java基础,IO]
---
### 文件字符输入流   加入缓冲流（逐行读取）

- reader.readLine()  逐行读取
<!--more-->
        package com.yishan.io3;

        import java.io.*;

        /**
        * 四个步骤：  分段读取   文件字符输入流   加入缓冲流
        * 1.创建源
        * 2.选择流
        * 3.操作
        * 4.释放资源
        * @author yishan
        */
        public class BufferedTest04 {
            public static void main(String[] args) {
                //1.创建源
                File src = new File("F:/My direction/java SE/IO/src/com/yishan/abc.txt");
                //2.选择流
                BufferedReader reader = null;
                try {
                    reader = new BufferedReader(new FileReader(src));
                    //3.操作（读取）
                    String line = null;
                    while ((line=reader.readLine())!=null){
                        //字符数组-->字符串
                        System.out.println(line);
                    }
                    //4.释放资源
                } catch (FileNotFoundException e) {
                    e.printStackTrace();
                } catch (IOException e) {
                    e.printStackTrace();
                }finally {
                    if (null != reader){
                        try {
                            reader.close();
                        } catch (IOException e) {
                            e.printStackTrace();
                        }
                    }
                }
            }

        }
### 文件字符输出流   加入缓冲流

- 加入了 writer.newLine(); 换行符

        package com.yishan.io3;

        import java.io.*;

        /**
        * 四个步骤：  分段读取   文件字符输出流   加入缓冲流
        * 1.创建源
        * 2.选择流
        * 3.操作（写出内容）
        * 4.释放资源
        * @author yishan
        */
        public class BufferedTest03 {
            public static void main(String[] args) {
                //1.创建源
                File src = new File("dest.txt");
                //2.选择流
                BufferedWriter writer = null;
                try {
                    writer = new BufferedWriter(new FileWriter(src));
                    //3.操作（写出）

                    //写法一：
                    //String msg = "change!\r\n上风上山";
                    //char[] datas = msg.toCharArray();//字符串-->字符数组
                    //writer.write(datas,0,datas.length);
                    //writer.flush();

                    //写法二：
                    //String msg = "change!\\r\\n上风上山";
                    //writer.write(msg);
                    //writer.flush();

                    //写法三：
                    writer.append("change!");
                    writer.newLine();
                    writer.append("上风上山2");
                    writer.flush();


                }catch (FileNotFoundException e){
                    e.printStackTrace();
                }catch (IOException e){
                    e.printStackTrace();
                }finally {
                    //4.释放资源
                    try {
                        if (null!=writer){
                            writer.close();
                        }
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }

            }
        }

### 文件拷贝，加入字符缓冲流，逐行读取

            package com.yishan.io3;

            import java.io.*;

            /**
            * 纯文本的复制
            * 文件拷贝：文件字符输出流和输出流
            * 加入缓冲流
            * 逐行读取
            * 1.创建源
            * 2.选择流
            * 3.操作（写出内容）
            * 4.释放资源
            * @author yishan
            */
            public class CopyTxt {
                public static void main(String[] args) {

            copy("F:/My direction/java SE/IO/src/com/yishan/io2/Test2.java","copy.txt");
                }

                public static void copy(String srcPath,String destPath){
                    //1.创建源
                    File src = new File(srcPath);//源头
                    File dest = new File(destPath);//目的地
                    //2.选择流
                    try (BufferedReader br= new BufferedReader(new FileReader(src));
                    BufferedWriter bw = new BufferedWriter(new FileWriter(dest));){
                        //3.操作（写出）
                        String line = null;
                        while ((line=br.readLine())!=null){//逐行写出
                            bw.write(line);
                            bw.newLine();
                        }
                        bw.flush();
                    }catch (FileNotFoundException e){
                        e.printStackTrace();
                    }catch (IOException e){
                        e.printStackTrace();
                    }
                }
            }

