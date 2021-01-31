---
title: TreeSet的使用和底层实现
date: 2020-08-31 20:48:20
tags: [java基础,TreeSet]
---
TreeSet底层实际是用TreeMap实现的，内部维持了一个简化版的TreeMap,通过key来存储Set的元素。
<!--more-->
TreeSet内部需要对存储的元素进行排序，因此，我们对应的类需要实现Cpmparable接口。这样，才能根据compareTo（）方法比较对象之间的大小，才能进行内部排序。

```java
    package cn.yishan.collection;

    import java.util.Set;
    import java.util.TreeSet;

    /**
    * 测试TreeSet的使用
    * 熟悉Comparable接口
    * @author yishan
    */
    public class TestTreeSet {
        public static void main(String[] args) {
            Set<Integer> set = new TreeSet<>();

            set.add(300);
            set.add(200);
            set.add(100);
            //按照元素递增的方式排序
            for (Integer m :set){
                System.out.println(m);
            }

            Set<Emp2> set2 = new TreeSet<>();
            set2.add(new Emp2(100,"张三",1000));
            set2.add(new Emp2(132,"李四",3000));
            set2.add(new Emp2(43,"王五",5600));
            set2.add(new Emp2(20,"赵六",2300));
            for (Emp2 m :set2){
                System.out.println(m);
            }
        }
    }
```




        class Emp2 implements Comparable<Emp2>{
            int id;
            String name;
            double salary;


```java
        public Emp2(int id, String name, double salary) {
            this.id = id;
            this.name = name;
            this.salary = salary;
        }

        @Override
        public String toString() {
            return "id:" + id + ",name:" + name + ",salary:" + salary;
        }

        @Override
        public int compareTo(Emp2 o) { //负数：小于，0：等于，正数：大于
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
