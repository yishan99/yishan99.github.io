---
title: 手工实现HashSet
date: 2020-08-31 20:29:53
tags: [java基础,HashSet]
---
### 手工实现HashSet
<!--more-->


```java
    package cn.yishan.mycollection;

    import java.util.HashMap;

    /**
    * 手工实现一个HashSet,更深刻的理解HashSet底层原理
    * @author yishan
    */
    public class SxtHashSet {

        HashMap map;

        private static final Object PRESENT = new Object();
        public SxtHashSet(){
            map = new HashMap<>();
        }

        public int size(){
            return map.size();
        }
        public void add(Object o){
            map.put(o,PRESENT);
        }

        @Override
        public String toString() {
            StringBuilder sb = new StringBuilder();
            sb.append("[");

            for (Object key:map.keySet()){
                sb.append(key+",");
            }
            sb.setCharAt(sb.length()-1,']');
            return sb.toString();
        }

        public static void main(String[] args) {
            SxtHashSet set = new SxtHashSet();

            set.add("aaa");
            set.add("bbb");
            set.add("ccc");
            System.out.println(set);
        }

    }
```

