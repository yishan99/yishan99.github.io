---
title: 手工实现HashMap
date: 2020-08-17 20:22:22
tags: [java基础,HashMap]
---

### 手工数显HashMap01_基本结构_put存储键值对
<!--more-->
        package cn.yishan.mycollection;

        /**
        * 自定义一个HashMap
        * @author yishan
        */
        public class SxtHashMap01 {

            Node2[] table; //位桶数组。bucket array
            int size; //存放的键值对的个数

            public SxtHashMap01(){
                table = new Node2[16]; //长度一般定义为2的整数幂

            }

            public void put(Object key,Object value){
                //定义了新的节点对象
                Node2 newNode = new Node2();
                newNode.hash = myHash(key.hashCode(),table.length);
                newNode.key = key;
                newNode.value = value;
                newNode.next = null;

                Node2 temp = table[newNode.hash];
                if (temp==null){
                    //此处数组元素为空，则直接将新节点放进去
                    table[newNode.hash] = newNode;
                }else{
                    //此处数组元素不为空，则遍历对应链表
                }
            }

            public static void main(String[] args) {
                SxtHashMap01 m = new SxtHashMap01();
                m.put(10,"aa");
                m.put(20,"bb");
                m.put(30,"cc");
                System.out.println(m);
            }

            public int myHash(int v,int length){
                System.out.println("hash in myHash:"+(v&(length-1)));//直接位运算，效率高
                System.out.println("hash in myHash:"+(v%(length-1)));//取模运算，效率低
                return v&(length-1);

            }
        }

### 手工数显HashMap02_解决键重复问题_链表生成问题

        package cn.yishan.mycollection;

        /**
        * 自定义一个HashMap
        * 实现了put方法增加键值对，并解决了键重复的时候覆盖相应的节点
        * @author yishan
        */
        public class SxtHashMap02 {

            Node2[] table; //位桶数组。bucket array
            int size; //存放的键值对的个数

            public SxtHashMap02(){
                table = new Node2[16]; //长度一般定义为2的整数幂

            }

            public void put(Object key,Object value){
                //定义了新的节点对象
                Node2 newNode = new Node2();
                newNode.hash = myHash(key.hashCode(),table.length);
                newNode.key = key;
                newNode.value = value;
                newNode.next = null;

                Node2 temp = table[newNode.hash];

                Node2 iterLast= null;//正在遍历的最后一个元素
                boolean keyRepeat = false;
                if (temp==null){
                    //此处数组元素为空，则直接将新节点放进去
                    table[newNode.hash] = newNode;
                }else{
                    //此处数组元素不为空，则遍历对应链表
                    while (temp!=null){
                        //判断key如果重复，则覆盖
                        if(temp.key.equals(key)){
                            keyRepeat = true;
                            System.out.println("key重复了");

                            temp.value = value; //只是覆盖了value即可。其他的值（hash，key,next）保持不变

                            break;
                        }else{
                            //key不重复，则遍历下一个
                            iterLast = temp;
                            temp = temp.next;
                        }
                    }
                    if(!keyRepeat){ //如果没有发生key重复的情况在，则添加到链表最后。

                        iterLast.next = newNode;
                    }
                }
            }

            public static void main(String[] args) {
                SxtHashMap01 m = new SxtHashMap01();
                m.put(10,"aa");
                m.put(20,"bb");
                m.put(30,"cc");
                m.put(20,"ssss");
                System.out.println(m);
            }

            public int myHash(int v,int length){
                System.out.println("hash in myHash:"+(v&(length-1)));//直接位运算，效率高
                System.out.println("hash in myHash:"+(v%(length-1)));//取模运算，效率低
                return v&(length-1);

            }
        }

