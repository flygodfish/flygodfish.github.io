---
title: Leetcode面试题-03
date: 2019-03-24 20:34:00
tags: 
- 字符串
categories: 
- 算法
- Leetcode
---

Leetcode 算法 - 字符串
<!-- more -->

# 问题

给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

说明：本题中，我们将空字符串定义为有效的回文串。

示例 1:

输入: "A man, a plan, a canal: Panama"
输出: true
示例 2:

输入: "race a car"
输出: false

回文串的解释：

正过来和反过来的字符串一致，不算特殊符号

## 思路

1.使用双指针，一个指向前一个指向后
2.同时往前和后，比较，如果遇到非字母和数字的就向下移，然后比较指针的值

## 自己的代码

```java
    public boolean isPalindrome(String cs) {
        boolean flag = true;
        char[] cs_c = cs.toLowerCase().toCharArray();
        int right = 0;
        int left = cs_c.length - 1;
        while(right < cs_c.length && left >=0) {
            char tmp = cs_c[right];
            char tmp2 = cs_c[left];

            while(!Pattern.matches("[a-z0-9]", String.valueOf(tmp)) && right < cs_c.length - 1){
                right++;
                tmp = cs_c[right];
            }
            while(!Pattern.matches("[a-z0-9]", String.valueOf(tmp2)) && left > 0){
                left--;
                tmp2 = cs_c[left];
            }
            if(!Pattern.matches("[a-z0-9]", String.valueOf(tmp2))){
                tmp2 = ' ';
            }
            if(!Pattern.matches("[a-z0-9]", String.valueOf(tmp))){
                tmp = ' ';
            }
            if(!String.valueOf(tmp).equals(String.valueOf(tmp2))){
                flag = false;
                break;
            }
            right++;
            left--;
        }
        return flag;
    }
```

## 网上的代码

```java
```



<!-- 面试题 -->

# 问题

给定一个字符串 s，将 s 分割成一些子串，使每个子串都是回文串。

返回 s 所有可能的分割方案。

示例:

输入: "aab"
输出:
[
  ["aa","b"],
  ["a","a","b"]
]


## 思路

1.主思想是递归。 返回值是[['m','m','a','n'],['mm','a','n']]
2.返回list<list<String>>,最后返回的时候，然后主要是add list,完成的list添加进list<list<String>>
3.变量是字符串，当字符串长度为1，或者0的是否返回list<list<String>>

## 自己的代码

```java
import java.util.ArrayList;
import java.util.List;
import java.util.regex.Pattern;

class Solution {
    public boolean isPalindrome(String cs) {
        boolean flag = true;
        char[] cs_c = cs.toLowerCase().toCharArray();
        int right = 0;
        int left = cs_c.length - 1;
        while(right < cs_c.length && left >=0) {
            char tmp = cs_c[right];
            char tmp2 = cs_c[left];

            while(!Pattern.matches("[a-z0-9]", String.valueOf(tmp)) && right < cs_c.length - 1){
                right++;
                tmp = cs_c[right];
            }
            while(!Pattern.matches("[a-z0-9]", String.valueOf(tmp2)) && left > 0){
                left--;
                tmp2 = cs_c[left];
            }
            if(!Pattern.matches("[a-z0-9]", String.valueOf(tmp2))){
                tmp2 = ' ';
            }
            if(!Pattern.matches("[a-z0-9]", String.valueOf(tmp))){
                tmp = ' ';
            }
            if(!String.valueOf(tmp).equals(String.valueOf(tmp2))){
                flag = false;
                break;
            }
            right++;
            left--;
        }
        return flag;
    }
    
    public List<List<String>> find_huiwen_str(String s,List<String> list, List<List<String>> llist){
        int s_len ;
        if(s.length() == 0){
            llist.add(list);
            return llist;
        }
        if(s.length() == 1){
            list.add(s);
            llist.add(list);
            return llist;
        }
        ArrayList tmp_list = (ArrayList) list;
        List<String> tl = (List<String>)tmp_list.clone();;

        for (int i = 1; i <= s.length() ; i++) {
            String tmp = s.substring(0,i);
            if(isPalindrome(tmp)){
                tl = (List<String>)tmp_list.clone();
                tl.add(tmp);
                int ss = tl.get(tl.size() - 1).toString().replace("[","").replace("]","").replace(",","").replace(" ","").length();
                s_len = s.length();
                String tmp_s = s.substring(ss, s_len);
                find_huiwen_str(tmp_s, tl, llist);
            }
        }
        
        return llist;
    }
    
    public List<List<String>> xunhuan_huiwen_str_list(String s, List<List<String>> list) {
        List<String> f_list = new ArrayList<String>();
        list = find_huiwen_str(s, f_list, list);
        return list;
    }
    
    public List<List<String>> partition(String s) {
        List<List<String>> first_list = new ArrayList<List<String>>();
        return xunhuan_huiwen_str_list(s, first_list);
    }
}
```

## 网上的代码

```java
```