---
description: 'source:leetcode level:easy'
---

# 20190704 Find Pivot Index

## Problem

Given an array of integers `nums`, write a method that returns the "pivot" index of this array.

We define the pivot index as the index where the sum of the numbers to the left of the index is equal to the sum of the numbers to the right of the index.

If no such index exists, we should return -1. If there are multiple pivot indexes, you should return the left-most pivot index.

**Example 1:**  


```text
Input: 
nums = [1, 7, 3, 6, 5, 6]
Output: 3
Explanation: 
The sum of the numbers to the left of index 3 (nums[3] = 6) is equal to the sum of numbers to the right of index 3.
Also, 3 is the first index where this occurs.
```

**Example 2:**  


```text
Input: 
nums = [1, 2, 3]
Output: -1
Explanation: 
There is no index that satisfies the conditions in the problem statement.
```

**Note:**

*  The length of `nums` will be in the range `[0, 10000]`.
*  Each element `nums[i]` will be an integer in the range `[-1000, 1000]`.

## Solution

### Verbal

### Java 

#### Solution 1

```java
class Solution {
    public int pivotIndex(int[] nums) {
        int sumleft = 0;
        int sumright = 0;
        int curIdx = 0;
        
        // First, get the sum of all right elements first
        for (int i=1; i < nums.length; i++) {
            sumright += nums[i];
        }
        
        // Subtract the values for right one by one, add values for left one by one until they're the same
        boolean matched = false;
        while (curIdx < nums.length) {
            if (sumleft == sumright) {
                matched = true;
                break;
            }
            sumleft += nums[curIdx];
            curIdx++;
            if (curIdx < nums.length) {
                sumright -= nums[curIdx];
            }
        }
        if (matched) {
            return curIdx;
        } else {
            return -1;
        }
        
    }
}
```

### Python 2

\[TODO\]

