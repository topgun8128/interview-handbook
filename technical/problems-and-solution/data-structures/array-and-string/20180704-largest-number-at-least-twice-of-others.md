---
description: 'source:leetcode level:easy'
---

# 20180704 Largest Number At Least Twice of Others

## Problem

In a given integer array `nums`, there is always exactly one largest element.

Find whether the largest element in the array is at least twice as much as every other number in the array.

If it is, return the **index** of the largest element, otherwise return -1.

**Example 1:**

```text
Input: nums = [3, 6, 1, 0]
Output: 1
Explanation: 6 is the largest integer, and for every other number in the array x,
6 is more than twice as big as x.  The index of value 6 is 1, so we return 1.
```

**Example 2:**

```text
Input: nums = [1, 2, 3, 4]
Output: -1
Explanation: 4 isn't at least as big as twice the value of 3, so we return -1.
```

**Note:**

1. `nums` will have a length in the range `[1, 50]`.
2. Every `nums[i]` will be an integer in the range `[0, 99]`.

## Solution

### Verbal

Set a doubled boolean parameter to false.  
Go through all the elements from the beginning, assign the current one as max, check if the next one is greater than current one, if so, update max and if it's doubled, set doubled to true, else set to false, if not, check if it's doubled the value, if so, set doubled to true, else, set doubled to false.

### Java

#### Solution 1 - Loop the whole array once, remember the position of doubled max.

#### Efficiency: O\(N\)

```java
class Solution {
    public int dominantIndex(int[] nums) {
        boolean doubled = true;
        int max  = nums[0];
        int maxIdx = 0;
        int curIdx = 1;
        
        while (curIdx < nums.length) {
            int curNum = nums[curIdx];
            
            if (curNum > max) {
                if (curNum < max*2) {
                    doubled = false;
                } else {
                    doubled = true;
                }
                max = curNum;
                maxIdx = curIdx;
            } else {
                if (max < curNum * 2) {
                    doubled = false;
                }
            }
            curIdx++;
        }
        
        if (doubled) {
            return maxIdx;
        } else {
            return -1;
        }
    }
}
```



