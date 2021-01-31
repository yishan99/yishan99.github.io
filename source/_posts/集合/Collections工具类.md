---
title: Collections工具类
date: 2020-09-01 18:16:52
tags: [java基础]
---
### Collections工具类
<!--more-->
        package cn.yishan.collection;

```java
    import java.util.ArrayList;
    import java.util.Collections;
    import java.util.List;

    /**
    * Collections工具类的使用
    * Collection是接口，Collections是工具类
    * @author yishan
    */
    public class TestCollections {
        public static void main(String[] args) {
            List<String> list = new ArrayList<>();
            for (int i = 0;i<10;i++){
                list.add("yang:"+i);
            }
            System.out.println(list);
            //随机排列List中的元素
            Collections.shuffle(list);
            System.out.println(list);
            //逆序排列
            Collections.reverse(list);
            System.out.println(list);
            //按照递增的方式排列.自定义的类使用：Comparable接口
            Collections.sort(list);
            System.out.println(list);
            //二分法查找,折半查找
            System.out.println(Collections.binarySearch(list,"yang:1"));
        }
    }
```

