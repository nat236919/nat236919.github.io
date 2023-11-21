---
title: "Leetcode: Minimum Amount of Time to Collect Garbage"
date: 2023-11-21T16:44:03+08:00
draft: false
categories: ["blog"]
tags: ["blog", "python", "leetcode"]
---

### Introduction
In the bustling city of algorithms and code, even garbage collection becomes an intriguing problem. This blog post delves into the LeetCode problem titled "Minimum Amount of Time to Collect Garbage." The objective is to efficiently plan the collection of metal ('M'), paper ('P'), and glass ('G') garbage spread across houses using three specialized trucks.

### The Problem

[Link to question](https://leetcode.com/problems/minimum-amount-of-time-to-collect-garbage)

You are provided with an array of strings, garbage, where each element represents the assortment of garbage at a particular house. Additionally, an array travel indicates the time needed to travel from one house to the next.

Three garbage trucks are available, each responsible for collecting one type of garbage. These trucks start at house 0 and must visit each house in order, but they don't necessarily have to stop at every house. The task is to determine the minimum number of minutes required to pick up all the garbage.

### The Solution Approach

Let's break down the provided Python solution:

```python
class Solution:

    def __init__(self):
        self.garbage = []
        self.travel = []

    def garbage_collection(self, garbage: List[str], travel: List[int]) -> int:
        self.garbage = garbage
        self.travel = travel
        return self.get_total_coll_time_from_type('M') + self.get_total_coll_time_from_type('P') + self.get_total_coll_time_from_type('G')
    
    def get_total_coll_time_from_type(self, g_type):
        count = 0
        max_travel_idx = 0
        for idx, garbage_type in enumerate(self.garbage):
            if g_type in garbage_type:
                count += int(garbage_type.count(g_type))
                max_travel_idx = max_travel_idx if max_travel_idx > idx else idx
        
        if count > 0 and max_travel_idx:
            count += sum(self.travel[i] for i in range(max_travel_idx))

        return count
```

#### Initialization
The Solution class has an __init__ method to initialize instance variables garbage and travel. These variables will store the city's garbage distribution and travel times.

##### garbage_collection Method
This method takes garbage and travel arrays as input and calculates the total time required for garbage collection. It achieves this by summing up the times for collecting metal ('M'), paper ('P'), and glass ('G') garbage, calling the get_total_coll_time_from_type method for each type.

##### get_total_coll_time_from_type Method
This method calculates the total collection time for a specific type of garbage. It iterates through the houses, counting the occurrences of the given garbage type and updating the maximum travel index. After the iteration, it adds the travel times for reaching the farthest house with the specified garbage type.

### Results and Analysis

The **garbage_collection** method calls get_total_coll_time_from_type three times, once for each type of garbage. In the worst case, each call to get_total_coll_time_from_type iterates through all the houses, resulting in a time complexity of O(N), where N is the number of houses.

The **get_total_coll_time_from_type** method iterates through the garbage array once. Inside the loop, it performs constant-time operations. Therefore, the overall time complexity of this method is O(N), where N is the number of houses.

Considering these factors, the overall time complexity of the solution is O(3N), which simplifies to O(N).

{{<image src="https://i.ibb.co/GQ5M9Db/garbage-coll-sol.jpg"  alt="Solution Result" position="center">}}

### Conclusion
The solution provides an efficient approach to minimize the time required for garbage collection in our algorithmic city. It demonstrates the application of thoughtful algorithm design, modular code structure, and a Big-O analysis to understand its efficiency.

Optimizing garbage collection may be a fictional scenario in this context, but it reflects the real-world applications of algorithmic problem-solving. This problem serves as a captivating example of how programming principles can be applied to diverse challenges, even ones as unique as garbage collection in a city of code.
