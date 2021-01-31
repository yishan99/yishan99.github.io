---
title: 包装类、String类源码分析、Date时间类
date: 2020-08-12 15:03:12
tags: [java基础]
---
## 包装类
<!--more-->
> 测试包装类

    package cn.yishan.test;
    
    public class TestWrappedClass {
        public static void main(String[] args) {
            //基本数据类型转成包装类对象
            Integer a = new Integer(3);
            Integer b = Integer.valueOf(30);
    
            //把包装类对象转成基本数据类型
            int c = b.intValue();
            double d = b.doubleValue();
    
            //把字符串转成包装类对象
            Integer e = new Integer("999");
            Integer f = Integer.parseInt("666666");
    
            //把包装类对象转成字符串
            String str = f.toString();
    
            //常见的常量
            System.out.println("int类型最大的整数："+Integer.MAX_VALUE);
        }
    }
## 自动装箱和拆箱（jdk1.5之后）
>测试自动装箱、自动拆箱

    package cn.yishan.test;
    
    public class TestAutoBox {
        public static void main(String[] args) {
            Integer a = 100; //自动装箱：Integer a = Integer.valueOf(100);
            int b = a ; //自动拆箱：编译器会修改成：int b = a.intValue();
    
            Integer c = null;
            if(c != null){
                int d = c; //自动拆箱：调用了：c.intValue()
            }
        }
    }

## 缓存问题
>测试自动装箱、自动拆箱

    package cn.yishan.test;
    
    public class TestAutoBox {
        public static void main(String[] args) {
            Integer a = 100; //自动装箱：Integer a = Integer.valueOf(100);
            int b = a ; //自动拆箱：编译器会修改成：int b = a.intValue();
    
            Integer c = null;
        //    if(c != null){
        //        int d = c; //自动拆箱：调用了：c.intValue()
        //    }
            //缓存[-128,127]之间的数字。实际就是系统初始的时候，创建了[-128,127]之间的一个缓存数组。
            //当我们调用valueOf()的时候，首先检查是否在[-128,127]之间，如果在这个范围则直接从缓存数组中拿出已经创建好的对象
            //如果不在这个范围，则创建新的Integer对象。
            Integer in1 = Integer.valueOf(-128);
            Integer in2 = -128;
            System.out.println(in1 == in2 );//true因为123在缓存范围内
            System.out.println(in1.equals(in2));//true
            Integer in3 = 1234;
            Integer in4 = 1234;
            System.out.println(in3 == in4);//false:因为1234不在缓存范围内
            System.out.println(in3.equals(in4));//true
        }
    }
## String
- 字符串内容全部存储到value[]数组中，而变量value是final类型的，也就是常量（即只能被赋值一次）。这就是“不可变对象”的典型定义方式
* private final char value[];
>测试String

    package cn.yishan.test;
    
    public class TestString {
        public static void main(String[] args) {
            //String str = "123456";
            //String str2 = str.substring(2,5);
            //System.out.println(str);
            //System.out.println(str2);
    
            //编译器做了优化，直接在编译的时候将字符串进行拼接
            String str1 = "hello" + " java";
            String str2 = "hello java";
            System.out.println(str1 == str2);//true
            String str3 = "hello";
            String str4 = " java";
            //编译的时候不知道变量中存储的是什么，所以没有办法在编译的时候进行优化
            String str5 = str3 + str4;
            System.out.println(str2 == str5);//false
            //做字符串比较的时候，使用equals 不要使用==
            System.out.println(str2.equals(str5));//true
        }
    }
## StringBuilder、StringBuffer
>测试StringBuilder、StringBuffer可变字符序列

    package cn.yishan.test;
    
    public class TestStringBuilder {
        public static void main(String[] args) {
            String str;
            //StringBuilder线程不安全，效率高（一般使用它）；StringBuffer线程安全，效率低。
            StringBuilder sb = new StringBuilder("abcdefg");
            System.out.println(Integer.toHexString(sb.hashCode()));
            System.out.println(sb);
            sb.setCharAt(2, 'M');
            System.out.println(Integer.toHexString(sb.hashCode()));
            System.out.println(sb);
        }
    }

>测试StringBuilder、StringBuffer可变字符序列的常用方法

    package cn.yishan.test;
    
    public class TestStringBuilder2 {
        public static void main(String[] args) {
            StringBuilder sb = new StringBuilder();
    
            for(int i = 0 ;i<26;i++){
                char temp = (char)('a'+i);
                sb.append(temp);
            }
            System.out.println(sb);
            sb.reverse();//倒序
            System.out.println(sb);
            sb.setCharAt(3,'高');//修改
            System.out.println(sb);
            //链式调用。核心就是：该方法调用了 return this,把自己返回了
            sb.insert(0,'我').insert(6,'爱');//添加
            System.out.println(sb);
            sb.delete(20,23);//删除
            System.out.println(sb);
        }
    }

