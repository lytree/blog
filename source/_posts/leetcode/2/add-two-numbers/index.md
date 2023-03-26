---
title: 'Add Two Numbers'
description:
date: 2021-11-04T21:23:28+08:00
image:
math: true
license:
categories:
  - leetcode
tags:
  - linked-list
  - math

hidden: false
comments: false
draft: false
---

## 两数相加

<!--more-->

| Category   | Difficulty      | Likes | Dislikes |
| ---------- | --------------- | ----- | -------- |
| algorithms | Medium (40.97%) | 7007  | -        |

- Tags
  - linked-list
  - math

给你两个**非空**的链表，表示两个非负的整数。它们每位数字都是按照**逆序**的方式存储的，并且每个节点只
能存储**一位**数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0  开头。

示例 1：

```java

 输入：l1 = [2,4,3], l2 = [5,6,4]
 输出：[7,0,8]
 解释：342 + 465 = 807.

```

示例 2：

```java
 输入：l1 = [0], l2 = [0]
 输出：[0]

```

示例 3：

```java

 输入：l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
 输出：[8,9,9,9,0,0,0,1]

```

> 提示：每个链表中的节点数在范围 [1, 100] 内 0 题目数据保证列表表示的数字不含前导零

同时遍历两个链表，逐位计算它们的和，并与当前位置的进位值相加。具体而言，如果当前两个链表处相应位置的
数字为 n1,n2，进位值为 $\textit{carry}$，则它们的和为 $n1+n2+ \textit{carry}$ ；其中，答案链表处相应
位置的数字为
$$
(n1+n2+\textit{carry}) \bmod10
$$
，而新的进位值为$\lfloor\frac{n1+n2+\textit{carry}}{10}\rfloor$。

如果两个链表的长度不同，则可以认为长度短的链表的后面有若干个 0 。

此外，如果链表遍历结束后，有 $\textit{carry} > 0$。

```java
//  Definition for singly-linked list.
 public class ListNode {
     int val;
     ListNode next;
     ListNode() {}
     ListNode(int val) { this.val = val; }
     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 }

class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode pre = new ListNode(0);
        ListNode curr = pre;
        int carry = 0;
        while (l1 != null || l2 != null) {
            int x = l1 != null ? l1.val : 0;
            int y = l2 != null ? l2.val : 0;
            int sum = x + y + carry;
            //进位数字
            carry = sum / 10;
            //本位数字
            sum = sum % 10;
            curr.next = new ListNode(sum);
            curr = curr.next;
            if (l1 != null) {
                l1 = l1.next;
            }
            if (l2 != null) {
                l2 = l2.next;
            }
        }
        //最后一位是否进位
        if (carry == 1) {
            curr.next = new ListNode(carry);
        }
        //去除第一个节点
        return pre.next;
    }
}
// @lc code=end

```
