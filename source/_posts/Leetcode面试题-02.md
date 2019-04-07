---
title: Leetcode面试题-02
date: 2019-03-20 23:02:42
tags: 
- 数组
- 数学归纳法
categories: 
- 算法
- Leetcode
---

Leetcode面试题-02  共2道

<!-- more -->

# 问题

编写一个高效的算法来搜索 m x n 矩阵 matrix 中的一个目标值 target。该矩阵具有以下特性：

每行的元素从左到右升序排列。
每列的元素从上到下升序排列。
示例:

现有矩阵 matrix 如下：

[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]

给定 target = 5，返回 true。

给定 target = 20，返回 false。


## 思路

1.从右上角开始运行也就是上面的15开始（也可以是左下角18开始）
2.比较目标值如果目标大的就下移，遇到小的就左移

## 自己的代码

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
       if(matrix.length == 0){
            return false;
        }
        boolean flag = false;
        int hang_index = 0;
        int lie_index =  matrix[0].length -1 ;
        while(hang_index <  matrix.length && lie_index >= 0 ){
            int tmp =matrix[hang_index][lie_index];
            if(tmp < target){
                hang_index ++;
            }else if(tmp > target){
                lie_index --;
            }else{
                flag = true;
                break;
            }
        }
        return flag;
    }
}
```

## 网上的代码

```java
```



# 问题

你将获得 K 个鸡蛋，并可以使用一栋从 1 到 N  共有 N 层楼的建筑。

每个蛋的功能都是一样的，如果一个蛋碎了，你就不能再把它掉下去。

你知道存在楼层 F ，满足 0 <= F <= N 任何从高于 F 的楼层落下的鸡蛋都会碎，从 F 楼层或比它低的楼层落下的鸡蛋都不会破。

每次移动，你可以取一个鸡蛋（如果你有完整的鸡蛋）并把它从任一楼层 X 扔下（满足 1 <= X <= N）。

你的目标是确切地知道 F 的值是多少。

无论 F 的初始值如何，你确定 F 的值的最小移动次数是多少？

示例 1：

输入：K = 1, N = 2
输出：2
解释：
鸡蛋从 1 楼掉落。如果它碎了，我们肯定知道 F = 0 。
否则，鸡蛋从 2 楼掉落。如果它碎了，我们肯定知道 F = 1 。
如果它没碎，那么我们肯定知道 F = 2 。
因此，在最坏的情况下我们需要移动 2 次以确定 F 是多少。
示例 2：

输入：K = 2, N = 6
输出：3
示例 3：

输入：K = 3, N = 14
输出：4
 

提示：

1 <= K <= 100
1 <= N <= 10000


## 思路

0.简单理解 2个鸡蛋，6层，如果二分法，第三层碎了，那么就必须是从1,2层都扔一次鸡蛋随意会有3次步骤
1.选择每m步能求出最大的层数！即使用d[k][m] = 最大的层数 k是鸡蛋 m是步数
2.如果在d[k][m] = X层 摔出鸡蛋 1.碎了 d[k-1][m-1] <= X 2.没碎 d[k][m-1] + X > X
3.如果要求d[k][m]的最大的层数，d[k][m] = d[k][m-1] + X = d[k][m-1] + d[k-1][m-1] + 1(本层)

分析2：
1.如果一个鸡蛋，确认有哪层，那就是必须每层都扔一次！所以有d[1][m] = m
2.如果没有鸡蛋，那么没法确认层数 所以有d[0][m] = 0 同理d[k][0] = 0



## 自己的代码

```java
public static  int superEggDrop(int K, int N) {
        if(K == 1){
            return N;
        }
        if(K == 0){
            return 0;
        }
        if(N==1){
            return 1;
        }
        int res = 0;
        int tmp;
        int[][] qiu_max_floor = new int[K+1][N+1];
        qiu_max_floor[0][0] = 0;
        for (int i = 1; i < K + 1; i++) {
            qiu_max_floor[i][0] = 0;
            qiu_max_floor[i][1] = 1;
            for (int j = 1; j < N + 1; j++) {
                qiu_max_floor[1][j] = j;
                qiu_max_floor[i][j] = qiu_max_floor[i][j-1] + qiu_max_floor[i-1][j-1] + 1;
                tmp = qiu_max_floor[i][j];
                if(tmp >= N){
                    res = j;
                    break;
                }

            }
        }
        return res;
    }
```

## 网上的代码

```java
```