>测试可变字符序列和不可变字符序列的陷阱(字符串循环累加一定要使用StringBuilder)

    package cn.yishan.test;
    
    public class TestStringBuilder3 {
        public static void main(String[] args) {
            //使用String进行字符串的拼接
            String str8 = " ";
            for (int i = 0; i < 5000;i++ ){
                str8 = str8 + i;//相当于产生了1000个对象，耗时费内存,工作中一定不能出现此种代码
            }
            //使用StringBuilder进行字符串的拼接
            StringBuilder sb1 = new StringBuilder();
            for (int i = 0; i < 5000;i++){
                sb1.append(i);//建议使用此种方式
            }
        }
    }
## 时间处理相关类(date)

##### Date类的构造方法

```java
package com.yishan.date;

import java.util.Date;

/**
 * @author : yishan
 * @date : 2021-01-02 19:18
 */
public class DateDemo1 {
    public static void main(String[] args) {
        //将当前时间封装成为一个对象
        Date date = new Date();
        System.out.println("date = " + date);
        //把从时间原点开始，过了指定的毫秒数，封装成为一个Date对象
        //需要考虑时差，中国在东八区，时差为八小时
        Date date1 = new Date(0L);
        System.out.println("date1 = " + date1);
    }
}
```

##### Date的成员方法

```java
package com.yishan.date;

import java.util.Date;

/**
 * Date的成员方法
 * @author : yishan
 * @date : 2021-01-02 19:25
 */
public class DateDemo02 {
    public static void main(String[] args) {
        //获取当前时间的毫秒值
        Date date = new Date();
        long time = date.getTime();
        System.out.println("time = " + time);
        long l = System.currentTimeMillis();
        System.out.println("l = " + l);

        System.out.println("---------------------");

        //设置时间，传递毫秒值
        Date date1 = new Date();
        date1.setTime(0L);
        System.out.println("date1 = " + date1);
    }
}

```



## SimpleDateFormat类

> 可以对Date对象，进行格式化和解析
>
> ##### 格式化
>
> - 格式化Date对象：Date的成员方法：format();
>
> ```java
> package com.yishan.date;
> 
> import java.text.SimpleDateFormat;
> import java.util.Date;
> 
> /**
>  * @author : yishan
>  * @date : 2021-01-02 19:36
>  */
> public class SimpleDateFormatDemo01 {
>     public static void main(String[] args) {
>         //创建Date对象
>         Date date = new Date();
>         //创建一个时间格式
>         SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH-mm-ss" );
>         //SimpleDateFormat中的成员方法format()，格式化时间
>         String format = sdf.format(date);
>         System.out.println("format = " + format);
>     }
> }
> ```

##### 解析

```java
package com.yishan.date;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

/**
 * 格式化和解析Date
 * @author : yishan
 * @date : 2021-01-02 19:36
 */
public class SimpleDateFormatDemo01 {
    public static void main(String[] args) throws ParseException {     

        //创建一个标准的时间类型格式
        String  s =  "2011-12-25 12-32-23";
        //SimpleDateFormat中的成员方法parse(),解析时间
        SimpleDateFormat sdf1 = new SimpleDateFormat("yyyy-MM-dd HH-mm-ss");
        Date parse = sdf1.parse(s);
        System.out.println("parse = " + parse);
    }
}
```



### JDK8时间日期类

![1609590321838](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1609590321838.png)

##### JDK8时间日期类（添加一天）

```java
package com.yishan.date;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.Date;

/**
 * JDK8时间日期类  添加一天 （体验）
 * @author : yishan
 * @date : 2021-01-02 20:11
 */
public class Jdk8Date {
    public static void main(String[] args) {
        //Test();
        String s = "2010-12-25 12:35:53";
        //创建一个时间格式
        DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        //解析
        LocalDateTime parse = LocalDateTime.parse(s, dateTimeFormatter);
        //添加一天
        LocalDateTime localDateTime = parse.plusDays(1);
        //格式化
        String format = localDateTime.format(dateTimeFormatter);
        System.out.println("format = " + format);
    }


    /**
     * jdk8之前  为当前时间添加一天详细实现
     * @throws ParseException
     */
    private static void Test() throws ParseException {
        String s = "2010-12-25 12:35:53";
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        Date date = sdf.parse(s);
        long time = date.getTime();
        time = time + (1000 * 60 * 60 * 24);
        String format = sdf.format(time);
        System.out.println("format = " + format);
    }
}

```

