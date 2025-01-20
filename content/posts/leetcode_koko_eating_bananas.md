---
title: "Leetcode: Koko Eating Bananas"
date: 2025-01-20T12:10:36+08:00
draft: false
categories: ["blog"]
tags: ["blog", "python", "leetcode"]
---

### Introduction

The **Koko Eating Bananas** problem is an interesting challenge that involves binary search and array manipulation. The goal is to determine the minimum eating speed `k` such that Koko can eat all the bananas within a given number of hours `h`.

In this article, I'll explain the problem, walk you through my thought process, and share the solution I came up with.

---

### The Problem

[Link to question](https://leetcode.com/problems/koko-eating-bananas)

You are given an array `piles` where `piles[i]` represents the number of bananas in the `i-th` pile. Koko can decide her bananas-per-hour eating speed `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during that hour.

Return the minimum integer `k` such that she can eat all the bananas within `h` hours.

**Input Constraints:**

1. `1 <= piles.length <= 10^4`
2. `piles.length <= h <= 10^9`
3. `1 <= piles[i] <= 10^9`

---

#### Examples

**Example 1:**

```python
Input: piles = [3,6,7,11], h = 8
Output: 4
```

**Example 2:**

```python
Input: piles = [30,11,23,4,20], h = 5
Output: 30
```

**Example 3:**

```python
Input: piles = [30,11,23,4,20], h = 6
Output: 23
```

---

### My Solution

The main idea is to use binary search to find the minimum eating speed k. To do this:

* Define the Search Range:

  * The minimum possible eating speed is 1 (Koko eats at least one banana per hour).
  * The maximum possible eating speed is the maximum number of bananas in any pile (max(piles)).

* Binary Search:

  * Calculate the middle point of the current range (try_hour).
  * Calculate the total hours Koko would spend eating all the bananas at this speed.
  * If the total hours is less than or equal to h, update the result and narrow the search to the left half.
  * If the total hours is more than h, narrow the search to the right half.

* Return the Result:

  * The result will be the minimum eating speed that allows Koko to eat all the bananas within h hours.

```python
class Solution:
    def minEatingSpeed(self, piles: List[int], h: int) -> int:
        # Input check
        if not piles or not h:
            return 0

        l, r = 1, max(piles)
        min_hours = r

        while l <= r:
            try_hour = l + ((r - l) // 2)
            hours_spent = 0
            for pile in piles:
                hours_spent += math.ceil(pile / try_hour)
                if hours_spent > h:
                    break

            if hours_spent <= h:
                min_hours = min(min_hours, try_hour)
                r = try_hour - 1
            else:
                l = try_hour + 1
```

---

### Complexity Analysis

The solution is efficient and scales well with input size:

* Time Complexity:
  * Binary search takes O(log(max(piles))).
  * For each candidate `k`, we check if Koko can finish the bananas in O(n) time.
  * Thus, the overall time complexity is O(n log(max(piles))).

* Space Complexity:
  * We use a constant amount of extra space.
  * Hence, the space complexity is O(1).

{{<image src="https://i.ibb.co/qRQPZN6/leetcode-koko.jpg"  alt="Solution Result" position="center">}}

---

### Reflections and Takeaways

This problem is a great example of how binary search can be applied to optimization problems. I found it fascinating how we can efficiently narrow down the search space to find the optimal solution. Let me know if you came up with a different approach—I'd love to hear it!
