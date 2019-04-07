---
title: 06 | 递归（下）：分而治之，从归并排序到MapReduce
date: 2019-03-31 22:38:17
tags: 计算机
categories: 
- 网络学习
- 极客时间
- 程序员的数学基础课
---

复杂的问题可以通过化简来逐步求解。

<!-- more -->

<!-- 文章 -->
# 摘要

今天主要讲分而治之的思想。
归并排序使用了分治的思想，而这个过程需要使用递归来实现。

# 疑问

## 什么是归并排序？

主要思想是将2个有序数组进行合并，再次之前一直将数组拆分成2个，直到只存在一个长度的数组，那么这个数组必然是有序的。
然后在慢慢有序的合并。

## 通过归并排序，如何合并有序数组{1,2,5,8} 和{3,4,6}？

![](1.jpg)

## 思想

1.通过递归来完成归并排序，主要1个函数进行递归，还要个函数进行数组的合并排序
2.guibing_sort这个方法先进行数组分割，分割到最小，也就是第一次循环merge的时候start到end只有2个数组长度
3.然后合并的过程函数就是上面图上的过程


## 代码

```java
public class guibing2_sort {
    public static void main(String[] args) {
        int[] a = new int[]{10,2,3,1};
        guibing_sort(a, 0, a.length- 1);
        for (int i = 0; i < a.length; i++) {
            System.out.println(a[i]);
        }
    }
    public static void guibing_sort(int[] nums, int start, int end){
        //不能相等 因为做right归并时用了mid+1!,不然会报错得。
        if(start < end){
            int mid = start + (end - start) /2 ;
            guibing_sort(nums, start, mid);
            guibing_sort(nums, mid + 1, end);
            merge(nums, start, mid, end);
        }
    }
    private static void merge(int[] nums, int start, int mid, int end) {
        int[] left_arr = new int[mid- start+1];
        int ls = 0;
        int le = left_arr.length - 1;
        for (int i = start; i < mid + 1; i++) {
            left_arr[ls] = nums[i];
            ls ++;
        }
        ls = 0;
        le = left_arr.length - 1;
        int[] right_arr = new int[end-mid];
        int rs = 0;
        int re = right_arr.length - 1;
        for (int i = mid + 1; i < end + 1; i++) {
            right_arr[rs] = nums[i];
            rs ++;
        }
        rs = 0;
        re = right_arr.length - 1;
        int[] tmp = new int[end -start + 1];
        int ts = 0;
        while(ls <= le && rs <= re){
            if(left_arr[ls] <= right_arr[rs]){
                tmp[ts] = left_arr[ls];
                ls ++;
            }else{
                tmp[ts] = right_arr[rs];
                rs ++;
            }
            ts++;
        }
        if(le < ls){
            tmp[ts] = right_arr[rs];
        }
        if(re < rs){
            tmp[ts] = left_arr[ls];
        }
        for (int i = 0; i < tmp.length; i++) {
            nums[start] = tmp[i];
            start++;
        }
    }
}
```

# 基本内容

## 分而治之的思想

简称分治。
将一个复杂的问题分解为两个或者更多的相同规模的子问题。
然后对这些子问题在细分，知道很容易被求解了，这样复杂的问题也就能得到解决。
分治主要用递归来实现。

## 分布式系统分治思想

如果排序的数组很大，大到超过内存。
那么我们就可以用分治的思想来做，吧这个数据集分解成很多小份，计算能胜任的大小。
并行处理，然后在各个机器上处理完后，一个一个返回结果即可。

## MapReduce主要的三个步骤体现了分布的思想

![](2.jpg)

### 1.数据分割和映射

将数据源切片，主要讲内容变成key-value的形式匹配然后存储到哈希结构中。
主要降低了每台机器的负载。

### 2.归约

就是将key相同的内容匹配，然后将value归并

### 3.合并

为了减少发送到归约阶段的key-value，现在本地将key-value进行一次合并。

# 思考题

## 归并的时候将原有的数组分解为2组，而不是更多组，只是为什么？分成多个组可以吗？

因为在合并阶段最小的合并就是2个长度为1的数组进行合并，而不是3个一起合并，可以分成多个组。
但是这样会产生更多的中间结果，计算也会变得更复杂，得不偿失

# 扩展

## 快速排序

快速排序就是随意选一个基准，然后遍历整个数组，把大于基准的数值放到基准的右边，把小于基准的数值放到基准的左边

### 思想

1.基准可以随意选，但是我这里的基准选的就是此数组的第一个也就是下标为0的数值作为基准
2.然后函数的返回值应该是重新洗牌后的下标，也就是上面的下标基准拍完后的下标的值。
3.也是使用递归函数，结束的标准就是2个指针一个初始指针一个结尾指针。初始指针的值小于结束指针的值，如果大于着结束return


### 代码

```java
public class kuaisu_sort {
    public static void main(String[] args) {
        int[] a = new int[]{3,1,10,6,77,4,2,6,8,9};
        sort(a, 0, a.length);
        for (int i = 0; i < a.length; i++) {
            System.out.println(a[i]);
        }
    }
     public static int kuaisu_sort(int[] a, int start, int end){
        int mid_index = start + (end - start) / 2;
        int mid = a[mid_index];
        int[] min = new int[a.length];
        int min_index = 0;
        int[] max = new int[a.length];
        int max_index = 0;
        int[] same = new int[a.length];
        int same_index = 0;
        for (int i = 0; i < a.length; i++) {
            if (a[i] < mid) {
                min[min_index] = a[i];
                min_index++;
            } else if (a[i] > mid) {
                max[max_index] = a[i];
                max_index++;
            } else {
                same[same_index] = a[i];
                same_index++;
            }
        }
        merge_arr(a, min, min_index, max, max_index, same, same_index);

        return min_index + same_index;


     }

    public static void sort(int[] array,int lo ,int hi){
        if(lo>=hi){
            return ;
        }
        int index = kuaisu_sort(array,lo,hi);
        sort(array,lo,index-1);
        sort(array,index+1,hi);
    }

     public static void merge_arr(int[] a, int[] min, int min_index, int[] max, int max_index, int[] same, int same_index){
         System.arraycopy(min, 0, a, 0,min_index );
         System.arraycopy(same, 0, a, min_index,same_index );
         System.arraycopy(max, 0, a, min_index + same_index , max_index );
     }
}
```

# 小结

![](lesson6.jpg)