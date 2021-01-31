---
title: 表格存储数据
date: 2020-09-01 18:32:10
tags: [java基础,Map,List]
---
### map和list结合存储整张表

每行数据使用一个：Map
<!--more-->
整个表格使用一个List

ORM思想：对象关系映射


        package cn.yishan.collection;

        import java.util.*;

        /**
        * 测试表格数据的存储
        * @author yishan
        */
        public class TestStoreData {
            public static void main(String[] args) {

                Map<String, Object> row1 = new HashMap<>();
                row1.put("id",1001);
                row1.put("姓名","张三");
                row1.put("薪水","2000");
                row1.put("入职日期","2001.5.2");

                Map<String, Object> row2 = new HashMap<>();
                row2.put("id",1002);
                row2.put("姓名","李四");
                row2.put("薪水","3000");
                row2.put("入职日期","2010.4.8");

                Map<String, Object> row3 = new HashMap<>();
                row3.put("id",1003);
                row3.put("姓名","王五");
                row3.put("薪水","4500");
                row3.put("入职日期","2010.7.9");

                List<Map<String, Object>> table1 = new ArrayList<>();
                table1.add(row1);
                table1.add(row2);
                table1.add(row3);

                for (Map<String, Object> row:table1){
                    Set<String> keyset = row.keySet();

                    for (String key:keyset){
                        System.out.print(key+":"+row.get(key)+"\t");
                    }
                    System.out.println();
                }
            }
        }

### javabean和list结合存储整张表

每一行数据使用一个：javabean对象

整个表格使用一个：Map/List


        package cn.yishan.collection;

        import java.util.*;

        /**
        * 测试表格数据的存储
        * 体会ORM思想
        * 每一行数据使用javabean对象存储，多行使用放到map或者list中
        * @author yishan
        */
        public class TestStoreData2 {
            public static void main(String[] args) {
                User user1 = new User(1001,"张三",2000,"2001.5.6");
                User user2 = new User(1002,"李四",3000,"2002.6.8");
                User user3 = new User(1003,"王五",4000,"2006.9.6");

                List<User> list = new ArrayList<>();
                list.add(user1);
                list.add(user2);
                list.add(user3);

                for (User u:list){
                    System.out.println(u);
                }
                System.out.println("--------------------------------------------");
                Map<Integer, User> map = new HashMap<>();
                map.put(1001,user1);
                map.put(1002,user2);
                map.put(1003,user3);
                Set<Integer> keyset = map.keySet();
                for (Integer key:keyset){
                    System.out.println(key+"--"+map.get(key));
                }
            }
        }

        class User{
            private int id;
            private String name;
            private double salary;
            private String hiredate;

            //一个完整的javabean。要有set和get方法，以及无参的构造器
            public User(){}
            public User(int id, String name, double salary, String hiredate) {
                this.id = id;
                this.name = name;
                this.salary = salary;
                this.hiredate = hiredate;
            }

            public int getId() {
                return id;
            }

            public void setId(int id) {
                this.id = id;
            }

            public String getName() {
                return name;
            }

            public void setName(String name) {
                this.name = name;
            }

            public double getSalary() {
                return salary;
            }

            public void setSalary(double salary) {
                this.salary = salary;
            }

            public String getHiredate() {
                return hiredate;
            }

            public void setHiredate(String hiredate) {
                this.hiredate = hiredate;
            }

            @Override
            public String toString() {
                return "id:"+id+",name:"+name+",salary:"+salary+",hiredate:"+hiredate;
            }
        }