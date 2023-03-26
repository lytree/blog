---
title: 'Search in a Binary Search Tree'
description:
date: 2021-11-26T13:57:03+08:00
image:
math: true
license:
categories:
  - leetcode
tags:
  - tree
hidden: false
comments: false
draft: false
---

## Search in a Binary Search Tree

<!--more-->

| Category   | Difficulty    | Likes | Dislikes |
| ---------- | ------------- | ----- | -------- |
| algorithms | Easy (76.14%) | 201   | -        |

给定二叉搜索树（BST）的根节点和一个值。 你需要在 BST 中找到节点值等于给定值的节点。 返回以该节点为根
的子树。 如果节点不存在，则返回 NULL。

```
例如，

给定二叉搜索树:

        4
       / \
      2   7
     / \
    1   3

和值: 2 你应该返回如下子树:

      2
     / \
    1   3

在上述示例中，如果要找的值是 5，但因为没有节点值为 5，我们应该返回 NULL。
```

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        while (root != null) {
            if (val == root.val) {
                return root;
            }
            root = val < root.val ? root.left : root.right;
        }
        return null;
    }
}
```
