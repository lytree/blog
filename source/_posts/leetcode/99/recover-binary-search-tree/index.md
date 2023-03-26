---
title: 'Recover Binary Search Tree'
description:
date: 2021-12-26T15:33:49+08:00
image:
math: true
license:
categories:
  - leetcode
tags:
  - tree
  - depth-first-search
hidden: false
comments: false
draft: false
---

## 恢复二叉搜索树

| Category   | Difficulty     | Likes | Dislikes |
| ---------- | -------------- | ----- | -------- |
| algorithms | Medium(61.08%) | 620   | -        |

给你二叉搜索树的根节点 root ，该树中的两个节点的值被错误地交换。请在不改变其结构的情况下，恢复这棵树
。

示例 1： ![recover1](./recover1.jpg)

```输入：root = [1,3,null,null,2] 输出：[3,1,null,null,2] 解释
：3 不能是 1 左孩子，因为 3 > 1 。交换 1 和 3 使二叉搜索树有效。

```

示例 2：

![recover2](./recover2.jpg)

```输入：root = [3,1,4,null,null,2] 输出：[2,1,4,null,null,3] 解释：2 不
能在 3 的右子树中，因为 2 < 3 。交换 2 和 3 使二叉搜索树有效。

```

提示：

树上节点的数目在范围 [2, 1000] 内 -231 <= Node.val <= 231 - 1

进阶：使用 O(n) 空间复杂度的解法很容易实现。你能想出一个只使用 O(1) 空间的解决方案吗？

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
/**
     * 题目限定只有两个节点错位 二叉搜索树特性 中序遍历是从小到大顺序 两个节点错误 第一个错误节点为大于后一个节点 第二个错误节点为小于前一个节点
     *
     * @param root
     */
    public void recoverTree(TreeNode root) {
        Deque<TreeNode> stack = new ArrayDeque<>();
        TreeNode pre = null, x = null, y = null;
        while (stack.size() > 0 || root != null) {
            while (root != null) {
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            if (pre != null && pre.val > root.val) {
                y = root;
                if (x == null) {
                    //第一处出现 前一个节点大于当前节点  取前一个节点
                    x = pre;
                } else {
                    //第二处出现 前一个几点大于当前节点  取当前节点 找到两个节点 跳出循环不需要在遍历先去
                    break;
                }
            }
            pre = root;
            root = root.right;
        }
        swap(x, y);
    }

    public void swap(TreeNode x, TreeNode y) {
        int tmp = x.val;
        x.val = y.val;
        y.val = tmp;
    }
}
```
