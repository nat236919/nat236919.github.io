---
title: "Leetcode: Minimum Number of Operations to Move All Balls to Each Box"
date: 2025-01-25T15:58:47+08:00
draft: false
categories: ["blog"]
tags: ["blog", "python", "leetcode"]
---

### Introduction

This blog post covers a solution to a Leetcode problem that requires calculating the minimum number of operations needed to move all balls to each box. The task is both interesting and practical, showcasing the application of algorithm design for efficiency.

In this article, I'll explain the problem, walk you through my thought process, and share the solution I came up with.

---

### The Problem

[Link to question](https://leetcode.com/problems/minimum-number-of-operations-to-move-all-balls-to-each-box)

Given a binary string `boxes` where `boxes[i] == '1'` represents a ball in the `i-th` box, and `boxes[i] == '0'` represents an empty box, calculate the minimum number of operations required to move all balls to each box.

Each operation consists of moving a ball from one box to an adjacent box. The goal is to output an array where the `i-th` element is the total number of operations required to move all balls to the `i-th` box.

---

#### Examples

**Example 1:**

```raw
Input: boxes = "110"
Output: [1, 1, 3]
Explanation:

- For box 0: 1 operation is needed (move ball from box 1 to box 0).
- For box 1: 1 operation is needed (move ball from box 0 to box 1).
- For box 2: 3 operations are needed (move balls from box 0 and box 1 to box 2).
```

**Example 2:**

```raw
Input: boxes = "001011"
Output: [11, 8, 5, 4, 3, 4]
```

---

### My Solution

The goal is to calculate the minimum number of operations required to move all balls to each box. Given a binary string boxes, where `boxes[i] == '1'` represents a ball in the `i-th` box and `boxes[i] == '0'` represents an empty box, we need to output an array where the `i-th` element is the total number of operations required to move all balls to the `i-th` box.

To achieve this, we can use a two-pass approach:

**Left-to-Right Pass:**

* We iterate through the string from left to right.
* We keep track of the number of balls encountered so far (`ball_count`) and the total number of moves required to bring those balls to the current box (`ball_moves`).
* For each box, we update the result array with the current `ball_moves`.
* If the current box contains a ball (`boxes[idx] == '1'`), we increment the `ball_count`.
* We then update `ball_moves` by adding `ball_count` to it.

**Right-to-Left Pass:**

* We iterate through the string from right to left.
* We use the same logic as the left-to-right pass to update the result array.
* This ensures that we account for the moves required to bring balls from both directions.
* Here is the implementation of the solution

```python
class Solution:
    def minOperations(self, boxes: str) -> List[int]:
        if not boxes:
            return []

        res = [0] * len(boxes)

        # Pass left to right
        self.passing_ball_count(res, boxes)

        # Pass right to left
        self.passing_ball_count(res, boxes, reverse_direction=True)

        return res

    def passing_ball_count(
        self, res: List[int], boxes: str, reverse_direction: bool = False
    ):
        """
        Update the result array by calculating ball movements in one direction.

        Args:
            res (List[int]): The result array to update.
            boxes (str): The input string representing boxes with balls.
            reverse_direction (bool): Whether to iterate from right to left.
        """
        ball_count, ball_moves = 0, 0
        box_range = (
            range(len(boxes) - 1, -1, -1) if reverse_direction else range(len(boxes))
        )

        for idx in box_range:
            res[idx] += ball_moves
            if boxes[idx] == '1':
                ball_count += 1
            ball_moves += ball_count
```

---

### Complexity Analysis

* Time Complexity:
  * The solution involves two passes over the input string, each taking `O(n)` time.
  * Thus, the overall time complexity is `O(n)`.

* Space Complexity:
  * We use a constant amount of extra space for the result array
  * Hence, the space complexity is `O(n)`

This approach ensures that we handle large inputs efficiently by leveraging the properties of the problem and making two linear passes to accumulate the necessary operations.

{{ <image src="https://i.ibb.co/TvPQCHx/leetcode-passing-ball-count.jpg"  alt="Solution Result" position="center"> }}

---

### Reflections and Takeaways

This problem is a great example of how to efficiently calculate operations in a linear pass by leveraging the properties of the problem. By making two passes (left-to-right and right-to-left), we can accumulate the necessary operations in an optimal manner. This approach ensures that we handle large inputs efficiently.

Let me know if you came up with a different approach—I’d love to hear it!
