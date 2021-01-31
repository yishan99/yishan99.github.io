---
title: this、static、静态
date: 2020-08-02 19:45:23
tags: java基础
---
### 对象创建的过程和this的本质
<!--more-->
创建一个对象分为如下四步：
- 1.分配对象空间，并将对象成员变量初始化为0或空
- 2.执行属性值的显式初始化
- 3.执行构造方法
- 4.返回对象的地址给相关的变量

this的本质就是“创建好的对象的地址”，由于在构造方法调用前，对象已经创建。因此，在构造方法中也可以使用this代表“当前对象”。

this最常用的用法：
- 1.在程序中产生二义性之处，用使用this来指明当前对象；普通方法中总是指向调用该方法的对象。构造方法中，this总是指向正要初始化的对象。
- 2.使用this关键字调用重载的构造方法，避免相同的初始化代码。但只能在构造方法中用，并且必须位于构造方法的第一局。
- 3.this不能用于static方法中。
### static 关键字
在类中，用static声明的成员变量为静态成员变量，也称为类变量。类变量的生命周期和类相同，在整个应用程序执行期间都有效。

> static修饰的成员变量和方法，从属于类。普通变量和方法从 属于对象的。
### 代码块
#### 局部代码块
- 位置：方法中定义

- 作用：限定变量的生命周期，及早释放，提高内存的利用率

  ```java
  package com.yishan.daimakuai;
  
  /**
   * 局部代码块
   * @author : yishan
   * @date : 2020-12-26 21:16
   */
  public class Test {
      public static void main(String[] args) {
          {
              int a = 10;
              System.out.println(a);
          }
          //System.out.println(a);
      }
  }
  
  ```

  
#### 构造代码块

- 位置：类中方法外定义

- 特点：每次构造方法执行时，都会执行该代码块中的代码，并且在构造方法前执行

- 作用：将多个构造

- 方法中相同的代码，抽取到构造代码中，提高代码的复用性

  ```java
  package com.yishan.daimakuai;
  
  /**
   * 构造代码块
   * @author : yishan
   * @date : 2020-12-26 21:20
   */
  public class Test1 {
      public static void main(String[] args) {
          Student student = new Student();
          Student student1 = new Student(11);
      }
  }
  class Student{
      {
          System.out.println("我是构造代码块");
      }
      public Student(){
          System.out.println("我是无参构造方法");
      }
      public Student(int a){
          System.out.println("我是带参构造方法。。。。");
      }
  
  }
  
  ```

  

#### 静态初始化快

构造方法用于对象的初始化！静态初始化块，用于类的初始化操作
！在静态初始化块中不能直接访问非static成员。

注意事项：

静态初始化块的执行顺序：
- 1.上溯到object类，先执行object的静态初始化块，再向下执行子类的静态初始化快，知道我们的类的静态初始化块为止。

- 2.构造方法执行顺序和上面的顺序一样。

- 位置：类中方法外定义

- 特点：需要通过static关键字修饰，随着类的加载而加载，并且只执行一次

- 作用：在类加载的时候做一些数据初始化的操作

  ```java
  package com.yishan.daimakuai;
  
  /**
   * 静态代码块
   * @author : yishan
   * @date : 2020-12-26 21:29
   */
  public class Test3 {
      public static void main(String[] args) {
          Teacher t = new Teacher();
          Teacher t1 = new Teacher(1);
  
      }
  }
  class Teacher{
      static{
          System.out.println("我是静态代码块。。");
      }
      public Teacher(){
          System.out.println("我是无参构造方法");
      }
      public Teacher(int a ){
          System.out.println("我是有参的构造方法。。。。。");
      }
  }
  
  ```

  
### 参数传值机制
java中，方法中所有的参数都是“值传递”也就是“传值的是值的副本”。也就是说，我们得到的是“原参数的复印件，而不是原件”。因此，复印件改变不会影响原件。
### package
包机制是java中管理类的重要手段。开发中，我们会遇到大量同名的类，通过包我们很容易对解决类崇明的问题，也可以实现对类的有效管理。包对于类，相当于文件夹对文件的作用。

package的使用有两个要点：
- 1.通常是类的第一句非注释性语句
- 2.域名倒着写即可，再加上模块名，便于内部管理类