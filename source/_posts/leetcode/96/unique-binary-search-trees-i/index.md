---
title: 'Unique Binary Search Trees I'
description:
date: 2021-11-26T20:40:34+08:00
image:
math: true
license:
categories:
  - leetcode
tags:
  - dynamic-programming
  - tree
hidden: false
comments: false
draft: false
---

## 不同的二叉搜索树

<!--more-->

| Category   | Difficulty      | Likes | Dislikes |
| ---------- | --------------- | ----- | -------- |
| algorithms | Medium (69.98%) | 1418  | -        |

Companies snapchat

给你一个整数 n ，求恰由 n 个节点组成且节点值从 1 到 n 互不相同的 二叉搜索树 有多少种？返回满足题意的
二叉搜索树的种数。

```
示例 1：

输入：n = 3 输出：5
```

```
示例 2：

输入：n = 1 输出：1
```

> 提示：1 <= n <= 19

假设 n 个节点存在二叉排序树的个数是 G (n)，令 f(i) 为以 i 为根的二叉搜索树的个数，则
$G(n) = f(1) + f(2) + f(3) + f(4) + ... + f(n)$

当 i 为根节点时，其左子树节点个数为 i-1 个，右子树节点为 n-i，则 $f(i) = G(i-1)*G(n-i)$

综合两个公式可以得到 卡特兰数 公式 $G(n) = G(0)_G(n-1)+G(1)_(n-2)+...+G(n-1)\*G(0)$

```java
class Solution {
    public int numTrees(int n) {
        int[] dp = new int[n + 1];
        dp[0] = 1;
        dp[1] = 1;

        for (int i = 2; i < n + 1; i++) {
            for (int j = 1; j < i + 1; j++) {
                dp[i] += dp[j - 1] * dp[i - j];
            }
        }

        return dp[n];
    }
}
```
