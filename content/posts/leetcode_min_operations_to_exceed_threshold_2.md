---
title: "Leetcode: Minimum Operations to Exceed Threshold Value II"
date: 2025-02-13T16:12:39+08:00
draft: false
categories: ["blog"]
tags: ["blog", "python", "leetcode"]
---

### Introduction

This blog post covers a solution to a Leetcode problem that requires calculating the minimum number of operations needed so that all elements of an array are greater than or equal to a given threshold value. The task is both interesting and practical, showcasing the application of algorithm design for efficiency.

In this article, I'll explain the problem, walk you through my thought process, and share the solution I came up with.

---

### The Problem

[Link to question](https://leetcode.com/problems/minimum-operations-to-exceed-threshold-value-ii)

You are given a 0-indexed integer array `nums`, and an integer `k`.

In one operation, you will:

- Take the two smallest integers `x` and `y` in `nums`.
- Remove `x` and `y` from `nums`.
- Add `min(x, y) * 2 + max(x, y)` anywhere in the array.

Note that you can only apply the described operation if `nums` contains at least two elements.

Return the minimum number of operations needed so that all elements of the array are greater than or equal to `k`.

---

#### Examples

**Example 1:**

```raw
Input: nums = [2,11,10,1,3], k = 10
Output: 2
Explanation:
- In the first operation, we remove elements 1 and 2, then add 1 * 2 + 2 to nums. nums becomes equal to [4, 11, 10, 3].
- In the second operation, we remove elements 3 and 4, then add 3 * 2 + 4 to nums. nums becomes equal to [10, 11, 10].
At this stage, all the elements of nums are greater than or equal to 10 so we can stop.
It can be shown that 2 is the minimum number of operations needed so that all elements of the array are greater than or equal to 10.
```

**Example 2:**

```raw
Input: nums = [1,1,2,4,9], k = 20
Output: 4
Explanation:
- After one operation, nums becomes equal to [2, 4, 9, 3].
- After two operations, nums becomes equal to [7, 4, 9].
- After three operations, nums becomes equal to [15, 9].
- After four operations, nums becomes equal to [33].
At this stage, all the elements of nums are greater than 20 so we can stop.
It can be shown that 4 is the minimum number of operations needed so that all elements of the array are greater than or equal to 20.
```

---

### My Solution

The goal is to calculate the minimum number of operations needed so that all elements of the array are greater than or equal to k. To achieve this, we can use a min-heap to efficiently get the two smallest elements in each operation.

Here is the implementation of the solution:

```python
class Solution:
    def minOperations(self, nums: List[int], k: int) -> int:
        if not nums or not k:
            return 0

        heapq.heapify(nums)
        res = 0

        while nums[0] < k:
            if len(nums) < 2:
                return -1  # Not possible to reach the threshold

            x = heapq.heappop(nums)
            y = heapq.heappop(nums)
            new_element = min(x, y) * 2 + max(x, y)
            heapq.heappush(nums, new_element)
            res += 1

        return res
```

---

### Complexity Analysis

- Time Complexity:
  - The solution involves heap operations which take `O(log n)` time.
  - In the worst case, we might need to perform `O(n)` operations.
  - Thus, the overall time complexity is `O(n log n)`.

- Space Complexity:
  - We use a heap to store the elements, which takes `O(n)` space.
  - Hence, the space complexity is `O(n)`.

This approach ensures that we handle large inputs efficiently by leveraging the properties of the heap data structure to perform the necessary operations.

{{<image src="https://i.ibb.co/pv1K97n9/leetcode-min-opr-exceed-thres.jpg" alt="Solution Result" position="center">}}

---

### Reflections and Takeaways

This problem is a great example of how to efficiently calculate operations using a heap data structure. By leveraging the properties of the heap, we can efficiently get the two smallest elements and perform the required operations. This approach ensures that we handle large inputs efficiently.

Let me know if you came up with a different approach—I'd love to hear it!
