---
title: 'Construct Binary Tree From Inorder and Postorder Traversal'
description:
date: 2021-12-26T19:49:26+08:00
image:
math: true
license:
categories:

tags:
  - tree
  - depth-first-search
hidden: false
comments: false
draft: false
---

## 从中序与后序遍历序列构造二叉树

| Category   | Difficulty      | Likes | Dislikes |
| ---------- | --------------- | ----- | -------- |
| algorithms | Medium (72.16%) | 641   | -        |

根据一棵树的中序遍历与后序遍历构造二叉树。

注意: 你可以假设树中没有重复的元素。

例如，给出

```
中序遍历 inorder = [9,3,15,20,7] 后序遍历 postorder = [9,15,7,20,3] 返回如下的二叉树：

    3
   / \
   9 20
    / \
    15 7
```

```java
public class ConstructBinaryTreeFromInorderAndPostorderTraversal {

    private Map<Integer, Integer> indexMap;

    public TreeNode buildTree(int[] inorder, int[] postorder) {
        indexMap = new HashMap<>();
        for (int i = 0; i < inorder.length; i++) {
            indexMap.put(inorder[i], i);
        }
        return buildTree(inorder, postorder, 0, postorder.length - 1, 0, inorder.length - 1);
    }

    private TreeNode buildTree(int[] inorder, int[] postorder, int postorderStart, int postorderEnd, int inorderStart, int inorderEnd) {
        if (postorderStart > postorderEnd) {
            return null;
        }

        int inorderIndex = indexMap.get(postorder[postorderEnd]);
        int size = inorderIndex - inorderStart;
        TreeNode treeNode = new TreeNode(inorder[inorderIndex]);
        // 递归地构造左子树，并连接到根节点

        treeNode.left = buildTree(inorder, postorder, postorderStart, postorderStart + size - 1, inorderStart, inorderIndex - 1);
        // 递归地构造右子树，并连接到根节点
        treeNode.right = buildTree(inorder, postorder, postorderStart + size, postorderEnd - 1, inorderIndex + 1, inorderEnd);
        return treeNode;

    }
}

```
