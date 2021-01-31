---
title: IO之数据流、对象流、打印流！
date: 2020-09-08 11:47:44
tags: [java基础,IO]
---

### 数据流
<!--more-->
        package com.yishan.io3;

        import java.io.*;

        /**
        * 数据流
        * 1.写出后读取
        * 2.读取的顺序与写出保持一致
        *
        * DataOutputStream
        * DataInputStream
        * @author : yishan
        * @date : 2020-09-08 11:27
        */
        public class DataTest {

            public static void main(String[] args) throws IOException {
                //写出
                ByteArrayOutputStream baos = new ByteArrayOutputStream();
                DataOutputStream dos = new DataOutputStream(new BufferedOutputStream(baos));
                //操作数据类型+数据
                dos.writeUTF("编程");
                dos.writeInt(56);
                dos.writeChar('a');
                dos.flush();
                byte[] datas = baos.toByteArray();

                //读取
                DataInputStream dis = new DataInputStream(new BufferedInputStream(new ByteArrayInputStream(datas)));
                String msg = dis.readUTF();
                int aa = dis.readInt();
                char bb = dis.readChar();
                System.out.println(aa);
            }

        }

### 对象流（序列化和反序列化）

- 1.先写出后读取
- 2.读取的顺序和写出的顺序保持一致
- 3.不是所有的对象都可以实现序列化，必须实现Serialization接口

        package com.yishan.io3;


        import javax.xml.crypto.Data;
        import java.io.*;
        import java.util.Date;

        /**
        * 对象流
        * 1.写出后读取
        * 2.读取的顺序与写出保持一致
        * 3.不是所有的对象都可以序列化Serializable
        *
        * ObjectOutputStream
        * ObjectInputStream
        * @author : yishan
        * @date : 2020-09-08 11:27
        */
        public class ObjectTest {

            public static void main(String[] args) throws IOException, ClassNotFoundException {
                //写出 -- 序列化
                ByteArrayOutputStream baos = new ByteArrayOutputStream();
                ObjectOutputStream oos = new ObjectOutputStream(new BufferedOutputStream(baos));
                //操作数据类型+数据
                oos.writeUTF("编程");
                oos.writeInt(56);
                oos.writeChar('a');
                //加入对象
                oos.writeObject("编程思想！");
                oos.writeObject(new Date());
                Employee emp = new Employee("马云",400);
                oos.writeObject(emp);
                oos.flush();
                byte[] datas = baos.toByteArray();

                //读取--反序列化
                ObjectInputStream ois = new ObjectInputStream(new BufferedInputStream(new ByteArrayInputStream(datas)));
                String msg = ois.readUTF();
                int aa = ois.readInt();
                char bb = ois.readChar();
                System.out.println(aa);
                Object str = ois.readObject();
                Object data = ois.readObject();
                Object employee = ois.readObject();

                if (str instanceof String){
                    String strObj = (String)str;
                    System.out.println(strObj);
                }
                if (data instanceof Data){
                    Data dataObj = (Data)data;
                    System.out.println(dataObj);
                }
                if (employee instanceof Employee){
                    Employee empObj = (Employee)employee;
                    System.out.println(empObj.getName()+"--"+((Employee) employee).getSalary());
                }
            }

        }

        //javaBean   封装数据
        class Employee implements java.io.Serializable{
            private transient String name;//该数据不需要序列化
            private double salary;

            public Employee(){
            }

            public Employee(String name, double salary) {
                this.name = name;
                this.salary = salary;
            }

            public String getName() {
                return name;
            }

            public void setName(String name) {
                this.name = name;
            }

            public double getSalary() {
                return salary;
            }

            public void setSalary(double salary) {
                this.salary = salary;
            }
        }

### 打印流 PrintStream

        package com.yishan.io3;

        import java.io.*;

        /**
        * 打印流  PrintStream
        * @author : yishan
        * @date : 2020-09-15 13:44
        */
        public class PrintTest01 {
            public static void main(String[] args) throws FileNotFoundException {
                //打印流 System.out
                PrintStream ps = System.out;
                ps.println("打印流");
                ps.println(true);

                ps = new PrintStream(new BufferedOutputStream(new FileOutputStream("print.txt")),true);
                ps.println("打印流");
                ps.println(true);
                ps.close();
                //重定向输出端
                System.setOut(ps);
                System.out.println("change!");
                //重定向回控制台
                System.setOut(new PrintStream(new BufferedOutputStream(new FileOutputStream(FileDescriptor.out)),true));
                System.out.println("back!");
            }
        }


### 打印流 PrintWriter

        package com.yishan.io3;

        import java.io.*;

        /**
        * 打印流  PrintWriter
        * @author : yishan
        * @date : 2020-09-15 13:44
        */
        public class PrintTest02 {
            public static void main(String[] args) throws FileNotFoundException {

                PrintWriter pw = new PrintWriter(new BufferedOutputStream(new FileOutputStream("print.txt")), true);
                pw.println("打印流");
                pw.println(true);
                pw.close();
            }
        }
