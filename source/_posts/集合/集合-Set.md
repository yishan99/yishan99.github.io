---
title: 集合-Set
date: 2021-01-04 18:37:40
tags: [java基础,Set]
---
### Set集合

<!--more-->

##### Set集合的概述和特点

- 不可以存储重复元素
- 没有索引,不能使用普通for循环遍历

##### Set集合的使用

```java
package com.yishan.myset;

import java.util.Iterator;
import java.util.Set;
import java.util.TreeSet;

/**
 * set集合
 * 1.不能重复
 * 2.有顺序
 * 3.没有索引，只能通过增强for和迭代器遍历
 * @author : yishan
 * @date : 2021-01-04 15:43
 */
public class Test01 {
    public static void main(String[] args) {
        Set<String> set = new TreeSet<>();
        set.add("aaa");
        set.add("ccc");
        set.add("aaa");
        set.add("bbb");

        Iterator<String> iterator = set.iterator();
        while(iterator.hasNext()){
            String s = iterator.next();
            System.out.println("s = " + s);
        }
        System.out.println("------------------------");
        for (String s : set) {
            System.out.println("s = " + s);
        }
    }
}
```

### TreeSet集合

##### TreeSet集合概述和特点

- 不可以存储重复元素
- 没有索引
- 可以将元素按照规则进行排序
  - TreeSet()：根据其元素的自然排序进行排序
  - TreeSet(Comparator comparator) ：根据指定的比较器进行排序

### 自然排序Comparable的使用

- 案例需求
  - 存储学生对象并遍历，创建TreeSet集合使用无参构造方法
  - 要求：按照年龄从小到大排序，年龄相同时，按照姓名的字母顺序排序
- 实现步骤
  1. 使用空参构造创建TreeSet集合
     - 用TreeSet集合存储自定义对象，无参构造方法使用的是自然排序对元素进行排序的
  2. 自定义的Student类实现Comparable接口
     - 自然排序，就是让元素所属的类实现Comparable接口，重写compareTo(T o)方法
  3. 重写接口中的compareTo方法
     - 重写方法时，一定要注意排序规则必须按照要求的主要条件和次要条件来写



```java
package com.yishan.myset;

/**
 * @author : yishan
 * @date : 2021-01-04 16:32
 */
public class Student implements Comparable<Student>{
    private String name;
    private int age;

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    @Override
    public int compareTo(Student o) {
        //先判断年龄，做差，小于零的放在前面，大于零的放在后面
        int result = this.age - o.age;
        //年龄做差相同的话，判断姓名，按照字典中的顺序排序
        return result == 0 ? this.name.compareTo(o.getName()):result ;
    }
}
```

- 测试类

```java
package com.yishan.myset;

import java.util.TreeSet;

/**
 * 自然排序 Comparable 泛型接口的使用
 * @author : yishan
 * @date : 2021-01-04 16:44
 */
public class TestMain01 {
    public static void main(String[] args) {
        TreeSet<Student> treeSet = new TreeSet<>();
        Student s = new Student("aa",26);
        Student s1 = new Student("bb",28);
        Student s2 = new Student("zz",22);
        Student s3 = new Student("gg",22);

        treeSet.add(s);
        treeSet.add(s1);
        treeSet.add(s2);
        treeSet.add(s3);

        System.out.println("treeSet = " + treeSet);

    }
}
```

### 比较器排序Comparator的使用

- 案例需求
  - 存储老师对象并遍历，创建TreeSet集合使用带参构造方法
  - 要求：按照年龄从小到大排序，年龄相同时，按照姓名的字母顺序排序
- 实现步骤
  - 用TreeSet集合存储自定义对象，带参构造方法使用的是比较器排序对元素进行排序的
  - 比较器排序，就是让集合构造方法接收Comparator的实现类对象，重写compare(T o1,T o2)方法
  - 重写方法时，一定要注意排序规则必须按照要求的主要条件和次要条件来写

```java
package com.yishan.myset;

/**
 * @author : yishan
 * @date : 2021-01-04 16:58
 */
public class Teacher {
    private String name;
    private int age;

    public Teacher() {
    }

    public Teacher(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Teacher{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

- 测试

```java
package com.yishan.myset;

import java.util.Comparator;
import java.util.TreeSet;

/**
 * 自然排序 Comparator比较器的使用
 * @author : yishan
 * @date : 2021-01-04 16:59
 */
public class TestMain02 {
    public static void main(String[] args) {
        TreeSet<Teacher> treeSet = new TreeSet<>(new Comparator<Teacher>() {
            @Override
            public int compare(Teacher o1, Teacher o2) {
                int result = o1.getAge() - o2.getAge();
                return result == 0 ? o1.getName().compareTo(o2.getName()) : result;
            }
        });

        Teacher teacher = new Teacher("aa",22);
        Teacher teacher1 = new Teacher("bb",55);
        Teacher teacher2 = new Teacher("cc",45);
        Teacher teacher3 = new Teacher("fd",22);

        treeSet.add(teacher);
        treeSet.add(teacher1);
        treeSet.add(teacher2);
        treeSet.add(teacher3);

        System.out.println("treeSet = " + treeSet);
    }
}
```





### 两种比较方式总结

- 两种比较方式小结
  - 自然排序: 自定义类实现Comparable接口,重写compareTo方法,根据返回值进行排序
  - 比较器排序: 创建TreeSet对象的时候传递Comparator的实现类对象,重写compare方法,根据返回值进行排序
  - 在使用的时候,默认使用自然排序,当自然排序不满足现在的需求时,必须使用比较器排序
- 两种方式中关于返回值的规则
  - 如果返回值为负数，表示当前存入的元素是较小值，存左边
  - 如果返回值为0，表示当前存入的元素跟集合中元素重复了，不存
  - 如果返回值为正数，表示当前存入的元素是较大值，存右边