---
description: 'source:leetcode level:medium'
---

# 20180705 Kth Largest Element in an Array

{% embed data="{\"url\":\"https://leetcode.com/problems/kth-largest-element-in-an-array/description/\",\"type\":\"link\",\"title\":\"Kth Largest Element in an Array - LeetCode\",\"description\":\"Can you solve this problem? 🤔\",\"icon\":{\"type\":\"icon\",\"url\":\"https://leetcode.com/favicon-192x192.png\",\"width\":192,\"height\":192,\"aspectRatio\":1},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://leetcode.com/static/images/LeetCode\_Sharing.png\",\"width\":500,\"height\":260,\"aspectRatio\":0.52}}" %}

## Problem

Find the **k**th largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

**Example 1:**

```text
Input: [3,2,1,5,6,4] and k = 2
Output: 5
```

**Example 2:**

```text
Input: [3,2,3,1,2,4,5,5,6] and k = 4
Output: 4
```

**Note:**   
You may assume k is always valid, 1 ≤ k ≤ array's length.

## Solution

### Verbal

**Method 1 \(Simple Solution\)**   
A Simple Solution is to sort the given array using a O\(nlogn\) sorting algorithm like [Merge Sort](http://geeksquiz.com/merge-sort/), [Heap Sort](http://geeksquiz.com/heap-sort/), etc and return the element at index k-1 in the sorted array. Time Complexity of this solution is O\(nLogn\).

**Method 2 \(Using Min Heap – HeapSelect\)**  
We can find k’th largest element in time complexity better than O\(nlogn\). A simple optomization is to create a [Max Heap ](http://geeksquiz.com/binary-heap/)of the given n elements and call extractMax\(\) k times.

### Java

#### Solution 1 - Efficiency: 

