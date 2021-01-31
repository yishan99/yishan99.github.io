---
title: IO之文件分割！
date: 2020-09-15 21:36:02
tags: [java基础,IO]
---

### 面向过程核心版
<!--more-->
        package com.yishan.io3;

        import java.io.File;
        import java.io.IOException;
        import java.io.RandomAccessFile;

        /**
        * 随机读取和写入流  RandomAccessFile
        * @author : yishan
        * @date : 2020-09-15 14:06
        */
        public class RandTest01 {
            public static void main(String[] args) throws IOException {
                //分多少块
                File src = new File("F:/My direction/java SE/copy.txt");
                //总长度
                long len = src.length();
                //没块大小
                int blockSize = 100;
                //多少块
                int size = (int) Math.ceil(len*1.0/blockSize);
                System.out.println(size);
                //起始位置和大小
                int beginPos = 0;
                int actualSize = (int)(blockSize>len?len:blockSize);
                for(int i = 0;i < size; i++){
                    beginPos = i * blockSize;
                    if(i==size-1){//最后一块
                        actualSize = (int)len;
                    }else{
                        actualSize = blockSize;
                        len -= actualSize;//剩余量
                    }

                    System.out.println(i+"--"+beginPos+"--"+actualSize);
                    split(i,beginPos,actualSize);
                }
            }

            /**
            * 指定第i快的起始位置和实际长度
            * @param i
            * @param beginPos
            * @param actualSize
            * @throws IOException
            */
            //  分块思想：起始、实际大小
            public static void split(int i,int beginPos,int actualSize ) throws IOException {

                RandomAccessFile raf = new RandomAccessFile("F:/My direction/java SE/copy.txt","r" );
                raf.seek(beginPos);
                //读取
                //操作
                byte[] flush = new byte[1024];
                int len = -1;
                while ((len=raf.read(flush))!=-1){
                    if(actualSize>len){
                        System.out.println(new String(flush,0,len));
                    }else{
                        System.out.println(new String(flush,0,actualSize));
                        break;
                    }
                }
                raf.close();



            }


            //指定起始位置，读取剩余所有内容
            public static void test01()throws IOException{
                RandomAccessFile raf = new RandomAccessFile("F:/My direction/java SE/copy.txt","r" );
                //随机读取
                raf.seek(2);
                //读取
                //操作
                byte[] flush = new byte[1024];
                int len = -1;
                while ((len=raf.read(flush))!=-1){
                    System.out.println(new String(flush,0,len));
                }
                raf.close();
            }
        }


### 面向对象终极版（合并流）

        package com.yishan.io3;

        import java.io.*;
        import java.util.ArrayList;
        import java.util.List;
        import java.util.Vector;

        /**
        * 面向对象思想封装 分割
        * @author : yishan
        * @date : 2020-09-15 14:06
        */
        public class SplitFile {
            //源头
            private  File src;
            //目的地（文件夹）
            private  String desrDir;
            //所有分割后的文件存储路径
            private  List<String> destPaths;
            //每块大小
            private int blockSize;
            //块数：多少块
            private int size;

            public SplitFile(String srcPath, String desrDir, int blockSize) {
                this.src = new File(srcPath);
                this.desrDir = desrDir;
                this.blockSize = blockSize;
                this.destPaths = new ArrayList<>();
                //初始化
                init();
            }
            //初始化
            private void init(){
                //总长度
                long len = this.src.length();
                //多少块
                this.size = (int) Math.ceil(len*1.0/blockSize);
                //路径
                for(int i = 0; i < size;i++){
                    this.destPaths.add(this.desrDir+"/"+i+this.src.getName());
                }
            }


            public void split() throws IOException {
                //总长度
                long len = src.length();
                //起始位置和大小
                int beginPos = 0;
                int actualSize = (int)(blockSize>len?len:blockSize);
                for(int i = 0;i < size; i++){
                    beginPos = i * blockSize;
                    if(i==size-1){//最后一块
                        actualSize = (int)len;
                    }else{
                        actualSize = blockSize;
                        len -= actualSize;//剩余量
                    }

                    System.out.println(i+"--"+beginPos+"--"+actualSize);
                    splitDetail(i,beginPos,actualSize);
                }
            }

            /**
            * 指定第i快的起始位置和实际长度
            * @param i
            * @param beginPos
            * @param actualSize
            * @throws IOException
            */
            //  分块思想：起始、实际大小
            private   void splitDetail(int i,int beginPos,int actualSize ) throws IOException {

                RandomAccessFile raf = new RandomAccessFile(this.src,"r" );
                RandomAccessFile raf2 = new RandomAccessFile(this.destPaths.get(i),"rw" );
                raf.seek(beginPos);
                //读取
                //操作
                byte[] flush = new byte[1024];
                int len = -1;
                while ((len=raf.read(flush))!=-1){
                    if(actualSize>len){
                        raf2.write(flush,0,len);
                        actualSize-=len;
                    }else{
                        raf2.write(flush,0,actualSize);
                        break;
                    }
                }
                raf.close();



            }

            /**
            * 文件的合并
            */
            public void merge(String destPath) throws IOException {
                //输出流
                OutputStream os = new BufferedOutputStream(new FileOutputStream(destPath,true));
                Vector<InputStream> vi = new Vector<>();
                SequenceInputStream sis = null;
                //输入流
                for(int i=0;i<destPaths.size();i++){
                    vi.add(new BufferedInputStream(new FileInputStream(destPaths.get(i))));
                }
                sis = new SequenceInputStream(vi.elements());
                //拷贝
                //操作（分段读取）
                byte[] flush = new byte[1024];
                int len = -1;
                while((len=sis.read(flush))!=-1){
                    os.write(flush,0,len);
                }
                os.flush();
                sis.close();
                os.close();

            }
            public static void main(String[] args) throws IOException {
                SplitFile sf= new SplitFile("F:\\My direction\\java SE\\IO\\src\\com\\yishan\\io3\\SplitFile.java", "dest", 1024);
                sf.split();
                sf.merge("aaa.java");
            }


        }
