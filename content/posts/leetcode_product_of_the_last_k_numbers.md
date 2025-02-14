---
title: "Leetcode: Product of the Last K Numbers"
date: 2025-02-14T14:02:56+08:00
draft: false
categories: ["blog"]
tags: ["blog", "python", "leetcode"]
---

### Introduction

This blog post covers a solution to a Leetcode problem that requires designing an algorithm to accept a stream of integers and retrieve the product of the last k integers of the stream. The task is both interesting and practical, showcasing the application of algorithm design for efficiency.

In this article, I'll explain the problem, walk you through my thought process, and share the solution I came up with.

---

### The Problem

[Link to question](https://leetcode.com/problems/product-of-the-last-k-numbers)

Design an algorithm that accepts a stream of integers and retrieves the product of the last k integers of the stream.

Implement the `ProductOfNumbers` class:

- `ProductOfNumbers()` Initializes the object with an empty stream.
- `void add(int num)` Appends the integer `num` to the stream.
- `int getProduct(int k)` Returns the product of the last `k` numbers in the current list. You can assume that always the current list has at least `k` numbers.

The test cases are generated so that, at any time, the product of any contiguous sequence of numbers will fit into a single 32-bit integer without overflowing.

---

#### Examples

**Example 1:**

```raw
Input: ["ProductOfNumbers","add","add","add","add","add","getProduct","getProduct","getProduct","add","getProduct"]
       [[],[3],[0],[2],[5],[4],[2],[3],[4],[8],[2]]

Output: [null,null,null,null,null,null,20,40,0,null,32]

Explanation:
ProductOfNumbers productOfNumbers = new ProductOfNumbers();
productOfNumbers.add(3);        // [3]
productOfNumbers.add(0);        // [3,0]
productOfNumbers.add(2);        // [3,0,2]
productOfNumbers.add(5);        // [3,0,2,5]
productOfNumbers.add(4);        // [3,0,2,5,4]
productOfNumbers.getProduct(2); // return 20. The product of the last 2 numbers is 5 * 4 = 20
productOfNumbers.getProduct(3); // return 40. The product of the last 3 numbers is 2 * 5 * 4 = 40
productOfNumbers.getProduct(4); // return 0. The product of the last 4 numbers is 0 * 2 * 5 * 4 = 0
productOfNumbers.add(8);        // [3,0,2,5,4,8]
productOfNumbers.getProduct(2); // return 32. The product of the last 2 numbers is 4 * 8 = 32
```

---

### My Solution

The goal is to design an algorithm that accepts a stream of integers and retrieves the product of the last k integers of the stream. To achieve this, we can use a prefix product array to efficiently calculate the product of the last k numbers.

#### Thought Process

- **Initialization**: We initialize the `ProductOfNumbers` class with an empty stream. We use a list called `prefix_numbers` to store the prefix products, starting with an initial value of `1`.

- **Adding Numbers**: When a new number is added to the stream:
  - If the number is `0`, it resets the prefix product list to `[1]` because any product involving `0` will be `0`.
  - Otherwise, it appends the product of the last element in `prefix_numbers` and the new number to the list.

- **Getting Product**: To get the product of the last k numbers:
  - If `k` is greater than or equal to the length of `prefix_numbers`, it means there are not enough numbers to calculate the product, so we return `0`.
  - Otherwise, we return the integer division of the last element in `prefix_numbers` by the element at the position `len(prefix_numbers) - 1 - k`.

Here is the implementation of the solution

```python
class ProductOfNumbers:

    def __init__(self):
        self.prefix_numbers = [1]  # Init number with 1

    def add(self, num: int) -> None:
        if num == 0:
            self.prefix_numbers = [1]
        else:
            # Append with a product of last elem * num
            self.prefix_numbers.append(self.prefix_numbers[-1] * num)

    def getProduct(self, k: int) -> int:
        if k >= len(self.prefix_numbers):
            return 0
        return self.prefix_numbers[-1] // self.prefix_numbers[-1 - k]
```

---

### Complexity Analysis

- Time Complexity:
  - The add operation takes `O(1)` time.
  - The getProduct operation takes `O(1)` time.
  - Thus, the overall time complexity is `O(1)` for both operations.

- Space Complexity:
  - We use a prefix product array to store the products, which takes `O(n)` space.
  - Hence, the space complexity is `O(n)`.

This approach ensures that we handle large inputs efficiently by leveraging the properties of the heap data structure to perform the necessary operations.

{{<image src="https://i.ibb.co/SDPrV30H/leetcode-product-the-last-k-numbers-sol.jpg" alt="Solution Result" position="center">}}

---

### Reflections and Takeaways

This problem is a great example of how to efficiently calculate the product of the last k numbers using a prefix product array. By leveraging the properties of the prefix product array, we can efficiently perform the required operations. This approach ensures that we handle large inputs efficiently.

Let me know if you came up with a different approach—I'd love to hear it!
