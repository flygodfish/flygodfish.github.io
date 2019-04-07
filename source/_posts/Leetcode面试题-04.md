---
title: Leetcode面试题-04
date: 2019-03-29 14:52:07
tags: 
- 数组
categories: 
- 算法
- Leetcode
---

数组主要活用下标，可以创建额外的空间
<!-- more -->

<!-- 面试题 -->

# 问题

给定一个整数数组 nums ，找出一个序列中乘积最大的连续子序列（该序列至少包含一个数）。

示例 1:

输入: [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。

示例 2:

输入: [-2,0,-1]
输出: 0
解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。



## 思路

1.取最大值进行比较，但是也需要取最小值，因为可能存在最小值为负数然后乘以一个负数变正数，而正数乘以负数就变最小值的情况
2.可能遇到的问题就是乘积是可以多个乘积的也就是说数组可以是1个，2个，3个。。。
3.中间必须要有个tmp变量值表示上一把最大值，而不是直接用min = Math.min(Math.min(tmp * nums[i], min * nums[i]), nums[i]);这样出来的最大值是这一次计算的，是不对的！！！！


## 自己的代码

```java
class Solution {
    public int maxProduct(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        int max = nums[0], min = nums[0], result = nums[0];
        for (int i = 1; i < nums.length; i++) {
            int tmp = max;
            max = Math.max(Math.max(max * nums[i], min * nums[i]), nums[i]);
            min = Math.min(Math.min(tmp * nums[i], min * nums[i]), nums[i]);
            if (max > result) {
                result = max;
            }
        }
        return result;
    }
}
```

## 网上的代码

```java
```








<!-- 面试题 -->

# 问题

给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。

示例 1:

输入: [1,2,3,4,5,6,7] 和 k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右旋转 1 步: [7,1,2,3,4,5,6]
向右旋转 2 步: [6,7,1,2,3,4,5]
向右旋转 3 步: [5,6,7,1,2,3,4]

示例 2:

输入: [-1,-100,3,99] 和 k = 2
输出: [3,99,-1,-100]
解释: 
向右旋转 1 步: [99,-1,-100,3]
向右旋转 2 步: [3,99,-1,-100]


## 思路

1.旋转数组的思路，首先将所有数组翻转，然后在翻转前K个，最后翻转第K+1个到最后一个即可！
2.可能遇到的问题就是K超过了数组本身的长度。使用取余的方式做


## 自己的代码

```java
class Solution {
    public static void rotate(int[] nums, int k) {
        
        if( k == 0 || k%nums.length == 0){
            return;
        }
        reverse_arr(nums, 0, nums.length - 1);
        reverse_arr(nums, 0, (k-1)%nums.length);
        reverse_arr(nums,  k%nums.length , nums.length - 1);

    }

    public static void change_arr(int[] nums, int start, int end){
        int tmp = nums[start];
        nums[start] = nums[end];
        nums[end] = tmp;
    }

    public static void reverse_arr(int[] nums, int start, int end){
        while(start <= end){
            change_arr(nums, start, end);
            start ++;
            end --;
        }
    }
}
```

## 网上的代码

```java
```







