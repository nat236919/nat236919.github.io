---
title: "Leetcode: Find Unique Binary String"
date: 2025-02-20T13:28:12+08:00
draft: false
categories: ["blog"]
tags: ["blog", "python", "leetcode"]
---

### Introduction

This blog post covers a solution to a Leetcode problem that requires finding a unique binary string that does not appear in a given list of binary strings. The task is both interesting and practical, showcasing the application of algorithm design for efficiency.

In this article, I'll explain the problem, walk you through my thought process, and share the solution I came up with.

---

### The Problem

[Link to question](https://leetcode.com/problems/find-unique-binary-string)

Given an array of binary strings `nums`, return a binary string that does not appear in `nums`. If there are multiple answers, you may return any of them.

---

#### Examples

**Example 1:**

```raw
Input: nums = ["01","10"]
Output: "11"
Explanation: "11" does not appear in nums. "00" would also be correct.
```

**Example 2:**

```raw
Input: nums = ["00","01"]
Output: "11"
Explanation: "11" does not appear in nums. "10" would also be correct.
```

**Example 3:**

```raw
Input: nums = ["111","011","001"]
Output: "101"
Explanation: "101" does not appear in nums. "000", "010", "100", and "110" would also be correct.
```

---

### My Solution

The goal is to find a unique binary string that does not appear in the given list of binary strings. To achieve this, we can use a set to store the integer representations of the binary strings and then generate all possible binary strings of the same length to find one that is not in the set.

#### Thought Process

- **Initialization**: We initialize a set to store the integer representations of the binary strings.

- **Transform Binary to Int**: For each binary string in the input list, convert it to an integer and add it to the set.

- **Generate Possible Numbers**: Generate all possible numbers in the range of the length of the input list plus one.

- **Check for Uniqueness**: For each generated number, check if it is not in the set. If it is not, convert it back to a binary string and return it, ensuring it has the correct length by prepending zeros if necessary.

Here is the implementation of the solution

```python
class Solution:
    def findDifferentBinaryString(self, nums: List[str]) -> str:
        # Transform binary to int
        ints = set()
        for num in nums:
            ints.add(int(num, 2))

        # Generate all possible numbers in range of nums
        num_length = len(nums)
        for num in range(2 ** num_length):
            if num not in ints:
                bin_num = bin(num)[2:]  # Get rid of '0b' prefix
                return bin_num.zfill(num_length)  # Prepend 0's until length

        return ''
```

---

### Complexity Analysis

- Time Complexity:
  - The transformation of binary strings to integers takes `O(n)` time, where `n` is the number of strings.
  - The generation and checking of possible numbers take `O(2^m)` time, where `m` is the length of the binary strings.
  - Thus, the overall time complexity is `O(n + 2^m)`.

- Space Complexity:
  - We use a set to store the integer representations of the binary strings, which takes `O(n)` space.
  - Hence, the space complexity is `O(n)`.

This approach ensures that we handle the problem efficiently by leveraging the properties of sets and binary string manipulation.

{{<image src="https://i.ibb.co/95hNBx6/leetcode-find-unique-binary-string.jpg" alt="Solution Result" position="center">}}

---

### Reflections and Takeaways

This problem is a great example of how to efficiently find a unique binary string using set operations and binary string manipulation. By leveraging these properties, we can efficiently perform the required operations. This approach ensures that we handle large inputs efficiently.

Let me know if you came up with a different approach—I'd love to hear it
