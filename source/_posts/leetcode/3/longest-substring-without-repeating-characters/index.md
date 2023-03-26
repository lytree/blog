---
title: 'Longest Substring Without Repeating Characters'
description:
date: 2021-11-04T21:41:45+08:00
image:
math: true
license:
categories:
  - leetcode
tags:
  - hash-table
  - two-pointers
  - string
  - sliding-window
hidden: false
comments: false
draft: false
---

## 无重复字符的最长子串

<!--more-->

| Category   | Difficulty      | Likes | Dislikes |
| ---------- | --------------- | ----- | -------- |
| algorithms | Medium (38.17%) | 6371  | -        |



给定一个字符串 s ，请你找出其中不含有重复字符的   最长子串   的长度。

示例  1:

```
输入: s = "abcabcbb" 输出: 3 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

示例 2:

```
输入: s = "bbbbb" 输出: 1 解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

示例 3:

```
输入: s = "pwwkew" 输出: 3 解释: 因为无重复字符的最长子串是  "wke"，所以其长度为 3。 请注意，你的答
案必须是 子串 的长度，"pwke"  是一个子序列，不是子串。
```

示例 4:

```
输入: s = "" 输出: 0
```

> 提示：0 <= s.length <= 5 \* 104 s 由英文字母、数字、符号和空格组成

我们使用两个指针表示字符串中的某个子串（或窗口）的左右边界，其中左指针代表着上文中「枚举子串的起始位
置」，而右指针即为上文中的 $r_k$ ；

在每一步的操作中，我们会将左指针向右移动一格，表示**我们开始枚举下一个字符作为起始位置**，然后我们可以
不断地向右移动右指针，但需要保证这两个指针对应的子串中没有重复的字符。在移动结束后，这个子串就对应着
**以左指针开始的，不包含重复字符的最长子串**。我们记录下这个子串的长度；

在枚举结束后，我们找到的最长的子串的长度即为答案

```java

class Solution {
    public int lengthOfLongestSubstring(String s) {
        HashSet<Character> characters = new HashSet<>();
        int result = 0;
        int size = s.length();
        int right = -1;
        for (int left = 0; left < size; left++) {
            // 左指针右移
            if (left != 0) {
                characters.remove(s.charAt(left-1));
            }
            while (right + 1 < size && !characters.contains(s.charAt(right + 1))) {
                // 不断地移动右指针
                characters.add(s.charAt(right + 1));
                ++right;
            }
            // 第 i 到 rk 个字符是一个极长的无重复字符子串
            result = Math.max(result, right - left + 1);
        }
        return result;
    }
}


```
