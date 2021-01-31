---
title: 手工实现LinkedList
date: 2020-08-16 12:20:03
tags: [java基础,LinkedList]
---
## LinkedList特点和底层实现
LinkedList底层是用双向链表实现的存储。特点：查询效率低，增删效率高，线程不安全。
<!--more-->
双向链表也叫双链表，是链表的一种，他的每个数据节点中都有两个指针，分别指向前一个节点和后一个节点。所以，从双向链表中的任意一个节点开始，都可以很方便的找到所有节点。

## 定义节点

```java
package cn.yishan.mycollection;

/**
* 定义节点
* @author yishan
*/
public class Node {

    Node previous; //上一个节点
    Node next; //下一个节点
    Object element; //元素数据

    public Node(Node previous, Node next, Object element) {
        this.previous = previous;
        this.next = next;
        this.element = element;
    }

    public Node(Object element) {
        this.element = element;
    }
}
```


## 手工实现LinkedList01_add方法

        package cn.yishan.mycollection;


```java
    /**
    * 自定义一个链表
    * add方法
    * @author yishan
    */
    public class SxtLinkedList01 {

        private Node first;
        private Node last;

        private int size;

        public void add(Object obj){
            Node node = new Node(obj);

            if (first==null){
                first = node;
                last = node;
            }else{
                node.previous = last;
                node.next = null;

                last.next = node;
                last = node;
            }
        }

        @Override
        public String toString() {
            StringBuilder sb = new StringBuilder("[");
            Node temp = first;
            while (temp!=null){
                sb.append(temp.element+",");
                temp=temp.next;
            }
            sb.setCharAt(sb.length()-1,']');
            return sb.toString();
        }

        public static void main(String[] args) {
            SxtLinkedList01 list = new SxtLinkedList01();
            list.add("a");
            list.add("b");
            list.add("c");
            System.out.println(list);
        }
    }
```

## 手工实现LinkedList02_get查询_节点遍历

```java
    package cn.yishan.mycollection;

    /**
    * 自定义一个链表
    * 增加get方法,节点遍历
    * @author yishan
    */
    public class SxtLinkedList02 {

        private Node first;
        private Node last;

        private int size;

        public void add(Object obj){
            Node node = new Node(obj);

            if (first==null){
                first = node;
                last = node;
            }else{
                node.previous = last;
                node.next = null;

                last.next = node;
                last = node;
            }
            size++;
        }

        public Object get(int index){

            if (index<0||index>size-1){
                throw new RuntimeException("索引数字不合法："+index);
            }
            Node temp = null;
            if(index<=(size>>1)){  //size>>1相当于除以2
                temp = first;
                for (int i = 0;i<index;i++){
                    temp = temp.next;
                }
            }else {
                temp = last;
                for (int i = size-1;i>index;i--){
                    temp = temp.previous;
                }
            }
            return temp.element;

        }

        @Override
        public String toString() {
            StringBuilder sb = new StringBuilder("[");
            Node temp = first;
            while (temp!=null){
                sb.append(temp.element+",");
                temp=temp.next;
            }
            sb.setCharAt(sb.length()-1,']');
            return sb.toString();
        }

        public static void main(String[] args) {
            SxtLinkedList02 list = new SxtLinkedList02();
            list.add("a");
            list.add("b");
            list.add("c");
            list.add("d");
            list.add("e");
            list.add("f");

            System.out.println(list);
            System.out.println(list.get(5));
        }
    }

    package cn.yishan.mycollection;
```

## 手工实现LinkedList03_增加remove

```java
    /**
    * 自定义一个链表
    * 增加remove
    * @author yishan
    */
    public class SxtLinkedList03 {

        private Node first;
        private Node last;

        private int size;

        public void remove(int index){
            Node temp = getNode(index);

            if (temp!=null){
                Node up = temp.previous;
                Node down = temp.next;
                if (up!=null){

                    up.next = down;
                }
                if (down!=null){

                    down.previous = up;
                }
                //被删除的元素是第一个元素时
                if (index==0){
                    first = down;
                }
                //被删除的元素时最后一个元素时
                if (index==size-1){
                    last = up;
                }

                size--;
            }
        }
        public void add(Object obj){
            Node node = new Node(obj);

            if (first==null){
                first = node;
                last = node;
            }else{
                node.previous = last;
                node.next = null;

                last.next = node;
                last = node;
            }
            size++;
        }

        public Object get(int index){

            if (index<0||index>size-1){
                throw new RuntimeException("索引数字不合法："+index);
            }

            Node temp = getNode(index);
            return temp!=null?temp.element:null;

        }

        public Node getNode(int index){
            Node temp = null;
            if(index<=(size>>1)){  //size>>1相当于除以2
                temp = first;
                for (int i = 0;i<index;i++){
                    temp = temp.next;
                }
            }else {
                temp = last;
                for (int i = size-1;i>index;i--){
                    temp = temp.previous;
                }
            }
            return temp;
        }
        @Override
        public String toString() {
            StringBuilder sb = new StringBuilder("[");
            Node temp = first;
            while (temp!=null){
                sb.append(temp.element+",");
                temp=temp.next;
            }
            sb.setCharAt(sb.length()-1,']');
            return sb.toString();
        }

        public static void main(String[] args) {
            SxtLinkedList03 list = new SxtLinkedList03();
            list.add("a");
            list.add("b");
            list.add("c");
            list.add("d");
            list.add("e");
            list.add("f");

            System.out.println(list);
            System.out.println(list.get(5));
            list.remove(5);
            System.out.println(list);
        }
    }
```

