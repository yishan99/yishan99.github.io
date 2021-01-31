---
title: 数组、Arrays、多维数组
date: 2020-08-11 11:56:57
tags: [java基础,冒泡排序,二分查找]
---
## 数组的拷贝
<!--more-->
>测试数组的拷贝
    package cn.yishan.array2;

    import java.util.Arrays;

    public class TestArrayCopy {
        public static void main(String[] args) {
            //testBasicCopy2();
            String[] s1 = {"你","是","一","个","帅","哥"};
            //removeElment(s1,2);
            s1 = extendRange(s1);

        }

        public static void testBasicCopy(){
            String[] s1 = {"aa","bb","cc","dd","ee"};
            String[] s2 = new String[10];
            System.arraycopy(s1,2,s2,6,3);

            System.out.println(Arrays.toString(s2));
        }
        //测试从数组中删除某个元素（本质上还是数组的拷贝）
        public static void testBasicCopy2(){
            String[] s1 = {"aa","bb","cc","dd","ee"};
            //String[] s2 = new String[5];
            System.arraycopy(s1,3,s1,3-1,s1.length-3);
            s1[s1.length-1] = null;
            System.out.println(Arrays.toString(s1));
        }
        //删除数组中指定索引位置的元素，并将原数组返回
        public static String[] removeElment(String[] s,int index){
            System.arraycopy(s,index+1,s,index,s.length-index-1);
            s[s.length-1] = null;
            System.out.println(Arrays.toString(s));
            return s;
        }

        //数组的扩容（本质上是：先定义一个更大的数组，然后将原数组内容原封不动的拷贝到新数组中
        public static String[] extendRange(String[] s1){
            //String[] s1 = {"aa","bb","cc"};
            String[] s2 = new String[s1.length + 10];
            System.arraycopy(s1,0,s2,0,s1.length);
            System.out.println(Arrays.toString(s2));
            return s2;
        }
    }
## Arrays类
>测试java.util.Arrays工具类的使用
    package cn.yishan.array2;

    import java.lang.reflect.Array;
    import java.util.Arrays;

    public class TestArrays {
        public static void main(String[] args) {
            int[] a = {100,20,30,56,86,31};
            System.out.println(a);
            System.out.println(Arrays.toString(a));
            //排序（从小到大）
            Arrays.sort(a);
            System.out.println(Arrays.toString(a));
            //二分查找
            System.out.println(Arrays.binarySearch(a,20));
        }
    }
## 二维数组
多维数组可以看成以数组为元素的数组。可以有二维、三维、甚至更多维数组，但是实际开发中用到的非常少。最多学习到二维数组（一般使用容器）
> 测试二维数组
    package cn.yishan.array2;

    public class Test2DimensionArray {
        public static void main(String[] args) {
            int[][] a = new int[3][];

            a[0] = new int[]{63,66};
            a[1] = new int[]{11,22,33,66,55};
            a[2] = new int[]{23,96,96,55};

            System.out.println(a[2][1]);
            //静态初始化二维数组
            int[][] b = {{20,30},{222,666,333},{56,99,66}};
            System.out.println(b[1][2]);
        }
    }
## 数组存储表格数据
>测试数组存储表格数据
    package cn.yishan.array2;

    import java.util.Arrays;

    public class TestArrayTableData {
        public static void main(String[] args) {
            Object[] emp1 = {"1001","杨亦山1",18,"程序猿1","2020.8.11"};
            Object[] emp2 = {"1002","杨亦山2",18,"程序猿2","2020.8.12"};
            Object[] emp3 = {"1003","杨亦山3",18,"程序猿3","2020.8.13"};

            Object[][] tableData = new Object[3][];
            tableData[0] = emp1;
            tableData[1] = emp2;
            tableData[2] = emp3;

            for (Object[] temp:tableData){
                System.out.println(Arrays.toString(temp));
            }
        }
    }
## 冒泡排序的基础算法+优化算法
>测试冒泡排序
    package cn.yishan.array2;

    import java.util.Arrays;

    public class TestBubbleSort {
        public static void main(String[] args) {
            int[] values = {6,2,5,4,3,2,8,9,7,1};
            int temp = 0;
            boolean flag = true;
            for (int i = 0;i<values.length-1;i++){

                for(int j=0;j<values.length-1-i;j++){
                    //比较大小，换顺序
                    if(values[j]>values[j+1]){
                        temp = values[j];
                        values[j] = values[j+1];
                        values[j+1] = temp;
                        flag = false;
                    }
                    System.out.println(Arrays.toString(values));
                }
                if (flag){
                    System.out.println("结束！");
                    break;
                }
                System.out.println("_____");
            }

        }
    }
## 二分查找（折半查找）
> 二分法查找
    package cn.yishan.array2;

    import java.util.Arrays;

    public class TestBinarySearch {
        public static void main(String[] args) {
            int[] arr = {20,30,21,54,1,5,23,65};
            Arrays.sort(arr);
            System.out.println(Arrays.toString(arr));
            System.out.println(myBinarySearch(arr,20));
        }

        public static int myBinarySearch(int[] arr,int value){
            int low = 0;
            int high = arr.length - 1 ;
            while (low <= high){
                int mid = (low + high) / 2;
                if (arr[mid] == value){
                    return mid;
                }
                if (arr[mid] < value){
                    low = mid+1;
                }
                if(arr[mid] > value){
                    high = mid - 1;
                }
            }
            return -1;
        }
    }