##### JDK8中获取时间对象

```java
package com.yishan.date;

import java.time.LocalDateTime;

/**
 * JDK8中获取时间对象
 * @author : yishan
 * @date : 2021-01-02 20:27
 */
public class Jdk8Date02 {
    public static void main(String[] args) {
        //获取当前时间对象
        LocalDateTime now = LocalDateTime.now();
        System.out.println("now = " + now);

        //获取指定时间对象
        LocalDateTime of = LocalDateTime.of(2010, 10, 25, 12, 23, 32);
        System.out.println("of = " + of);
    }
}

```

##### JDK8中获取时间的每个值

```java
package com.yishan.date;

import java.time.LocalDateTime;

/**
 * @author : yishan
 * @date : 2021-01-02 20:36
 */
public class Jdk8Date03 {
    public static void main(String[] args) {
        LocalDateTime localDateTime = LocalDateTime.of(2010, 12, 25, 12, 30, 59);

        //获取年份
        int year = localDateTime.getYear();
        System.out.println("year = " + year);

        //获取月份
        int monthValue = localDateTime.getMonthValue();
        System.out.println("monthValue = " + monthValue);

        //获取一个月中的第几天
        int dayOfMonth = localDateTime.getDayOfMonth();
        System.out.println("dayOfMonth = " + dayOfMonth);

        //获取一年中的第几天
        int dayOfYear = localDateTime.getDayOfYear();
        System.out.println("dayOfYear = " + dayOfYear);

        //获取小时
        int hour = localDateTime.getHour();
        System.out.println("hour = " + hour);

        //获取分钟
        int minute = localDateTime.getMinute();
        System.out.println("minute = " + minute);

        //获取秒
        int second = localDateTime.getSecond();
        System.out.println("second = " + second);
    }
}

```

##### LocalDateTime对象转换为 LocalDate对象 和 LocalTime对象

```java

package com.yishan.date;

import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.LocalTime;

/**
 * @author : yishan
 * @date : 2021-01-02 20:43
 */
public class Jdk8Date04 {
    public static void main(String[] args) {
        LocalDateTime localDateTime = LocalDateTime.of(2012, 12, 25, 10, 30, 36);

        //将 LocalDateTime对象 转换为 LocalDate对象
        LocalDate localDate = localDateTime.toLocalDate();
        System.out.println("localDate = " + localDate);

        //将 LocalDateTime对象 转换为 LocalTime对象
        LocalTime localTime = localDateTime.toLocalTime();
        System.out.println("localTime = " + localTime);
    }
}

```

##### LocalDateTime 格式化和解析

```java
package com.yishan.date;

import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

/**
 * LocalDateTime 中的格式化和解析
 * @author : yishan
 * @date : 2021-01-02 20:52
 */
public class Jdk8Date05 {
    public static void main(String[] args) {
        //method01();
        String s = "2010-12-25 12:12:25";
        //创建一个格式
        DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        //解析
        LocalDateTime parse = LocalDateTime.parse(s, dateTimeFormatter);
        System.out.println("parse = " + parse);
    }

    /**
     * JDK8中格式化时间
     */
    private static void method01() {
        LocalDateTime localDateTime = LocalDateTime.of(2020, 12, 25, 12, 12, 30);
        //创建一个时间格式
        DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH-mm-ss");
        //格式化时间
        String format = dateTimeFormatter.format(localDateTime);
        System.out.println("format = " + format);
    }
}

```



##### JDK8 中增加或者减少时间的方法

···java

```
package com.yishan.date;

import java.time.LocalDateTime;

/**
 * JDK8 中增加或者减少时间的方法
 * @author : yishan
 * @date : 2021-01-02 21:05
 */
public class Jdk8Date06 {
    public static void main(String[] args) {
        LocalDateTime localDateTime = LocalDateTime.of(2012, 10, 10, 11, 23, 50);
        //添加一年 返回新对象
        LocalDateTime newLocalDateTime = localDateTime.plusYears(1);
        System.out.println("newLocalDateTime = " + newLocalDateTime);

        //减少一年
        LocalDateTime localDateTime1 = localDateTime.plusYears(-2);
        System.out.println("localDateTime1 = " + localDateTime1);

    }
}
```

##### JDK8 时间类 修改时间

