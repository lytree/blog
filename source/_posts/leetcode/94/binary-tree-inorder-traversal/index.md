---
title: 'Binary Tree Inorder Traversal'
description:
date: 2021-11-21T16:16:04+08:00
image:
math: true
license:
categories:
  - leetcode
tags:
  - hash-table
  - stack
  - tree
hidden: false
comments: false
draft: false
---

## 二叉树的中序遍历

| Category   | Difficulty    | Likes | Dislikes |
| ---------- | ------------- | ----- | -------- |
| algorithms | Easy (75.57%) | 1170  | -        |

- Tags
  - hash-table
  - stack
  - tree

Companies microsoft

给定一个二叉树的根节点 root ，返回它的 中序 遍历。

示例 1：

```
输入：root = [1,null,2,3] 输出：[1,3,2]
```

示例 2：

```
输入：root = [] 输出：[]
```

示例 3：

```
输入：root = [1] 输出：[1]
```

示例 4：

```
输入：root = [1,2] 输出：[2,1]
```

示例 5：

```
输入：root = [1,null,2] 输出：[1,2]
```

> 提示：树中节点数目在范围 [0, 100] 内-100 <= Node.val <= 100

> 进阶: 递归算法很简单，你可以通过迭代算法完成吗？

中序遍历顺序 左-中-右

```java

class TreeNode {

    int val;
    TreeNode left;
    TreeNode right;

    TreeNode() {
    }

    TreeNode(int val) {
        this.val = val;
    }

    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

class Solution {

    public List<Integer> inorderTraversal(TreeNode root) {
        ArrayList<Integer> integers = new ArrayList<>();
        Deque<TreeNode> stack = new LinkedList<>();
        while (root != null || !stack.isEmpty()) {
            while (root != null) {
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            integers.add(root.val);
            root = root.right;
        }
        return integers;
    }
}
```
