---
title: 手工实现ArrayList
date: 2020-08-15 20:00:22
tags: [java基础,ArrayList]
---
## 手工实现ArrayList01_最简化的方式
<!--more-->
        package cn.yishan.mycollection;

```java
    /**
    * 自定义实现一个ArrayList,体会底层原理
    * @author yishan
    */
    public class SxtArrayList01 {
        private Object[] elementData;
        private int size;

        private static final int DEFAULT_CAPACITY = 10;

        public SxtArrayList01(){
        elementData = new Object[DEFAULT_CAPACITY];
        }
        public SxtArrayList01(int capacity){
            elementData = new Object[capacity];
        }

        public void add(Object obj){
            elementData[size++] = obj;
        }

        @Override
        public String toString() {
            StringBuilder sb = new StringBuilder();

            sb.append("[");
            for (int i = 0;i < size;i++){
                sb.append(elementData[i]+",");
            }
            sb.setCharAt(sb.length()-1,']');
            return sb.toString();
        }

        public static void main(String[] args) {
            SxtArrayList01 s1 = new SxtArrayList01(20);
            s1.add("aa");
            s1.add("bb");
            System.out.println(s1);
        }
    }
```

## 手工实现ArrayList02_增加泛型

```java
    package cn.yishan.mycollection;

    /**
    * 自定义实现一个ArrayList,体会底层原理
    * 增加泛型
    * @author yishan
    */
    public class SxtArrayList02<E> {
        private Object[] elementData;
        private int size;

        private static final int DEFAULT_CAPACITY = 10;

        public SxtArrayList02(){
        elementData = new Object[DEFAULT_CAPACITY];
        }
        public SxtArrayList02(int capacity){
            elementData = new Object[capacity];
        }

        public void add(E element){
            elementData[size++] = element;
        }

        @Override
        public String toString() {
            StringBuilder sb = new StringBuilder();

            sb.append("[");
            for (int i = 0;i < size;i++){
                sb.append(elementData[i]+",");
            }
            sb.setCharAt(sb.length()-1,']');
            return sb.toString();
        }

        public static void main(String[] args) {
            SxtArrayList02 s1 = new SxtArrayList02(20);
            s1.add("aa");
            s1.add("bb");
            System.out.println(s1);
        }
    }
```

## 手工实现ArrayList03_增加数组扩容

```java
    package cn.yishan.mycollection;

    /**
    * 自定义实现一个ArrayList,体会底层原理
    * 增加数组扩容
    * @author yishan
    */
    public class SxtArrayList03<E> {
        private Object[] elementData;
        private int size;

        private static final int DEFAULT_CAPACITY = 10;

        public SxtArrayList03(){
        elementData = new Object[DEFAULT_CAPACITY];
        }
        public SxtArrayList03(int capacity){
            elementData = new Object[capacity];
        }

        public void add(E element){
            //什么时候扩容？
            if (size == elementData.length){
                //扩容操作
                Object[] newArray = new Object[elementData.length + (elementData.length>>1)];//10 --> 10+5

                System.arraycopy(elementData,0,newArray,0,elementData.length);

                elementData = newArray;
            }
            elementData[size++] = element;
        }

        @Override
        public String toString() {
            StringBuilder sb = new StringBuilder();

            sb.append("[");
            for (int i = 0;i < size;i++){
                sb.append(elementData[i]+",");
            }
            sb.setCharAt(sb.length()-1,']');
            return sb.toString();
        }

        public static void main(String[] args) {
            SxtArrayList03 s1 = new SxtArrayList03(20);

            for (int i=0;i<40;i++){
                s1.add("山"+i);
            }
            System.out.println(s1);
        }
    }
```
## 手工实现ArrayList04_索引越界问题_set和get方法

