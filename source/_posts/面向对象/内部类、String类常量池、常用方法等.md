---
title: 内部类、String类常量池、常用方法等
date: 2020-08-10 20:00:36
tags: [java基础]
---
## 内部类
<!--more-->

### 成员内部类

- 内部类就是在一个类中定义一个类。比如：在一个A类的内部定义一个B类，B类就被称为内部类
- 按照内部类在类中定义的位置不同，可以分为
  - 在类的成员位置：成员内部类
  - 在类的局部位置：局部内部类

##### 内部类的访问特点

- 内部类可以直接访问外部类的成员，包括私有
- 外部类要访问内部类的成员，必须创建对象

##### 成员内部类

- 普通的成员内部类
- 私有的成员内部类:private
  - 私有成员内部类访问：在自己所在的外部类中创建对象访问
- 静态的成员内部类：static
  - 静态成员内部类访问格式：外部类名.内部类名 对象名 = new 外部类名.内部类名();
  - 静态成员内部类的静态方法：外部类名.内部类名.方法名();

> ##### 示例代码
>
> ```
> package com.yishan.mynbl;
> 
> /**
>  * 成员内部类
>  *      普通的成员内部类
>  *      私有的成员内部类
>  *      静态的成员内部类
>  * @author : yishan
>  * @date : 2020-12-28 23:47
>  */
> public class test01 {
>     public static void main(String[] args) {
>         System.out.println("----普通的成员内部类----");
>         Animal.Dog dog = new Animal().new Dog();
>         dog.eat();
>         System.out.println("dog.age = " + dog.age);
>         System.out.println("-------私有的成员内部类-------");
>         Animal animal = new Animal();
>         animal.showCat();
>         System.out.println("------静态的成员内部类------");
>         Animal.Fish fish = new Animal.Fish();
>         fish.eat();
>     }
> }
> class Animal{
>     int age = 13;
>     public void show(){
>         //外部类访问内部类，需要创建对象
>         Dog dog = new Dog();
>         System.out.println("dog.age = " + dog.age);
>         dog.eat();
> 
> 
>     }
> 
>     /**
>      *  普通的成员内部类
>      */
>     class Dog{
>         int age = 12;
>         //内部类访问外部类，可以直接访问
>         void eat(){
>             System.out.println("吃骨头");
>         }
> 
>     }
> 
>     /**
>      * 私有的成员内部类
>      */
>     private class Cat{
>         void eat(){
>             System.out.println("吃鱼");
>         }
>     }
>     void showCat(){
>         Cat cat = new Cat();
>         cat.eat();
>     }
> 
>     /**
>      * 静态的成员内部类
>      */
>     static class Fish{
>         void eat(){
>             System.out.println("我是鱼");
>         }
>     }
> 
> }
> 
> ```

### 局部内部类

- 局部内部类是在方法中定义的类，所以外界是无法直接使用，需要在方法内部创建对象并使用该类可以直接访问外部类的成员，也可以访问方法内的局部变量

### 匿名内部类

- 匿名内部类式将（继承/实现）（方法重写）（创建对象）三个步骤，放在了一步进行

> #### 示例代码
>
> ```java
> package com.yishan.myniming;
> 
> /**
>  * 匿名内部类
>  * @author : yishan
>  * @date : 2020-12-29 11:15
>  */
> public class Test01 {
>     public static void main(String[] args) {
>         System.out.println("---标准调用---");
>         DogImpl dog = new DogImpl();
>         dog.eat();
>         System.out.println("---匿名内部类---");
> 
>         //匿名内部类
>         IEat eatAll = new IEat(){
> 
>             @Override
>             public void eat() {
>                 System.out.println("猫吃鱼！");
>             }
> 
>             @Override
>             public void drink() {
>                 System.out.println("喝牛奶");
>             }
>         };
>         eatAll.eat();
>         eatAll.drink();
>     }
> }
> 
> interface IEat{
>     void eat();
>     void drink();
> }
> class DogImpl implements IEat{
> 
>     @Override
>     public void eat() {
>         System.out.println("狗啃骨头！");
>     }
> 
>     @Override
>     public void drink() {
>         System.out.println("喝水");
>     }
> }
> 
> ```
>
> 

### 局部内部类
服务于方法 用的很少
## String基础
>测试String类
>
> package cn.yishan.oop;

```java
public class TestString {
    public static void main(String[] args) {
        String str =  "abc";
        String str2 = new String("def");
        String str3 = "abc" + "defgh";
        String str4 = "11" + 20;
        String str10 = "yishan";
        String str11 = "yishan";
        String str12 = new String("yishan");

        System.out.println(str10 == str11);
        System.out.println(str11 == str12);

        //通常比较字符串时，使用equals
        System.out.println(str11.equals(str12));
    }
}

public class TestString2 {
    public static void main(String[] args) {
        String s1 = "core java";
        String s2 = "Core java";
        //提取下标为3的字符
        System.out.println(s1.charAt(3));
        //字符串的长度
        System.out.println(s2.length());
        //比较两个字符串是否相等
        System.out.println(s1.equals(s2));
        //比较两个字符串（忽略大小写）
        System.out.println(s1.equalsIgnoreCase(s2));
        //字符串s1中是否包含java
        System.out.println(s1.indexOf("java"));
        //将s1中的空格替换为&
        String s = s1.replace(" ","&");
        System.out.println(s);
    }
}

public class TestString3 {
    public static void main(String[] args) {
        String s = "";
        String s1 = "How are you?";
        //是否以How开头
        System.out.println(s1.startsWith("How"));
        //是否以you结尾
        System.out.println(s1.endsWith("you"));
        //提取子字符串：从下标为4的开始到字符串结尾为止
        s = s1.substring(4);
        System.out.println(s);
        //提取子字符串：下标[4,7)  不包括7
        s = s1.substring(4,7);
        System.out.println(s);
        //转小写
        s = s1.toLowerCase();
        System.out.println(s);
        //转大写
        s = s1.toUpperCase();
        System.out.println(s);

        String s2 = " How old are you!! ";
        //去除字符串首尾的空格。注意：中间的空格不能去除
        s = s2.trim();
        System.out.println(s);
        //因为String是不可变字符串，所以s2不变
        System.out.println(s2);
    }
}
```

