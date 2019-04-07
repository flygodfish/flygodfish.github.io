---
title: Leetcode面试题-05
date: 2019-03-29 14:52:11
tags:
- 堆
- 栈
---

栈先进后出，堆先进先出
<!-- more -->
<!-- 面试题 -->

# 问题

设计一个支持 push，pop，top 操作，并能在常数时间内检索到最小元素的栈。

    push(x) -- 将元素 x 推入栈中。
    pop() -- 删除栈顶的元素。
    top() -- 获取栈顶元素。
    getMin() -- 检索栈中的最小元素。

示例:

MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.


## 思路

1.做一个辅助栈，保存最小值，还有一个栈正常push,pop
2.当放入的值和最小栈最上面的值比较，如果比最小栈上的值小，则放入正常栈的同时也放入最小栈




## 自己的代码

```java
class MinStack {
        Stack normal_stack  = new Stack<>();
        Stack min_stack  = new Stack<>();

        /** initialize your data structure here. */
        public MinStack() {

        }

        public void push(int x) {
            normal_stack.push(x);
            if(min_stack.isEmpty()){
                min_stack.push(x);
            }else{
                int last_min = (int)min_stack.peek();
                if(last_min >= x){
                    min_stack.push(x);
                }
            }
        }

        public void pop() {
            int is_min = (int)normal_stack.peek();
            int last_min = (int)min_stack.peek();
            normal_stack.pop();
            if(is_min == last_min){
                min_stack.pop();
            }

        }

        public int top() {
           return  (int)normal_stack.peek();
        }

        public int getMin() {
            return (int) min_stack.peek();
        }
    }
```

## 网上的代码

```java
```



<!-- 面试题 -->

# 问题

在未排序的数组中找到第 k 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

示例 1:

输入: [3,2,1,5,6,4] 和 k = 2
输出: 5

示例 2:

输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4

说明:

你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度。


## 思路

1.简单的方式，进行排序然后选择第k个大的值。
2.这边我使用最大堆得方式，也就是先创建一个数组，大小就是k。这个数组是有序的！！
3.第一次初始化这个最大堆，是要排序的，然后遍历数组，和里面的值比较，如果给定的数组中有大于其中数组的值，则大的值放入最大堆。
4.最后返回k-1下标的值即可。


## 自己的代码

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {

        int[] max_arr = new int[k];
        init_max_arr(max_arr, nums, k);

        for (int i = k ; i < nums.length; i++) {
            insert(max_arr, nums[i]);
        }
        return max_arr[k - 1];


    }

    public void init_max_arr(int[] max_arr,int[] nums, int k){
        for (int i = 0; i < k; i++) {
            max_arr[i] = nums[i];
        }
        int start = 0;
        int tmp;
        while (start< max_arr.length - 1){
            int second = start + 1;
            if(max_arr[start] < max_arr[second]){
                tmp = max_arr[start];
                max_arr[start] = max_arr[second];
                max_arr[second] = tmp;
            }

            start++;
        }
    }

    public void insert(int[] max_arr, int num){
        int start = 0;
        int tmp;
        while (start< max_arr.length){
            if(max_arr[start] < num){
                tmp = max_arr[start];
                max_arr[start] = num;
                num = tmp;
            }

            start++;
        }
    }

}
```

## 网上的代码

```java
```