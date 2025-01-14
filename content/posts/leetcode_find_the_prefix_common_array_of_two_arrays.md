---
title: "Leetcode: Find the Prefix Common Array of Two Arrays"
date: 2025-01-14T15:34:07+08:00
draft: false
categories: ["blog"]
tags: ["blog", "python", "leetcode"]
---

### Introduction

The **Prefix Common Array** problem is a fascinating puzzle that combines array manipulation and set operations. It requires you to find common elements in two arrays at every index and maintain a running count. While the problem may seem tricky at first, breaking it down into smaller steps reveals a very simple approach.

In this article, I'll explain the problem, walk you through my thought process, and share the solution I came up with.

---

### The Problem

[Link to question](https://leetcode.com/problems/find-the-prefix-common-array-of-two-arrays)

You are given two 0-indexed integer permutations **A** and **B** of length **n**. A **prefix common array** of **A** and **B** is an array **C** where `C[i]` equals the count of numbers that are present at or before index `i` in both **A** and **B**.

**Input Constraints:**

1. Both **A** and **B** are permutations of integers from 1 to **n**.
2. Both arrays are of equal length.

**Output:**
Return the prefix common array **C**.

---

#### Examples

**Example 1:**

```python
Input: A = [1,3,2,4], B = [3,1,2,4]
Output: [0,2,3,4]
```

**Explanation:**

* At i = 0, no numbers are common → C[0] = 0.
* At i = 1, numbers 1 and 3 are common → C[1] = 2.
* At i = 2, numbers 1, 2, and 3 are common → C[2] = 3.
* At i = 3, all numbers (1, 2, 3, 4) are common → C[3] = 4.

**Example 2:**

```python
Input: A = [2,3,1], B = [3,1,2]
Output: [0,1,3]
```

**Explanation:**

* At i = 0, no numbers are common → C[0] = 0.
* At i = 1, only 3 is common → C[1] = 1.
* At i = 2, numbers 1, 2, and 3 are all common → C[2] = 3.

---

### My Solution

The main idea is to maintain a running count of common elements between the two arrays as you iterate through their prefixes. To do this:

Use a set to keep track of all numbers you’ve seen so far in either array. This helps us efficiently check whether a number in one array is already present in the other.

As you iterate over the arrays:

* If a number from A or B is already in the set, it means it has appeared in both arrays, so you increment the count of common elements.
* If the number hasn’t been seen yet, add it to the set.
* After processing the current index, append the running count of common elements to the result array C.

This approach is efficient because set operations (like adding or checking for membership) are average O(1).

```python
from typing import List

class Solution:
    def findThePrefixCommonArray(self, A: List[int], B: List[int]) -> List[int]:
        res = []  # Result array to store the prefix common counts
        seen = set()  # Set to track all elements we've encountered so far
        common_count = 0  # Counter for common elements

        # Iterate through both arrays simultaneously
        for a, b in zip(A, B):
            # Process element from A
            if a in seen:
                common_count += 1  # a is common between A and B
            else:
                seen.add(a)

            # Process element from B
            if b in seen:
                common_count += 1  # b is common between A and B
            else:
                seen.add(b)

            # Append the current count of common elements to the result array
            res.append(common_count)

        return res
```

---

### Complexity Analysis

The solution is efficient and scales well with input size:

* Time Complexity:
  * Iterating through the arrays takes O(n).
  * Set operations (like adding and checking membership) are O(1) on average.
  * Thus, the overall time complexity is O(n).

* Space Complexity:
  * We use a set to track elements, which at most contains n elements (the size of the arrays).
  * Hence, the space complexity is O(n).

{{<image src="https://i.ibb.co/tQMgv1Q/leetcode-prefix-common-arr-of-two-arrs.jpg"  alt="Solution Result" position="center">}}

---

### Reflections and Takeaways

This problem is a great example of how we can use sets to track relationships between elements in arrays. I found it fascinating how simple operations like checking membership and counting can combine to solve a seemingly complex problem. Let me know if you came up with a different approach—I’d love to hear it!