### 手工数显HashMap03_实现toString方法

        package cn.yishan.mycollection;

        /**
        * 自定义一个HashMap
        * 实现toString方法，方便查看Map中的键值对信息
        * @author yishan
        */
        public class SxtHashMap03 {

            Node2[] table; //位桶数组。bucket array
            int size; //存放的键值对的个数

            public SxtHashMap03(){
                table = new Node2[16]; //长度一般定义为2的整数幂

            }

            public void put(Object key,Object value){
                //定义了新的节点对象
                Node2 newNode = new Node2();
                newNode.hash = myHash(key.hashCode(),table.length);
                newNode.key = key;
                newNode.value = value;
                newNode.next = null;

                Node2 temp = table[newNode.hash];

                Node2 iterLast= null;//正在遍历的最后一个元素
                boolean keyRepeat = false;
                if (temp==null){
                    //此处数组元素为空，则直接将新节点放进去
                    table[newNode.hash] = newNode;
                }else{
                    //此处数组元素不为空，则遍历对应链表
                    while (temp!=null){
                        //判断key如果重复，则覆盖
                        if(temp.key.equals(key)){
                            keyRepeat = true;
                            System.out.println("key重复了");

                            temp.value = value; //只是覆盖了value即可。其他的值（hash，key,next）保持不变

                            break;
                        }else{
                            //key不重复，则遍历下一个
                            iterLast = temp;
                            temp = temp.next;
                        }
                    }
                    if(!keyRepeat){ //如果没有发生key重复的情况在，则添加到链表最后。

                        iterLast.next = newNode;
                    }
                }
            }

            @Override
            public String toString() {
                StringBuilder sb = new StringBuilder("{");

                //遍历bucket数组
                for(int i = 0;i < table.length; i++){
                    Node2 temp = table[i];
                    //遍历链表
                    while (temp!=null){
                        sb.append(temp.key+":"+temp.value+",");

                        temp = temp.next;
                    }
                }
                sb.setCharAt(sb.length()-1,'}');
                return sb.toString();

            }

            public static void main(String[] args) {
                SxtHashMap02 m = new SxtHashMap02();
                m.put(10,"aa");
                m.put(20,"bb");
                m.put(30,"cc");
                m.put(20,"ssss");
                System.out.println(m);
            }

            public int myHash(int v,int length){
                System.out.println("hash in myHash:"+(v&(length-1)));//直接位运算，效率高
                System.out.println("hash in myHash:"+(v%(length-1)));//取模运算，效率低
                return v&(length-1);

            }
        }

### 手工数显HashMap04_实现get方法
        package cn.yishan.mycollection;

        /**
        * 自定义一个HashMap
        * 实现get方法，根据键对象获得对应的值
        * @author yishan
        */
        public class SxtHashMap04 {

            Node2[] table; //位桶数组。bucket array
            int size; //存放的键值对的个数

            public SxtHashMap04(){
                table = new Node2[16]; //长度一般定义为2的整数幂

            }

            public Object get(Object key){
                int hash = myHash(key.hashCode(),table.length);
                Object value = null;
                if(table[hash]!= null){
                    Node2 temp = table[hash];

                    while (temp!=null){
                        if(temp.key.equals(key)){ //如果相等，则说明找到了键值对，返回相应的value
                            value = temp.value;
                            break;
                        }else{
                            temp = temp.next;
                        }
                    }
                }
                return value;
            }

            public void put(Object key,Object value){
                //定义了新的节点对象
                Node2 newNode = new Node2();
                newNode.hash = myHash(key.hashCode(),table.length);
                newNode.key = key;
                newNode.value = value;
                newNode.next = null;

                Node2 temp = table[newNode.hash];

                Node2 iterLast= null;//正在遍历的最后一个元素
                boolean keyRepeat = false;
                if (temp==null){
                    //此处数组元素为空，则直接将新节点放进去
                    table[newNode.hash] = newNode;
                    size++;
                }else{
                    //此处数组元素不为空，则遍历对应链表
                    while (temp!=null){
                        //判断key如果重复，则覆盖
                        if(temp.key.equals(key)){
                            keyRepeat = true;
                            System.out.println("key重复了");

                            temp.value = value; //只是覆盖了value即可。其他的值（hash，key,next）保持不变

                            break;
                        }else{
                            //key不重复，则遍历下一个
                            iterLast = temp;
                            temp = temp.next;
                        }
                    }
                    if(!keyRepeat){ //如果没有发生key重复的情况在，则添加到链表最后。
                        iterLast.next = newNode;
                        size++;
                    }
                }
            }

            @Override
            public String toString() {
                StringBuilder sb = new StringBuilder("{");

                //遍历bucket数组
                for(int i = 0;i < table.length; i++){
                    Node2 temp = table[i];
                    //遍历链表
                    while (temp!=null){
                        sb.append(temp.key+":"+temp.value+",");

                        temp = temp.next;
                    }
                }
                sb.setCharAt(sb.length()-1,'}');
                return sb.toString();

            }

            public static void main(String[] args) {
                SxtHashMap03 m = new SxtHashMap03();
                m.put(10,"aa");
                m.put(20,"bb");
                m.put(30,"cc");
                m.put(20,"ssss");
                System.out.println(m);
                System.out.println(m.get(20));
            }

            public int myHash(int v,int length){
                //System.out.println("hash in myHash:"+(v&(length-1)));//直接位运算，效率高
                //System.out.println("hash in myHash:"+(v%(length-1)));//取模运算，效率低
                return v&(length-1);

            }
        }

