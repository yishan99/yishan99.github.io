---
title: IO之文件流，字节数组流，对接流！
date: 2020-09-03 18:00:05
tags: [java基础,IO]
---
### 文件流

FileReader:通过字符的方式读取文件，仅适合字符文件

FileWriter:通过字节的方式写出或追加数据到文件中，仅适合字符文件
<!--more-->
### 四个步骤：  分段读取   文件字符输入流

        package com.yishan.io2;

        import java.io.*;

        /**
        * 四个步骤：  分段读取   文件字符输入流
        * 1.创建源
        * 2.选择流
        * 3.操作
        * 4.释放资源
        * @author yishan
        */
        public class IOTest05 {
            public static void main(String[] args) {
                //1.创建源
                File src = new File("F:/My direction/java SE/IO/src/com/yishan/abc.txt");
                //2.选择流
                Reader reader = null;
                try {
                    reader = new FileReader(src);
                    //3.操作（读取）
                    char[] flush = new char[1024];//缓冲容器
                    int len = -1;
                    while ((len=reader.read(flush))!=-1){
                        //字符数组-->字符串
                        String str = new String(flush,0,len);
                        System.out.println(str);
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

### 四个步骤：  分段读取   文件字符输出流

        package com.yishan.io2;

        import java.io.*;

        /**
        * 四个步骤：  分段读取   文件字符输出流
        * 1.创建源
        * 2.选择流
        * 3.操作（写出内容）
        * 4.释放资源
        * @author yishan
        */
        public class IOTest06 {
            public static void main(String[] args) {
                //1.创建源
                File src = new File("dest.txt");
                //2.选择流
                Writer writer = null;
                try {
                    writer = new FileWriter(src);
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
                    writer.append("change!\\r\\n").append("上风上山");
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

### 字节数组输入流

        package com.yishan.io2;

        import java.io.*;

        /**
        * 四个步骤：字节数组输入流
        * 1.创建源:字节数组 不要太大
        * 2.选择流
        * 3.操作
        * 4.释放资源：可以不用处理
        * @author yishan
        */
        public class IOTest07 {
            public static void main(String[] args) {
                //1.创建源
                byte[] src = "talk is cheap show me the code".getBytes();
                //2.选择流
                InputStream is = null;
                try {
                    is = new ByteArrayInputStream(src);
                    //3.操作（读取）
                    byte[] flush = new byte[5];//缓冲容器
                    int len = -1;
                    while ((len=is.read(flush))!=-1){
                        //字节数组-->字符串（解码）
                        String str = new String(flush,0,len);
                        System.out.println(str);
                    }
                    //4.释放资源
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

### 字节数组输出流 ByteArrayOutputStream

        package com.yishan.io2;


        import java.io.*;

        /**
        * 字节数组输出流 ByteArrayOutputStream
        * 1.创建源:内部维护
        * 2.选择流：不关联源
        * 3.操作（写出内容）
        * 4.释放资源：可以不用
        *
        * 获取数据：toByteArray()
        * @author yishan
        */
        public class IOTest08 {
            public static void main(String[] args) {
                //1.创建源
                byte[] dest = null;
                //2.选择流(新增方法)
                ByteArrayOutputStream baos = null;
                try {
                    baos = new ByteArrayOutputStream();
                    //3.操作（写出）
                    String msg = "show me the code";
                    byte[] datas = msg.getBytes();//字符串-->字节数组（编码）
                    baos.write(datas,0,datas.length);
                    baos.flush();
                    //获取数据
                    dest = baos.toByteArray();
                    System.out.println(dest.length+"-->"+new String(dest,0,baos.size()));
                }catch (FileNotFoundException e){
                    e.printStackTrace();
                }catch (IOException e){
                    e.printStackTrace();
                }finally {
                    //4.释放资源
                    try {
                        if (null!=baos){
                            baos.close();
                        }
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }

            }
        }

### 1.图片读取到字节数组

### 2.字节数组写出到文件

        package com.yishan.io2;

        import java.io.*;

        /**
        * 1.图片读取到字节数组
        * 2.字节数组写出到文件
        * @author yishan
        */
        public class IOTest09 {
            public static void main(String[] args) {
                //图片转成字节数组
                byte[] datas =  fileToByteArray("111copy.png");
                System.out.println(datas.length);
                byteArrayToFile(datas,"111copy-byte.png");
            }

            /**
            * 1.图片读取到字节数组
            * a.图片到程序 FileInputStream
            * b.程序到字节数组 ByteArrayOutputStream
            */
            public static byte[] fileToByteArray(String filePath) {
                //1.创建源与目的地
                File src = new File(filePath);
                byte[] dest = null;
                //2.选择流
                InputStream is = null;
                ByteArrayOutputStream baos = null;
                try {
                    is = new FileInputStream(src);
                    baos = new ByteArrayOutputStream();
                    //3.操作（读取）
                    byte[] flush = new byte[1024 * 10];//缓冲容器
                    int len = -1;
                    while ((len = is.read(flush)) != -1) {
                        baos.write(flush, 0, len);//写出到字节数组中
                    }
                    baos.flush();
                    return baos.toByteArray();
                    //4.释放资源
                } catch (FileNotFoundException e) {
                    e.printStackTrace();
                } catch (IOException e) {
                    e.printStackTrace();
                } finally {
                    if (null != is) {
                        try {
                            is.close();
                        } catch (IOException e) {
                            e.printStackTrace();
                        }
                    }
                }
                return null;
            }

            /**
            * 2.字节数组写出到图片
            *  a.字节数组读取到程序 ByteArrayInputStream
            *  b.程序到文件 FileOutputStream
            */
            public static void byteArrayToFile ( byte[] src, String filePath){
                //1.创建源
                File dest = new File(filePath);
                //2.选择流
                InputStream is = null;
                OutputStream os = null;
                try {
                    is = new ByteArrayInputStream(src);
                    os = new FileOutputStream(dest);
                    //3.操作（读取）
                    byte[] flush = new byte[5];//缓冲容器
                    int len = -1;
                    while ((len=is.read(flush))!=-1){
                        os.write(flush,0,len);
                    }
                    os.flush();
                } catch (IOException e) {
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




















