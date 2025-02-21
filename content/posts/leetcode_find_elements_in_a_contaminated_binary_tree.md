---
title: "Leetcode: 1261. Find Elements in a Contaminated Binary Tree"
date: 2025-02-21T16:08:49+08:00
draft: false
categories: ["blog"]
tags: ["blog", "python", "leetcode"]
---

### Introduction

This blog post covers a solution to a Leetcode problem that requires finding elements in a contaminated binary tree. The task is both interesting and practical, showcasing the application of algorithm design for efficiency.

In this article, I'll explain the problem, walk you through my thought process, and share the solution I came up with.

---

### The Problem

[Link to question](https://leetcode.com/problems/find-elements-in-a-contaminated-binary-tree)

Given a binary tree with the following rules:

- `root.val == 0`
- For any `treeNode`:
  - If `treeNode.val` has a value `x` and `treeNode.left != null`, then `treeNode.left.val == 2 * x + 1`
  - If `treeNode.val` has a value `x` and `treeNode.right != null`, then `treeNode.right.val == 2 * x + 2`

Now the binary tree is contaminated, which means all `treeNode.val` have been changed to `-1`.

Implement the `FindElements` class:

- `FindElements(TreeNode* root)` Initializes the object with a contaminated binary tree and recovers it.
- `bool find(int target)` Returns true if the target value exists in the recovered binary tree.

---

### My Solution

The goal is to recover the binary tree and then check if a target value exists in the recovered tree. To achieve this, we can use a set to store the values of the recovered tree nodes and a recursive function to recover the tree.

#### Thought Process

- **Initialization**: We initialize a set to store the values of the recovered tree nodes.
- **Recover Tree**: Use a recursive function `_recover` to traverse the tree and set the correct values for each node.
- **Find Target**: Check if the target value exists in the set of recovered tree values.

Here is the implementation of the solution:

```python
class FindElements:

    def __init__(self, root: Optional[TreeNode]):
        self.recovered_tree = set()
        self._recover(root, 0)

    def _recover(self, node: Optional[TreeNode], val: int) -> None:
        if not node:
            return

        node.val = val
        self.recovered_tree.add(node.val)

        self._recover(node.left, 2 * val + 1)
        self._recover(node.right, 2 * val + 2)

    def find(self, target: int) -> bool:
        return target in self.recovered_tree
```

---

### Complexity Analysis

- Time Complexity:
  - The recovery process takes `O(n)` time, where `n` is the number of nodes in the tree.
  - The find operation takes `O(1)` time due to the use of a set.

- Space Complexity:
  - We use a set to store the values of the recovered tree nodes, which takes `O(n)` space.

This approach ensures that we handle the problem efficiently by leveraging the properties of sets and recursive tree traversal.

{{<image src="https://i.ibb.co/prGHLbwW/leetcode-find-elements-in-a-contaminated-binary-tree-sol.jpg" alt="Solution Result" position="center">}}

---

### Reflections and Takeaways

This problem is a great example of how to efficiently recover a contaminated binary tree and perform search operations on it. By leveraging the properties of sets and recursive tree traversal, we can efficiently perform the required operations.

Let me know if you came up with a different approach—I'd love to hear it!
