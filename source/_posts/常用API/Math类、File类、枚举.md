---
title: Math类、File类、枚举
date: 2020-08-14 11:49:16
tags: [java基础]
---
## Math类
<!--more-->
    package cn.yishan.test;

    /**
    * 测试Math的常用方法
    * @author yishan
    */
    public class TestMath {
        public static void main(String[] args) {
            //取整相关操作
            System.out.println(Math.ceil(3.2));//向上取整 4.0
            System.out.println(Math.floor(3.2));//向下取整 3.0
            System.out.println(Math.round(3.2));//四舍五入 3
            System.out.println(Math.round(3.8));//四舍五入 4
            //绝对值、开方、a的b次幂等操作
            System.out.println(Math.abs(-45));//绝对值 45
            System.out.println(Math.sqrt(16));//开方 4.0
            System.out.println(Math.pow(2,3));//8.0
            System.out.println(Math.pow(3,2));//9.0
            //Math中常用的常量
            System.out.println(Math.PI);
            System.out.println(Math.E);
            //随机数[0,1)
            System.out.println(Math.random());
        }
    }

## Random类

    package cn.yishan.test;

    import java.util.Random;

    /**
    * 测试Random的常用方法
    * @author yishan
    */
    public class TestRandom {
        public static void main(String[] args) {
            Random rand = new Random();
            //随机生成[0,1)之间的double类型的数据
            System.out.println(rand.nextDouble());
            //随机生成int类型允许范围之间的整型数据
            System.out.println(rand.nextInt());
            //随机生成[0,1)之间的float类型的数据
            System.out.println(rand.nextFloat());
            //随机生成true或者false
            System.out.println(rand.nextBoolean());
            //随机生成[0,10)之间的int类型的数据
            System.out.println(rand.nextInt(10));
            //随机生成[20,30)之间的int类型的数据
            System.out.println(20 + rand.nextInt(10));
            //随机生成[20,30)之间的int类型的数据(此种方法比较复杂)
            System.out.println(20 + (int)(rand.nextDouble() * 10));

        }
    }

## File类

java.io.File类：代表文件和目录。在开发中，读取文件、生成文件、删除文件、修改文件的属性时会经常用到

    package cn.yishan.test;

    import java.io.File;
    import java.io.IOException;
    import java.util.Date;

    /**
    * 测试File类的基本用法
    * @author yishan
    */
    public class TestFile {
        public static void main(String[] args) throws IOException {
            //File f = new File("d:/a.txt");
            File f = new File("d:\\a.txt");
            System.out.println(f);
            f.renameTo(new File("d:/bb.txt"));

            System.out.println(System.getProperty("user.dir"));
            File f2 = new File("gg.txt");
            //f2.createNewFile();
            System.out.println("File是否存在："+f2.exists());
            System.out.println("File是否是目录："+f2.isDirectory());
            System.out.println("File是否是文件："+f2.isFile());
            System.out.println("File最后修改时间："+new Date(f2.lastModified()));
            System.out.println("File的大小："+f2.length());
            System.out.println("File的文件名："+f2.getName());
            System.out.println("File的目录路径："+f2.getAbsolutePath());
            //f2.delete();//删除文件

            File f3 = new File("d:/电影/华语/大陆");
            //boolean flag = f3.mkdir();//目录结构中有一个不存在，则不会创建整个目录树
            boolean flag = f3.mkdirs();//目录结构中有一个不存在也没关系；创建整个目录树
            System.out.println(flag);//创建失败

        }
    }
## 使用递归算法打印目录树

    package cn.yishan.test;

    import java.io.File;

    /**
    * 使用递归算法打印目录树
    * @author yishan
    */
    public class PrintFileTree {
        public static void main(String[] args) {
            File f = new File("E:\\My_Girl\\blog");
            printFile(f,0);
        }

        static void printFile(File file,int level){
            //输出层数
            for (int i = 0;i < level;i++){
                System.out.print("-");
            }
            System.out.println(file.getName());
            if (file.isDirectory()){
                File[] files = file.listFiles();

                for (File temp:files){
                    printFile(temp,level+1);
                }
            }
        }
    }
## 枚举

- 1.当你需要定义一组常量时，可以使用枚举类型
- 2.尽量不要使用枚举的高级特性，事实上高级特性都可以使用普通类来实现，没有必要引入枚举，增加程序的复杂性

        package cn.yishan.test;

        /**
        * 测试枚举
        * @author yishan
        */
        
        public class TestEnum {

            public static void main(String[] args) {
                System.out.println(Season.SPRING);
                Season a = Season.AUTUMN;
                switch (a){
                    case SPRING:
                        System.out.println("春天来了，播种的季节");
                        break;
                    case SUMMER:
                        System.out.println("夏天来了，游泳的季节");
                        break;
                    case AUTUMN:
                        System.out.println("秋天来了，收获的季节");
                        break;
                    case WINTER:
                        System.out.println("冬天来了，冬眠的季节");
                        break;
                }

            }
        }
        enum Season{
            SPRING,SUMMER,AUTUMN,WINTER
        }
        enum Week{
            星期一,星期二,星期三,星期四,星期五,星期六,星期天
        }












