---
title: Lambad表达式
date: 2020-12-29 13:51:10
tags: [java基础,Lambad]
---
### Lambda初体验

<!--more-->

```java

        goDrink(new IDrink() {
            @Override
            public void drink() {
                System.out.println("喝水");
            }
        });
        goDrink(()->{
            System.out.println("喝水");
        });

```

#### 函数式编程思想

- 在数学中，函数就是有输入量、输出量的一套计算方案，也就是“拿数据做操作”
- 面向对象思想强调“必须通过对象的形式来做事情”
- 函数式思想则尽量忽略面向对象的复杂语法：“强调做什么，而不是以什么形式去做”

### Lambda测试

##### 示例代码

```java
package com.yishan.myniming;

/**
 * Lambda表达式体验
 * @author : yishan
 * @date : 2020-12-29 14:22
 */
public class Test03 {
    puzblic static void main(String[] args) {

        //匿名内部类实现
        getIHandler(new IHandler() {
            @Override
            public void show() {
                System.out.println("我是匿名内部类的show方法");
            }
        });

        //Lambda表达式实现
        getIHandler(()->{
            System.out.println("我是Lambda表达式的show方法");
        });


    }
    public static void getIHandler(IHandler handler){
        handler.show();
    }
}

interface IHandler{
    void show();
}

```

### 带参数无返回值的Lambda表达式

```java
package com.yishan.myniming;

/**
 * 带参无返回值的Lambda表达式
 * @author : yishan
 * @date : 2020-12-29 14:40
 */
public class Test04 {
    public static void main(String[] args) {
        //带参数的匿名内部类实现
        useDrink(new Drink() {
            @Override
            public void drink(String str) {
                System.out.println("对瓶吹"+str);
            }
        });

        //带参无返回值的Lambda表达式实现
        useDrink((String str)->{
            System.out.println("用"+str+"洗碗！");
        });
    }

    public static void useDrink(Drink drink){
        drink.drink("茅台");
    }
}
@FunctionalInterface
interface Drink{
    void drink(String str);
}

```



### 不带参数有返回值的Lambda表达式

```java
package com.yishan.myniming;

import java.util.Random;

/**
 * 无参有返回值Lambda表达式
 * @author : yishan
 * @date : 2020-12-29 14:56
 */
public class Test05 {
    public static void main(String[] args) {

        //匿名内部类实现
        usegetNum(new Handler() {
            @Override
            public int getNum() {
                Random random = new Random();
                int result = random.nextInt(10) + 1;
                return result;
            }
        });
        //无参有返回值Lambda表达式
        usegetNum(()->{
            Random random = new Random();
            int result = random.nextInt(10) + 1;
            return result;
        });
    }
    public static void usegetNum(Handler handler){
        int num = handler.getNum();
        System.out.println("num = " + num);
    }
}
@FunctionalInterface
interface Handler{
    int getNum();
}
```



### 有参有返回值的Lambda表达式

```java
package com.yishan.mynbl.myniming;

/**
 * 有参有返回值的Lambda表达式
 * @author : yishan
 * @date : 2020-12-29 15:22
 */
public class Test06 {
    public static void main(String[] args) {

        //匿名内部类实现
        useCalc(new Calculator() {
            @Override
            public int calc(int a, int b) {
                return a + b;
            }
        });
        //有参有返回值的Lambda表达式
        useCalc((int a,int b)->{
            return a * b;
        });
    }
    public static void useCalc(Calculator calculator){
        int result = calculator.calc(10,12);
        System.out.println("result = " + result);
    }
}
interface Calculator{
    int calc(int a ,int b);
}

```

### Lambda的省略模式

#### 省略规则

- 参数类型可以省略，但是有多个参数的情况下，不能只省略一个
- 如果参数有且仅有一个，那么小括号可以省略
- 如果代码块的语句只有一条，可以省略大括号和分号，甚至是return

### Lambda表达式和匿名内部类的区别

- 所需类型不同
  - 匿名内部类：可以是接口，也可以是抽象类，还可以是具体类
  - Lambda表达式：只能是接口
- 使用限制不同
  - 如果接口中有且仅有一个抽象方法，可以使用Lambda表达式，也可以使用匿名内部类
  - 如果接口中多于一个抽象方法，只能使用匿名内部类，而不能使用Lambda表达式
- 实现原理不同
  - 匿名内部类：编译之后，产生一个单独的.Class字节码文件
  - Lambda表达式：编译之后，没有一个单独的.Class字节码文件。对应的字节码会在运行的时候动态生成