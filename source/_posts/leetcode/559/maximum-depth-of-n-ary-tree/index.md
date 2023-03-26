---
title: 'Maximum Depth of N Ary Tree'
description:
date: 2021-11-21T16:42:30+08:00
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

## N 叉树的最大深度

| Category   | Difficulty    | Likes | Dislikes |
| ---------- | ------------- | ----- | -------- |
| algorithms | Easy (72.85%) | 226   | -        |

- Tags
  - tree

给定一个 N 叉树，找到其最大深度。

最大深度是指从根节点到最远叶子节点的最长路径上的节点总数。

N 叉树输入按层序遍历序列化表示，每组子节点由空值分隔（请参见示例）。

示例 1：

```
输入：root = [1,null,3,2,4,null,5,6]
输出：3
```

示例 2：

```
输入：root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
输出：5
```

> 提示：树的深度不会超过 1000 。树的节点数目位于 [0, 104] 之间。

## 深度搜索

```java
class Node {

    public int val;
    public List<Node> children;

    public Node() {
    }

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
}


class Solution {

    public int maxDepth(Node root) {
        if (root == null) {
            return 0;
        }
        int maxDepth = 0;
        List<Node> children = root.children;
        for (Node node : children) {
            int childDepth = maxDepth(node);
            maxDepth = Math.max(maxDepth, childDepth);
        }
        return maxDepth + 1;
    }
}
```

## 广度搜索

广度优先搜索的队列里存放的是「当前层的所有节点」。每次拓展下一层的时候，不同于广度优先搜索的每次只从
队列里拿出一个节点，我们需要将队列里的所有节点都拿出来进行拓展，这样能保证每次拓展完的时候队列里存放
的是当前层的所有节点，即我们是一层一层地进行拓展。最后我们用一个变量 $\textit{ans}$ 来维护拓展的次数
，该 $N$ 叉树的最大深度即为 $\textit{ans}$。

```java
class Node {

    public int val;
    public List<Node> children;

    public Node() {
    }

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
}


class Solution {

    public int maxDepth(Node root) {
        if (root == null) {
            return 0;
        }
        //记录广度遍历每一层的所有节点
        Queue<Node> queue = new LinkedList<>();
        queue.offer(root);
        int result = 0;
        while (!queue.isEmpty()) {
            //广度遍历的每层节点书
            int size = queue.size();
            while (size > 0) {
                Node node = queue.poll();//出队
                List<Node> children = node.children;
                for (Node child : children) {
                    queue.offer(child);
                }
                size--;
            }
            result++;
        }
        return result;
    }
}

```