## 手工实现LinkedList04_增加插入节点

```java
    package cn.yishan.mycollection;

    /**
    * 自定义一个链表
    * 增加插入节点
    * @author yishan
    */
    public class SxtLinkedList04 {

        private Node first;
        private Node last;

        private int size;

        public void add(int index, Object obj){

            Node newNode = new Node(obj);
            Node temp = getNode(index);

            if(temp!=null){
                Node up = temp.previous;

                up.next = newNode;
                newNode.previous = up;

                newNode.next = temp;
                temp.previous = newNode;

            }
        }

        public void remove(int index){
            Node temp = getNode(index);

            if (temp!=null){
                Node up = temp.previous;
                Node down = temp.next;
                if (up!=null){

                    up.next = down;
                }
                if (down!=null){

                    down.previous = up;
                }
                //被删除的元素是第一个元素时
                if (index==0){
                    first = down;
                }
                //被删除的元素时最后一个元素时
                if (index==size-1){
                    last = up;
                }

                size--;
            }
        }
        public void add(Object obj){
            Node node = new Node(obj);

            if (first==null){
                first = node;
                last = node;
            }else{
                node.previous = last;
                node.next = null;

                last.next = node;
                last = node;
            }
            size++;
        }

        public Object get(int index){

            if (index<0||index>size-1){
                throw new RuntimeException("索引数字不合法："+index);
            }

            Node temp = getNode(index);
            return temp!=null?temp.element:null;

        }

        public Node getNode(int index){
            Node temp = null;
            if(index<=(size>>1)){  //size>>1相当于除以2
                temp = first;
                for (int i = 0;i<index;i++){
                    temp = temp.next;
                }
            }else {
                temp = last;
                for (int i = size-1;i>index;i--){
                    temp = temp.previous;
                }
            }
            return temp;
        }
        @Override
        public String toString() {
            StringBuilder sb = new StringBuilder("[");
            Node temp = first;
            while (temp!=null){
                sb.append(temp.element+",");
                temp=temp.next;
            }
            sb.setCharAt(sb.length()-1,']');
            return sb.toString();
        }

        public static void main(String[] args) {
            SxtLinkedList04 list = new SxtLinkedList04();
            list.add("a");
            list.add("b");
            list.add("c");
            list.add("d");
            list.add("e");
            list.add("f");

            System.out.println(list);
            list.add(3,"杨");
            System.out.println(list);
        }
    }
```

## 手工实现LinkedList05_增加泛型_增加封装


```java
    package cn.yishan.mycollection;

    /**
    * 自定义一个链表
    * 增加增加泛型，增加封装
    * @author yishan
    */
    public class SxtLinkedList05<E> {

        private Node first;
        private Node last;

        private int size;

        public void add(int index, E element){
            checkRange(index);
            Node newNode = new Node(element);
            Node temp = getNode(index);

            if(temp!=null){
                Node up = temp.previous;

                up.next = newNode;
                newNode.previous = up;

                newNode.next = temp;
                temp.previous = newNode;

            }
        }

        public void remove(int index){
            checkRange(index);
            Node temp = getNode(index);

            if (temp!=null){
                Node up = temp.previous;
                Node down = temp.next;
                if (up!=null){

                    up.next = down;
                }
                if (down!=null){

                    down.previous = up;
                }
                //被删除的元素是第一个元素时
                if (index==0){
                    first = down;
                }
                //被删除的元素时最后一个元素时
                if (index==size-1){
                    last = up;
                }

                size--;
            }
        }
        public void add(E element){

            Node node = new Node(element);

            if (first==null){
                first = node;
                last = node;
            }else{
                node.previous = last;
                node.next = null;

                last.next = node;
                last = node;
            }
            size++;
        }

        public E get(int index){
            checkRange(index);
            Node temp = getNode(index);
            return temp!=null?(E) temp.element:null;

        }

        private void checkRange(int index){
            if (index<0||index>size-1){
                throw new RuntimeException("索引数字不合法："+index);
            }
        }

        private Node getNode(int index){
            Node temp = null;
            if(index<=(size>>1)){  //size>>1相当于除以2
                temp = first;
                for (int i = 0;i<index;i++){
                    temp = temp.next;
                }
            }else {
                temp = last;
                for (int i = size-1;i>index;i--){
                    temp = temp.previous;
                }
            }
            return temp;
        }
        @Override
        public String toString() {
            StringBuilder sb = new StringBuilder("[");
            Node temp = first;
            while (temp!=null){
                sb.append(temp.element+",");
                temp=temp.next;
            }
            sb.setCharAt(sb.length()-1,']');
            return sb.toString();
        }

        public static void main(String[] args) {
            SxtLinkedList05<String> list = new SxtLinkedList05<>();
            list.add("a");
            list.add("b");
            list.add("c");
            list.add("d");
            list.add("e");
            list.add("f");

            System.out.println(list);
            list.add(3,"杨");
            System.out.println(list);
            System.out.println(list.get(1));
        }
    }
```

