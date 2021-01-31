---
title: TreeMap、Set接口、HashSet常用方法
date: 2020-08-31 17:22:50
tags: [java基础,TreeMap,HashSet]
---

### TreeMap的使用和底层实现
<!--more-->
TreeMap是红黑二叉树的典型实现。

```java
    package cn.yishan.collection;

    import java.util.Map;
    import java.util.TreeMap;

    /**
    * 测试TreeMap的使用
    * @author yishan
    */
    public class TestTreeMap {
        public static void main(String[] args) {
            Map<Integer,String> treemap1 = new TreeMap<>();
            treemap1.put(20,"aa");
            treemap1.put(3,"bb");
            treemap1.put(6,"cc");
            //按照key递增的方式排序
            for (Integer key:treemap1.keySet()){
                System.out.println(key+"---"+treemap1.get(key));
            }

            Map<Emp,String> treemap2 = new TreeMap<>();
            treemap2.put(new Emp(100,"张三",2000),"张三一个人");
            treemap2.put(new Emp(200,"王二",5000),"王二一个人");
            treemap2.put(new Emp(150,"李四",6000),"李四一个人");
            treemap2.put(new Emp(50,"赵柳",6000),"周六一个人");
            for (Emp key:treemap2.keySet()){
                System.out.println(key+"---"+treemap2.get(key));
            }
        }
    }

    class Emp implements Comparable<Emp>{
        int id;
        String name;
        double salary;

        public Emp(int id, String name, double salary) {
            this.id = id;
            this.name = name;
            this.salary = salary;
        }

        @Override
        public String toString() {
            return "id:" + id + ",name:" + name + ",salary:" + salary;
        }

        @Override
        public int compareTo(Emp o) { //负数：小于，0：等于，正数：大于
            if (this.salary>o.salary){
                return 1;
            }else if (this.salary<o.salary){
                return -1;
            }else {
            if (this.id>o.id){
                return 1;
            }else if (this.id<o.id){
                return -1;
            }else{
                return 0;
            }
            }
        }
    }
```

HashMap与HashTable的区别
- 1.HashMap:线程不安全，效率高。允许key或value为null.
- 2.HashTable:线程安全，效率低。不允许key或value为null.
### Set接口

- Set容器的特点：无序、不可重复。无序指Set中的元素没有索引，我们只能遍历查找；不可重复指不允许加入重复的元素。更确切的讲，新元素如果和Set中某个元素通过equals()方法对比为true,则不能加入；甚至，Set中也只能放入一个null元素，不能多个。

- Set常用的实现类有：HashSet、TreeSet等，我们一般使用HashSet
- 不可以使用普通for循环遍历，只能用迭代器和增强for循环进行遍历

```java
    package cn.yishan.collection;

    import java.util.HashSet;
    import java.util.Set;

    /**
    * 测试HashSet的基本用法
    * Set:没有顺序，不可重复
    * List:有顺序，可重复
    * @author yisahn
    */
    public class TestHashSet {
        public static void main(String[] args) {
            Set<String> set1 = new HashSet<>();

            set1.add("aa");
            set1.add("bb");
            set1.add("aa");
            System.out.println(set1);
            set1.remove("bb");
            System.out.println(set1);

            Set<String> set2 = new HashSet<>();
            set2.add("ddd");
            set2.addAll(set1);
            System.out.println(set2);

        }
    }
```

> **Set的底层也是Map,set是作为map的K来存储，所以不可重复**