### 手工数显HashMap05_实现泛型
    package cn.yishan.mycollection;

    /**
    * 自定义一个HashMap
    * 实现泛型
    * @author yishan
    */
    public class SxtHashMap05<K,V>{

        Node3[] table; //位桶数组。bucket array
        int size; //存放的键值对的个数

        public SxtHashMap05(){
            table = new Node3[16]; //长度一般定义为2的整数幂

        }

        public V get(K key){
            int hash = myHash(key.hashCode(),table.length);
            V value = null;
            if(table[hash]!= null){
                Node3 temp = table[hash];

                while (temp!=null){
                    if(temp.key.equals(key)){ //如果相等，则说明找到了键值对，返回相应的value
                        value = (V) temp.value;
                        break;
                    }else{
                        temp = temp.next;
                    }
                }
            }
            return V;
        }

        public void put(K key,V value){
            //定义了新的节点对象
            Node3 newNode = new Node3();
            newNode.hash = myHash(key.hashCode(),table.length);
            newNode.key = key;
            newNode.value = value;
            newNode.next = null;

            Node3 temp = table[newNode.hash];

            Node3 iterLast= null;//正在遍历的最后一个元素
            boolean keyRepeat = false;
            if (temp==null){
                //此处数组元素为空，则直接将新节点放进去
                table[newNode.hash] = newNode;
                size++;
            }else{
                //此处数组元素不为空，则遍历对应链表
                while (temp!=null){
                    //判断key如果重复，则覆盖
                    if(temp.key.equals(key)){
                        keyRepeat = true;
                        System.out.println("key重复了");

                        temp.value = value; //只是覆盖了value即可。其他的值（hash，key,next）保持不变

                        break;
                    }else{
                        //key不重复，则遍历下一个
                        iterLast = temp;
                        temp = temp.next;
                    }
                }
                if(!keyRepeat){ //如果没有发生key重复的情况在，则添加到链表最后。
                    iterLast.next = newNode;
                    size++;
                }
            }
        }

        @Override
        public String toString() {
            StringBuilder sb = new StringBuilder("{");

            //遍历bucket数组
            for(int i = 0;i < table.length; i++){
                Node3 temp = table[i];
                //遍历链表
                while (temp!=null){
                    sb.append(temp.key+":"+temp.value+",");

                    temp = temp.next;
                }
            }
            sb.setCharAt(sb.length()-1,'}');
            return sb.toString();

        }

        public static void main(String[] args) {
            SxtHashMap04<Integer,String> m = new SxtHashMap04<>();
            m.put(10,"aa");
            m.put(20,"bb");
            m.put(30,"cc");
            m.put(20,"ssss");
            System.out.println(m);
            System.out.println(m.get(20));
        }

        public int myHash(int v,int length){
            //System.out.println("hash in myHash:"+(v&(length-1)));//直接位运算，效率高
            //System.out.println("hash in myHash:"+(v%(length-1)));//取模运算，效率低
            return v&(length-1);

        }
    }