```java
    package cn.yishan.mycollection;

    /**
    * 自定义实现一个ArrayList,体会底层原理
    * 增加set和get方法
    * 增加数组边界的检查
    * @author yishan
    */
    public class SxtArrayList04<E> {
        private Object[] elementData;
        private int size;

        private static final int DEFAULT_CAPACITY = 10;

        public SxtArrayList04(){
            elementData = new Object[DEFAULT_CAPACITY];
        }
        public SxtArrayList04(int capacity){

            if (capacity<0){
                throw new RuntimeException("容器的容量不能为负数");
            }else if (capacity == 0){
                elementData = new Object[DEFAULT_CAPACITY];
            }else {
                elementData = new Object[capacity];
            }
        }

        public void add(E element){
            //什么时候扩容？
            if (size == elementData.length){
                //扩容操作
                Object[] newArray = new Object[elementData.length + (elementData.length>>1)];//10 --> 10+5

                System.arraycopy(elementData,0,newArray,0,elementData.length);

                elementData = newArray;
            }
            elementData[size++] = element;
        }

        public E get(int index){
            checkRange(index);
            return (E)elementData[index];
        }

        public void set(E element,int index){
            checkRange(index);
            elementData[index] = element;
        }

        public void checkRange(int index){
            //索引合法判断[0,size）
            if (index < 0 || index > size - 1){
                //不合法
                throw new RuntimeException("索引不合法：" + index);
            }
        }
        @Override
        public String toString() {
            StringBuilder sb = new StringBuilder();

            sb.append("[");
            for (int i = 0;i < size;i++){
                sb.append(elementData[i]+",");
            }
            sb.setCharAt(sb.length()-1,']');
            return sb.toString();
        }

        public static void main(String[] args) {
            SxtArrayList04 s1 = new SxtArrayList04(20);

            for (int i=0;i<40;i++){
                s1.add("山"+i);
            }
            s1.set("hhh",10);

            System.out.println(s1);
            System.out.println(s1.get(39));
        }
    }
```
## 手工实现ArrayList05_增加remove方法

```java
    package cn.yishan.mycollection;

    /**
    * 自定义实现一个ArrayList,体会底层原理
    * 增加remove方法
    * @author yishan
    */
    public class SxtArrayList05<E> {
        private Object[] elementData;
        private int size;

        private static final int DEFAULT_CAPACITY = 10;

        public SxtArrayList05(){
            elementData = new Object[DEFAULT_CAPACITY];
        }
        public SxtArrayList05(int capacity){

            if (capacity<0){
                throw new RuntimeException("容器的容量不能为负数");
            }else if (capacity == 0){
                elementData = new Object[DEFAULT_CAPACITY];
            }else {
                elementData = new Object[capacity];
            }
        }

        public int size(){
            return size;
        }

        public boolean isEmpty(){
            return size==0?true:false;
        }

        public void add(E element){
            //什么时候扩容？
            if (size == elementData.length){
                //扩容操作
                Object[] newArray = new Object[elementData.length + (elementData.length>>1)];//10 --> 10+5

                System.arraycopy(elementData,0,newArray,0,elementData.length);

                elementData = newArray;
            }
            elementData[size++] = element;
        }

        public E get(int index){
            checkRange(index);
            return (E)elementData[index];
        }

        public void set(E element,int index){
            checkRange(index);
            elementData[index] = element;
        }

        public void checkRange(int index){
            //索引合法判断[0,size）
            if (index < 0 || index > size - 1){
                //不合法
                throw new RuntimeException("索引不合法：" + index);
            }
        }

        public void remove (E element){
            //element,将它和所有元素挨个比较，获得第一个比较为true的，返回
            for (int i = 0;i < size;i++){
                if (element.equals(get(i))){//容器中所有的比较操作都是equals，不是==
                    //将该元素从此处移除
                    remove(i);
                }
            }

        }

        public void remove(int index){
            int numMoved = elementData.length-index-1;
            if (numMoved>0){
                System.arraycopy(elementData,index + 1,elementData,index,numMoved);
            }
            elementData[--size] = null;
        }
        @Override
        public String toString() {
            StringBuilder sb = new StringBuilder();

            sb.append("[");
            for (int i = 0;i < size;i++){
                sb.append(elementData[i]+",");
            }
            sb.setCharAt(sb.length()-1,']');
            return sb.toString();
        }

        public static void main(String[] args) {
            SxtArrayList05 s1 = new SxtArrayList05(20);

            for (int i=0;i<40;i++){
                s1.add("山"+i);
            }
            s1.set("hhh",10);

            System.out.println(s1);
            System.out.println(s1.get(39));
            s1.remove(3);
            s1.remove("山2");
            System.out.println(s1);
            System.out.println(s1.size);
            System.out.println(s1.isEmpty());
        }
    }
```

