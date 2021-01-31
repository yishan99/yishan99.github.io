---
title: Collection中的常用方法
date: 2021-01-03 14:07:15
tags: [java基础,Collection]
---
### Collection集合中的常用方法

<!--more-->

```java
package com.yishan.arrays;

import java.util.ArrayList;
import java.util.Collection;

/**
 * Collection集合中的常用方法
 * @author : yishan
 * @date : 2021-01-03 13:45
 */
public class MyCollectionDemo01 {
    public static void main(String[] args) {
        Collection<String> arrays = new ArrayList<>();
        arrays.add("aaa");
        arrays.add("bbb");
        arrays.add("ccc");
        arrays.add("dddd");
        //Method01(arrays);
        //method02(arrays);
        //method03(arrays);
        //method4(arrays);
        //method05(arrays);
        //method06(arrays);
        //method07(arrays);
    }

    /**
     * 集合的长度，也就是集合中元素的个数
     * @param arrays
     */
    private static void method07(Collection<String> arrays) {
        int size = arrays.size();
        System.out.println("size = " + size);
    }

    /**
     * 判断集合是否为空
     * @param arrays
     */
    private static void method06(Collection<String> arrays) {
        boolean empty = arrays.isEmpty();
        System.out.println("empty = " + empty);
    }

    /**
     * 判断集合中是否存在指定的元素
     * @param arrays
     */
    private static void method05(Collection<String> arrays) {
        boolean contains = arrays.contains("aaa");
        System.out.println("contains = " + contains);
    }

    /**
     * 清空集合元素
     * @param arrays
     */
    private static void method4(Collection<String> arrays) {
        arrays.clear();
        System.out.println("arrays = " + arrays);
    }

    /**
     * 根据条件进行删除元素
     * @param arrays
     */
    private static void method03(Collection<String> arrays) {
        arrays.removeIf((String s)->{
            return s.length() == 3;
        });
        System.out.println("arrays = " + arrays);
    }

    /**
     * 集合删除元素的方法
     * @param arrays
     */
    private static void method02(Collection<String> arrays) {
        //删除指定元素
        arrays.remove("ddd");
        System.out.println("arrays = " + arrays);
    }

    /**
     * 集合添加元素的方法
     * @param arrays
     */
    private static void Method01(Collection<String> arrays) {
        arrays.add("aaa");
        arrays.add("bbb");
        arrays.add("ccc");
        System.out.println("arrays = " + arrays);
    }
}

```

