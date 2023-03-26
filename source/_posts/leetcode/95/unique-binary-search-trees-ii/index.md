---
title: 'Unique Binary Search Trees Ii'
description:
date: 2021-11-25T21:04:50+08:00
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

## 不同的二叉搜索树 II

| Category   | Difficulty     | Likes | Dislikes |
| ---------- | -------------- | ----- | -------- |
| algorithms | Medium(70.35%) | 1055  | -        |

给你一个整数 n ，请你生成并返回所有由 n 个节点组成且节点值从 1 到 n 互不相同的不同 二叉搜索树 。可以
按 任意顺序 返回答案。

```
示例 1：

输入：n = 3 输出：[[1,null,2,null,3],[1,null,3,2],[2,1,3],[3,1,null,null,2],[3,2,null,1]]
```

```
示例 2：

输入：n = 1 输出：[[recover1.jpg]]
```

> 提示：1 <= n <= 8


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
    public List<TreeNode> generateTrees(int n) {
        if (n == 0) {
            return new ArrayList<>();
        }
        return generateTrees(1, n);
    }

    //利用二叉搜索树的规律 左子树的所有值小于根节点，右子树的所有值大于根节点
    private List<TreeNode> generateTrees(int start, int end) {
        LinkedList<TreeNode> treeNodes = new LinkedList<>();
        if (start > end) {
            treeNodes.add(null);
            return treeNodes;
        }
        for (int i = start; i <= end; i++) {

            //所有左子树集合
            List<TreeNode> treeNodesLeft = generateTrees(start, i - 1);
            //所有右子树集合
            List<TreeNode> treeNodeRight = generateTrees(i + 1, end);

            // 从左子树集合中选出一棵左子树，从右子树集合中选出一棵右子树，拼接到根节点 i 上
            for (TreeNode left : treeNodesLeft) {
                for (TreeNode right : treeNodeRight) {
                    TreeNode currTree = new TreeNode(i);
                    currTree.left = left;
                    currTree.right = right;
                    treeNodes.add(currTree);
                }
            }
        }
        return treeNodes;
    }

}
```

```java



```