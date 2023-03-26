---
title: 'median-of-two-sorted-arrays'
description:
date: 2021-11-20T21:59:18+08:00
image:
math:
license:
categories:
  - leetcode
tags:
  - array
  - binary-search
  - divide-and-conquer
hidden: false
comments: false
draft: false
---

## 寻找两个正序数组的中位数

<!--more-->

| Category   | Difficulty    | Likes | Dislikes |
| ---------- | ------------- | ----- | -------- |
| algorithms | Hard (40.94%) | 4681  | -        |


给定两个大小分别为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。请你找出并返回这两个正序数组的 中位
数 。

算法的时间复杂度应该为 O(log (m+n)) 。

示例 1：

```
输入：nums1 = [1,3], nums2 = [2] 输出：2.00000 解释：合并数组 = [1,2,3] ，中位数 2
```

示例 2：

```
输入：nums1 = [1,2], nums2 = [3,4] 输出：2.50000 解释：合并数组 = [1,2,3,4] ，中位数 (2 + 3) / 2 =
2.5
```

示例 3：

```
输入：nums1 = [0,0], nums2 = [0,0] 输出：0.00000
```

示例 4：

```
输入：nums1 = [], nums2 = [1] 输出：1.00000
```

示例 5：

```
输入：nums1 = [2], nums2 = [] 输出：2.00000
```

> 提示：nums1.length == m nums2.length == n 0 <= m <= 1000 0 <= n <= 1000 1 <= m + n <= 2000 -106 <=
> nums1[i], nums2[i] <= 106

```java

```
