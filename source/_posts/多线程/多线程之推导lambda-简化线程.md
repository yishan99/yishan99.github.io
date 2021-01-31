---
title: '多线程之推导lambda,简化线程'
date: 2020-10-13 15:09:15
tags: [java基础,多线程]
---

### lambda表达式

<!--more-->
        package com.yishan.thread;

```java
    /**
    * lambda表达式：用于简化简单的线程，只能是一个方法
    * @author : yishan
    * @date : 2020-09-26 16:37
    */
    public class LambdaThread  {
        static class Test implements Runnable {

            //静态内部类
            public void run() {
                for (int i = 0; i < 20; i++) {
                    System.out.println("一遍听歌");
                }
            }

        }

        public static void main(String[] args) {

            //局部内部类
            class Test2 implements Runnable {
                public void run() {
                    for (int i = 0; i < 20; i++) {
                        System.out.println("一遍听歌");
                    }
                }
            }
            new Thread(new Test2()).start();

            //匿名内部类 必须借助接口或者父类
            new Thread(new Runnable(){
                public void run() {
                    for (int i = 0; i < 20; i++) {
                        System.out.println("一遍听歌");
                    }
                }
            }).start();

            //jdk8简化  lambda表达式：用于简化简单的线程，只能是一个方法
            new Thread(()->{
                for (int i = 0; i < 20; i++) {
                    System.out.println("一遍听歌");
                }
            }
            ).start();

        }
    }
```

#### lambda推导

        package com.yishan.thread;
    
        /**
        * lambda推导
        * @author : yishan
        * @date : 2020-10-13 15:21
        */
        public class LambdaTest01 {
            //静态内部类
            static class Like2 implements ILike{
    
                @Override
                public void lambda() {
                    System.out.println("i like lambda2");
                }
            }
            public static void main(String[] args) {
                ILike like = new Like();
    
                like.lambda();
                like = new Like2();
                like.lambda();
    
                //方法内部类
                class Like3 implements ILike{
    
                    @Override
                    public void lambda() {
                        System.out.println("i like lambda3");
                    }
                }
                like = new Like3();
                like.lambda();
    
                //匿名内部类
                like = new Like(){
                    public void lambda() {
                        System.out.println("i like lambda4");
                    }
                };
                like.lambda();
    
                //lambda
                like = ()->{
                    System.out.println("i like lambda5");
                };
                like.lambda();
            }
        }
    
        interface ILike{
            void lambda();
        }
    
        //外部类
        class Like implements ILike{
    
            @Override
            public void lambda() {
                System.out.println("i like lambda");
            }
        }

### lambda推导 + 参数

        package com.yishan.thread;
    
        /**
        * lambda推导 + 参数
        * @author : yishan
        * @date : 2020-10-13 15:21
        */
        public class LambdaTest02 {
            public static void main(String[] args) {
                ILove love = (int a ) -> {
                    System.out.println("i like lambda"+"-->"+a);
                };
                love.lambda(100);


                love = ( a ) -> {
                    System.out.println("i like lambda"+"-->"+a);
                };
                love.lambda(50);


                love =  a  -> {
                    System.out.println("i like lambda"+"-->"+a);
                };
                love.lambda(5);


                love =  a -> System.out.println("i like lambda"+"-->"+a);
                love.lambda(0);
            }
        }
    
        interface ILove{
            void lambda(int a );
        }
    
        //外部类
        class Love implements ILove{
    
            @Override
            public void lambda(int a ) {
                System.out.println("i like lambda"+"-->"+a);
            }
        }

### lambda推导 + 参数 + 返回值

        package com.yishan.thread;
    
        /**
        * lambda推导 + 参数 + 返回值
        * @author : yishan
        * @date : 2020-10-13 15:21
        */
        public class LambdaTest03 {
            public static void main(String[] args) {
                IInterest interest = (int a, int c )-> {
                    System.out.println("i like lambda"+"-->"+(a+c));
                    return a+c;
                };
                interest.lambda(100,200);
    
                interest = ( a,  c )-> {
                    System.out.println("i like lambda"+"-->"+(a+c));
                    return a+c;
                };
                interest.lambda(200,200);
            }
    
            interface IInterest{
                int lambda(int a,int b);
            }
    
            //外部类
            class Interest implements IInterest{
    
                @Override
                public int lambda(int a, int c ) {
                    System.out.println("i like lambda"+"-->"+(a+c));
                    return a+c;
                }
            }
        }