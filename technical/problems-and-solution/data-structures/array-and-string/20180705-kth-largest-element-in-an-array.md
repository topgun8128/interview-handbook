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

**Method 4 \(QuickSelect\)**   
This is an optimization over method 1 if [QuickSort ](http://geeksquiz.com/quick-sort/)is used as a sorting algorithm in first step. In QuickSort, we pick a pivot element, then move the pivot element to its correct position and partition the array around it. The idea is, not to do complete quicksort, but stop at the point where pivot itself is k’th largest element. Also, not to recur for both left and right sides of pivot, but recur for one of them according to the position of pivot. The worst case time complexity of this method is **O\(n2\)**, but it works in **O\(n\) on average**.

### Java

#### Solution 1 - Using Default Sorting

#### Efficiency: O\(N log N\)

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        Arrays.sort(nums);
        return nums[nums.length-k];
    }
}
```

#### Solution 2 - Using Selection Sort up to the kth largest element

#### Efficiency: Average: O\(2N\), Max: O\(N^2\)

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        
        // Perform Selection Sort until the kth largest one, then stop
        int len = nums.length;
        int maxElem;
        for (int i=0; i < k; i++) {
            // Assume the first one is the largest
            maxElem = i;
            
            // for the remaining unsorted items
            for (int j = i+1; j < len; j++) {
                if (nums[maxElem] < nums[j]) {
                    maxElem = j;
                }
            }
            swap(nums, i, maxElem);
        }
        
        return nums[k-1];
        
    }
    
    public void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

#### Solution 2b - Improved version to use Selection Sort in opposite way if k is larger than half of the length of array

#### Efficiency: Average: O\(2N\), Max: O\(N^2\)

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        int len = nums.length;
        if (k * 2 > len) {
            // Perform Selection Sort until the len - kth smallest one, then stop
            int reverseK = len - k + 1;
            int minElem;
            for (int i=0; i < reverseK; i++) {
                // Assume the first one is the smallest
                minElem = i;

                // for the remaining unsorted items
                for (int j = i+1; j < len; j++) {
                    if (nums[minElem] > nums[j]) {
                        minElem = j;
                    }
                }
                swap(nums, i, minElem);
            }

            return nums[reverseK-1]; 
        } else {
            // Perform Selection Sort until the kth largest one, then stop
            int maxElem;
            for (int i=0; i < k; i++) {
                // Assume the first one is the largest
                maxElem = i;

                // for the remaining unsorted items
                for (int j = i+1; j < len; j++) {
                    if (nums[maxElem] < nums[j]) {
                        maxElem = j;
                    }
                }
                swap(nums, i, maxElem);
            }

            return nums[k-1]; 
        }
    }
    
    public void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```



