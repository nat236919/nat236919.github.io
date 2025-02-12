---
title: "Leetcode: Max Sum of a Pair With Equal Sum of Digits"
date: 2025-02-12T13:47:59+08:00
draft: false
categories: ["blog"]
tags: ["blog", "python", "leetcode"]
---

### Introduction

This blog post covers a solution to a Leetcode problem that requires finding the maximum sum of a pair of numbers with equal sum of digits. The task is both interesting and practical, showcasing the application of algorithm design for efficiency.

In this article, I'll explain the problem, walk you through my thought process, and share the solution I came up with.

---

### The Problem

[Link to question](https://leetcode.com/problems/max-sum-of-a-pair-with-equal-sum-of-digits)

You are given a 0-indexed array `nums` consisting of positive integers. You can choose two indices `i` and `j`, such that `i != j`, and the sum of digits of the number `nums[i]` is equal to that of `nums[j]`.

Return the maximum value of `nums[i] + nums[j]` that you can obtain over all possible indices `i` and `j` that satisfy the conditions.

---

#### Examples

**Example 1:**

```raw
Input: nums = [18,43,36,13,7]
Output: 54
Explanation: The pairs (i, j) that satisfy the conditions are:
- (0, 2), both numbers have a sum of digits equal to 9, and their sum is 18 + 36 = 54.
- (1, 4), both numbers have a sum of digits equal to 7, and their sum is 43 + 7 = 50.
So the maximum sum that we can obtain is 54.
```

**Example 2:**

```raw
Input: nums = [10,12,19,14]
Output: -1
Explanation: There are no two numbers that satisfy the conditions, so we return -1.
```

---

### My Solution

To solve this problem, we can use a dictionary to group numbers by their digit sum. Then, for each group with at least two numbers, we find the two largest numbers and calculate their sum. The maximum sum across all groups is the result.

Here is the implementation of the solution:

```python
from typing import List
from collections import defaultdict
import heapq

class Solution:
    def maximumSum(self, nums: List[int]) -> int:
        groups_by_digit_sum = defaultdict(list)

        # Group numbers by digit sum
        for num in nums:
            digit_sum = sum(map(int, str(num)))
            groups_by_digit_sum[digit_sum].append(num)

        max_sum = -1

        # Find max sum from groups with at least two numbers
        for group in groups_by_digit_sum.values():
            if len(group) >= 2:
                largest_two = heapq.nlargest(2, group)  # Get the two largest numbers
                max_sum = max(max_sum, sum(largest_two))

        return max_sum
```

---

### Complexity Analysis

* Time Complexity:
  * The solution involves iterating over the input array and grouping numbers by their digit sum, which takes `O(n)` time.
  * Finding the two largest numbers in each group takes `O(k log k)` time, where `k` is the size of the group.
  * Thus, the overall time complexity is `O(n log k)`.

* Space Complexity:
  * We use extra space for the dictionary to store groups of numbers.
  * Hence, the space complexity is `O(n)`

This approach ensures that we handle large inputs efficiently by leveraging the properties of the problem and using a dictionary to group numbers by their digit sum.

{{<image src="https://i.ibb.co/4RxrmPBw/leetcode-max-sum-pair-digit.jpg" alt="Solution Result" position="center">}}

---

### Reflections and Takeaways

This problem is a great example of how to efficiently calculate operations by leveraging the properties of the problem. By grouping numbers and finding the two largest in each group, we can accumulate the necessary operations in an optimal manner. This approach ensures that we handle large inputs efficiently.

Let me know if you came up with a different approach—I'd love to hear it!
