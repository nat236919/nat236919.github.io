---
title: "Leetcode: Maximum Subarray"
date: 2024-12-11T17:21:03+08:00
draft: false
categories: ["blog"]
tags: ["blog", "python", "leetcode"]
---

### Introduction

The **Maximum Subarray** problem is a classic challenge in computer science and algorithm design. It provides a great exercise for understanding array manipulation, optimization techniques, and dynamic programming. In this article, I'll delve into the problem statement, explore solution approaches, and analyze their efficiencies.

### The Problem

[Link to question](https://leetcode.com/problems/maximum-subarray)

Given an integer array **nums**, find the contiguous subarray (containing at least one number) that has the largest sum and return its sum.

**Example:**

```python
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: The subarray [4,-1,2,1] has the largest sum = 6.
```

### The Solution Approach

There are multiple ways to tackle this problem, ranging from a straightforward brute force method to the highly efficient Kadane's Algorithm.

#### Brute Force Method

The brute force approach computes the sum of every possible subarray and tracks the maximum sum encountered.

**Algorithm:**

* Iterate over all possible starting indices of subarrays (**i**).
* For each starting index, iterate over all possible ending indices (**j**).
* Calculate the sum of the subarray from **i** to **j**.
* Update the maximum sum (**max_sum**) if the current subarray sum is larger.
* Return **max_sum**.

```python
class Solution:
    def maxSubArray(self, nums):
        max_sum = float('-inf')
        for i in range(len(nums)):
            for j in range(i, len(nums)):
                cur_sum = sum(nums[i:j+1])
                max_sum = max(max_sum, cur_sum)
        return max_sum
```

Time Complexity: **O(n²)** due to the nested loops.
Space Complexity: **O(1)** since no extra data structures are used.

#### Dynamic Programming Method

Kadane's Algorithm is an efficient way to solve the problem in linear time by dynamically updating the maximum subarray sum ending at each index.

**Algorithm:**

* Initialize two variables:
  * **cur_sum**: Tracks the sum of the current subarray.
  * **max_sum**: Tracks the maximum sum encountered so far.
* Traverse the array:
  * At each element, decide whether to include it in the current subarray or start a new subarray.
  * Update **cur_sum** to the larger of the current element (n) or the sum of the current element and **cur_sum** (**cur_sum + n**).
  * Update **max_sum** to be the maximum of **cur_sum** and **max_sum**.
* Return **max_sum** after traversing the array.

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        cur_sum, max_sum = 0, float(-inf)
        for n in nums:
            cur_sum = n if n > cur_sum + n else cur_sum + n  # max()
            max_sum = cur_sum if cur_sum > max_sum else max_sum  # max()
        return max_sum
```

Time Complexity: **O(n)** as the array is traversed once.
Space Complexity: **O(1)**.

### Results and Analysis

Kadane's Algorithm is a significant improvement over the brute force approach. It reduces the time complexity from **O(n²)** to **O(n)**, making it suitable for large datasets.

{{<image src="https://i.ibb.co/2NY1j1K/max-subarr.jpg"  alt="Solution Result" position="center">}}

### Conclusion

The **Maximum Subarray** problem highlights the importance of optimization and choosing the right algorithm for a given problem. While the brute force approach is straightforward, it's impractical for large inputs. Kadane's Algorithm, on the other hand, showcases how dynamic programming principles can greatly enhance efficiency.