```java
package com.yishan.date;

import java.time.LocalDateTime;

/**
 * JDK8 时间类 修改时间
 * @author : yishan
 * @date : 2021-01-02 21:14
 */
public class Jdk8Date07 {
    public static void main(String[] args) {
        LocalDateTime localDateTime = LocalDateTime.of(2021, 1, 2, 21, 15, 30);
        //with...修改时间
        LocalDateTime newLocalDateTime = localDateTime.withYear(2015);
        System.out.println("newLocalDateTime = " + newLocalDateTime);
    }
}

```



##### 获取两个LocalDate对象的时间间隔(年，月，日)

```java
package com.yishan.date;

import java.time.LocalDate;
import java.time.Period;

/**
 * 获取两个LocalDate对象的时间间隔
 * @author : yishan
 * @date : 2021-01-02 21:21
 */
public class Jdk8Date08 {
    public static void main(String[] args) {
        LocalDate localDate = LocalDate.of(2005, 12, 12);
        LocalDate localDate1 = LocalDate.of(2018, 2, 25);

        //获取间隔时间
        Period period = Period.between(localDate, localDate1);
        System.out.println("period = " + period);
        //获取间隔时间中的年份
        System.out.println("period.getYears() = " + period.getYears());
        //获取间隔时间中的月份
        System.out.println("period.getMonths() = " + period.getMonths());
        //获取时间中的天数
        System.out.println("period.getDays() = " + period.getDays());
        //获取间隔的总月份
        System.out.println("period.toTotalMonths() = " + period.toTotalMonths());
    }
}

```

##### 获取两个LocalDateTime对象的时间间隔(秒，毫秒，纳秒)

```java
package com.yishan.date;

import java.time.Duration;
import java.time.LocalDateTime;

/**
 *获取两个LocalDateTime对象的时间间隔(秒，毫秒，纳秒)
 * @author : yishan
 * @date : 2021-01-02 21:33
 */
public class Jdk8Date09 {
    public static void main(String[] args) {
        LocalDateTime localDateTime = LocalDateTime.of(2020, 12, 25, 22, 30, 12);
        LocalDateTime localDateTime1 = LocalDateTime.of(2020, 12, 30, 12, 25, 40);
        Duration duration = Duration.between(localDateTime, localDateTime1);
        System.out.println("duration = " + duration);

        //获取间隔时间中的秒
        System.out.println("duration.getSeconds() = " + duration.getSeconds());
        //毫秒
        System.out.println("duration.toMillis() = " + duration.toMillis());
        //纳秒
        System.out.println("duration.toNanos() = " + duration.toNanos());
    }
}

```



## Calendar日历类

Calendar类是一个抽象类，提供了关于日期计算的相关功能

> 测试日期类的使用

    package cn.yishan.test;
    
    import javax.xml.crypto.Data;
    import java.util.Calendar;
    import java.util.Date;
    import java.util.GregorianCalendar;



    public class TestCalendar {
        public static void main(String[] args) {
            //获得日期的相关元素
            Calendar calendar = new GregorianCalendar(2999,10,9,22,10,50);
            int year = calendar.get(Calendar.YEAR);
            int month = calendar.get(Calendar.MONDAY);
            //1-7表示对应的星期几。1是星期一   以此类推
            int weekday = calendar.get(Calendar.DAY_OF_WEEK);
            //几号
            int day = calendar.get(Calendar.DATE);
            System.out.println(year);
            System.out.println(month);
            //0-11表示对应的月份。0是1一月   以此类推
            System.out.println(calendar);
            System.out.println(weekday);
            System.out.println(day);
    
            //设置日期的相关元素
            Calendar c2 = new GregorianCalendar();
            c2.set(Calendar.YEAR,8012);
            System.out.println(c2);
    
            //日期的计算
            Calendar c3 = new GregorianCalendar();
            c3.add(Calendar.DATE,100);
            System.out.println(c3);
    
            //日期对象和时间对象的转化
            Date d4 = c3.getTime();
            Calendar c4 = new GregorianCalendar();
            c4.setTime(new Date());
            printCalendar(c4);
        }
    
        public static void printCalendar(Calendar c){
            //打印：1918年2月3日 11:23:34 周三
            int year = c.get(Calendar.YEAR);
            int month = c.get(Calendar.MONDAY) + 1;
            int date = c.get(Calendar.DAY_OF_MONTH);
            int dayweek = c.get(Calendar.DAY_OF_WEEK) - 1;
            String dayweek2 = dayweek==0?"日":dayweek+"";
            int hour = c.get(Calendar.HOUR);
            int minute = c.get(Calendar.MINUTE);
            int second = c.get(Calendar.SECOND);
            System.out.println(year+"年"+month+"月"+date+"日"+hour+"时"+minute+"分"+second+"秒"+" 周"+dayweek2);
        }
    }

