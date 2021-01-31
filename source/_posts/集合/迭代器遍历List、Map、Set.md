---
title: 迭代器遍历List、Map、Set
date: 2020-09-01 17:15:43
tags: [java基础,List,Map,Set]
---
### 增强for循环和迭代器遍历List、Map、Set
<!--more-->


```java
    package cn.yishan.collection;

    import java.util.*;

    /**
    * 测试迭代器遍历List,Set,Map
    * @author yishan
    */
    public class TestIterator {
        public static void main(String[] args) {
            testIteratorList();
            //testIteratorSet();
            //testIteratorMap();
        }

        public static void testIteratorMap(){
            Map<Integer, String> map1 = new HashMap<>();
            map1.put(100,"aa");
            map1.put(200,"bb");
            map1.put(300,"cc");

            //增强for循环遍历Map
            Set<Integer> keyset = map1.keySet();
            for (Integer m:keyset){
                System.out.println(m.intValue()+"--"+map1.get(m));
            }
            System.out.println("------------------------");
            //第一种遍历Map的方式
            Set<Map.Entry<Integer,String>> ss = map1.entrySet();
            for (Iterator<Map.Entry<Integer,String>> iter = ss.iterator();iter.hasNext();){
                Map.Entry<Integer,String> temp = iter.next();

                System.out.println(temp.getKey()+"--"+temp.getValue());
            }
            System.out.println("-------------------------");
            //第二种遍历Map的方式
            Set<Integer> keySet = map1.keySet();
            for (Iterator<Integer> iter = keySet.iterator();iter.hasNext();){
                Integer key = iter.next();
                System.out.println(key+"---"+map1.get(key));
            }
        }

        public static void testIteratorSet(){
            Set<String> set = new HashSet<>();
            set.add("aaa");
            set.add("bbb");
            set.add("ccc");

            //使用增强for循环遍历set
            for (String m:set){
                System.out.println(m);
            }
            System.out.println("---------------------");
            //使用iterator遍历set
            for (Iterator<String> iter = set.iterator();iter.hasNext();){
                String temp = iter.next();
                System.out.println(temp);
            }
        }
        
        public static void testIteratorList(){
            List<String> list = new ArrayList<>();
            list.add("aaa");
            list.add("bbb");
            list.add("ccc");

            //使用增强for循环遍历List
            for (String m:list){
                System.out.println(m);
            }
            System.out.println("-------------------");
            //使用iterator遍历List
            for (Iterator<String> iter = list.iterator();iter.hasNext();){
                String temp = iter.next();
                System.out.println(temp);
            }
        }
    }
```

