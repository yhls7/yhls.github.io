---
title: 使用JS模拟二叉树并添加数组序列化方法
description: 
categories: technology
tags: Algorithm
---

### 什么是二叉树？
1.常见的一种树结构
2.每个节点最多只有两个分支
3.分支具有左右次序，不能颠倒

### 使用JS模拟
```javascript
class TreeNode {
  constructor (value) {
    this.value = value || null;
    this.left = null;
    this.right = null;
  }
}

class BinaryTree {
  static serialize(list = []) {
    const root = new TreeNode(list[0]);
    const queue = [root];
    let cursor = 1;
    while (cursor < list.length) {
      const node = queue.shift();
      const leftValue = list[cursor];
      const rightValue = list[cursor + 1];
      if (leftValue) {
        const leftNode = new TreeNode(leftValue);
        node.left = leftNode;
        queue.push(leftNode);
      }
      if (rightValue) {
        const rightNode = new TreeNode(rightValue);
        node.right = rightNode;
        queue.push(rightNode);
      }
      cursor += 2;
    }
    const tree = new BinaryTree(root);
    return tree;
  }
  constructor (root) {
    this.root = root;
  }
  deserialize() {
    const queue = [this.root];
    const res = [];
    while (queue.length) {
      const node = queue.shift();
      if (node) {
        res.push(node.value);
        queue.push(node.left);
        queue.push(node.right);
      } else {
        res.push(null);
      }
    }
    return res;
  }
}

const arr = [1, 2, 3, null, null, 6, 7];
const tree = BinaryTree.serialize(arr);
const list = tree.deserialize();
console.log(arr, tree, list);
```
