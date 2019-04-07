---
title: Leetcode面试题-01
date: 2019-03-13 21:52:32
tags: 数组
categories: 
- 算法
- Leetcode
---

Leetcode面试题-01  共3道

<!-- more -->

# 问题
只出现一次的数字

给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。
说明：

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

示例 1:

输入: [2,2,1]
输出: 1
示例 2:

输入: [4,1,2,1,2]
输出: 4


## 思路

1.所有数组内的数值都做异或处理
异或的共性 相同数值做异或为0！


## 自己的代码

```java
class Solution {
    public int singleNumber(int[] nums) {
        int result = nums[0]; 
        for(int x =1; x < nums.length; x++){
            result = result^nums[x];
        }
        return result;
    }
}
```



# 问题
求众数

给定一个大小为 n 的数组，找到其中的众数。众数是指在数组中出现次数大于  n/2  的元素。

你可以假设数组是非空的，并且给定的数组总是存在众数。

示例 1:

输入: [3,2,3]
输出: 3
示例 2:

输入: [2,2,1,1,1,2,2]
输出: 2


## 思路

1.遍历一次数组，使用额外存储HASHMAP，key存值，val存计数器
2.当计数器超过或等于n/2+1直接跳出循环
3.返回得到的结果

## 自己的代码

```java
import java.util.HashMap;
class Solution {
    public int majorityElement(int[] nums) {
        HashMap hashMap = new HashMap<Integer,Integer>();
        int result =nums[0] ;
        for (int i = 0; i < nums.length; i++) {
            if(hashMap.containsKey(nums[i])){
                int tmp = (Integer)hashMap.get(nums[i])+1;
                if(tmp >= (nums.length/2 +1)){
                    result = nums[i];
                    break;
                }else{
                    hashMap.put(nums[i],(Integer)hashMap.get(nums[i])+1);
                }
                
            }else {
                hashMap.put(nums[i], 1);
            }
        }
        
        return result;
    }
}
```



# 问题
合并两个有序数组

给定两个有序整数数组 nums1 和 nums2，将 nums2 合并到 nums1 中，使得 num1 成为一个有序数组。

说明:

初始化 nums1 和 nums2 的元素数量分别为 m 和 n。
你可以假设 nums1 有足够的空间（空间大小大于或等于 m + n）来保存 nums2 中的元素。
示例:

输入:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

输出: [1,2,2,3,5,6]


## 思路

### 思路一
1.使用额外的数组存储比较2个数组的最小值
2.每次都取2个数组的头一个，如果小，存入额外的数组
3.直到结束，将额外的数组赋值给一开始的数组

### 思路二
1.直接从m,n处从后往前比较，将比较大的值放入第一个数组的最末位
2.和思路一类似

## 自己的代码
### 思路二
```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int i = m -1 , j = n- 1, count = nums1.length - 1;//定义三个指针，分别指向三个数组的第一个元素

        while (i >=0 && j >=0) {
            if (nums1[i] >= nums2[j]) {
                nums1[count--] = nums1[i--];
            } else {
                nums1[count--] = nums2[j--];
            }
        }
        System.out.println("111111");

        while (i >= 0) {
            nums1[count--] = nums1[i--];
        }


        while (j >= 0) {
            nums1[count--] = nums2[j--];
        }
        
    }
}
```


