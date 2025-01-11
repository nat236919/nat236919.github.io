---
title: "Leetcode: Construct K Palindrome Strings"
date: 2025-01-11T15:19:42+08:00
draft: false
categories: ["blog"]
tags: ["blog", "python", "leetcode"]
---

### Introduction

When I first came across the Construct K Palindrome Strings problem, I was intrigued by how it tied together two seemingly unrelated concepts: palindromes and character frequencies. It's not just about solving a puzzle—it's about understanding the fundamental properties of strings and how they can be rearranged to meet specific constraints.

In this article, I'll walk you through my thought process, how I approached the problem, and the solution that I implemented.

### The Problem

[Link to question](https://leetcode.com/problems/construct-k-palindrome-strings)

Here's the challenge: You're given a string `s` and an integer `k`, and you need to determine if it's possible to split the string into `k` palindrome substrings. Every character in the string must be used exactly once.

**Example:**

```python
Input: s = 'annabelle', k = 2
Output: True
Explanation: You can construct 'anna' and 'belle'.
```

At first glance, this might seem tricky. But once you break it down, the solution becomes surprisingly straightforward.

### My Approach

Let's start with some observations that helped guide my solution:

* If the length of the string is less than `k`, it's game over.
Why? Because you simply don't have enough characters to create `k` substrings, let alone palindromes. So, the first check is:

```python
if len(s) < k: return False
```

* If the length of the string equals `k`, it's an automatic win.
Think about it: each substring can just be a single character. Since every character is its own palindrome, this works out perfectly.

* The real challenge lies in odd-frequency characters.
A palindrome can have at most one character with an odd frequency (it goes in the middle). So, the number of characters with odd frequencies in the string becomes a critical factor. If there are more odd-frequency characters than k, it's impossible to construct k palindromes.

This last observation is the key. Once I realized this, everything else fell into place.

### My Solution

Here's how I implemented the solution in Python:

```python
class Solution:
    def canConstruct(self, s: str, k: int) -> bool:
        # If the length of the string is less than k, it's impossible
        if len(s) < k:
            return False

        # Count characters with odd frequencies
        odd_count = sum(1 for char in set(s) if s.count(char) % 2 != 0)

        # K palindromes can be constructed if odd_count <= k
        return odd_count <= k
```

#### Breaking It Down

Let me explain this step by step:

* Character Frequency Check: I used Python's `set()` to get all unique characters in the string and calculated how many of them have an odd frequency. This is the `odd_count`.

* The Decision: If the `odd_count` is less than or equal to k, we can create the necessary palindromes. Otherwise, it's impossible.

That's it! The logic is clean, and the code is simple.

#### Example Walkthrough

Let's take an example to make things concrete. Say we have `s = 'annabelle'` and `k = 2`.

* **Step 1**: Check if the string length is less than `k`. It's not (length is 9).

* **Step 2**: Count the odd-frequency characters:
  * `'a': 2, 'n': 2, 'b': 1, 'e': 2, 'l': 2`.
  * Only `b` has an odd frequency. So, `odd_count = 1`.

* **Step 3**: Compare `odd_count` with `k`. Since `1 ≤ 2`, the answer is `True`.

We can construct two palindromes: 'anna' and 'belle'.

### Complexity Analysis

Here's why I'm happy with this solution:

* Time Complexity: Counting character frequencies takes O(n). Calculating the odd frequencies takes O(c), where c is the number of unique characters. Since c is bounded (26 for lowercase letters), the overall complexity is O(n).

* Space Complexity: We use a set to store unique characters, so the space complexity is O(c).

{{<image src="https://i.ibb.co/6mjVR5K/leetcode-construct-k-palidromes-nuttaphat.jpg"  alt="Solution Result" position="center">}}

### Reflections and Takeaways

I really enjoyed solving this problem because it's a perfect blend of logic and intuition. The key insight about odd-frequency characters made me appreciate how powerful a simple observation can be. Let me know if you have a different approach or if you found this explanation helpful!
