---
description: 'source:hackerearth level:medium'
---

# 20180705 Merge Sort

## Problem

Given an array _A_ on size _N_, you need to find the number of ordered pairs \(i,j\) such that i&lt;j and A\[i\]&gt;A\[j\].

**Input:**  
First line contains one integer, _N_, size of array.  
Second line contains _N_ space separated integers denoting the elements of the array _A_.

**Output:**  
Print the number of ordered pairs \(i,j\) such that i&lt;j and A\[i\]&gt;A\[j\].

**Constraints:**  
1≤N≤10^6  
1≤A\[i\]≤10^6

SAMPLE INPUT 

```text
5
1 4 3 2 5
```

SAMPLE OUTPUT 

```text
3
```

## Solution

### Verbal

Use Merge Sort and everytime when a swap happens, add the number of remaining elements in the left array to count. Finally, return count.

### Java

#### Solution 1 - Efficiency: O\(N log N\)

```java
/* IMPORTANT: Multiple classes and nested static classes are supported */

//imports for BufferedReader
import java.io.BufferedReader;
import java.io.InputStreamReader;

//import for Scanner and other utility classes
import java.util.*;

// Warning: Printing unwanted or ill-formatted data to output will cause the test cases to fail

class Solution {
    public static void main(String args[] ) throws Exception {

        Scanner s = new Scanner(System.in);
        int size = s.nextInt();
        
        int[] nums = new int[size];
        
        for (int i = 0 ; i < size; i++) {
            nums[i] = s.nextInt();
        }
        
        //System.out.println("Given Array");
        //printArray(nums);
        
        Solution solution = new Solution();
        long count = solution.findOrderedPairs(nums);
        
        //System.out.println("Sorted Array");
        //printArray(nums);
        
        System.out.println(count);
        

    }
    
    public static void printArray(int arr[])
    {
        int n = arr.length;
        for (int i=0; i<n; ++i)
            System.out.print(arr[i] + " ");
        System.out.println();
    }
    
    public long findOrderedPairs(int[] nums) {
        return mergeSort(0, nums, 0, nums.length-1);
    }
    
    public long merge(long count, int[] nums, int l, int m, int r) {
        int[] leftArray = Arrays.copyOfRange(nums, l, m+1);
        int[] rightArray = Arrays.copyOfRange(nums, m+1, r+1);
        //System.out.println("LeftArray: ");
        //printArray(leftArray);
        //System.out.println("RightArray: ");
        //printArray(rightArray);
        
        int leftArrayLength = leftArray.length;
        int rightArrayLength = rightArray.length;
        
        int i= 0, j = 0, k = l;
        
        while (i < leftArrayLength && j < rightArrayLength) {
            //System.out.println("i="+i+", j="+j+", k="+k);
            if (leftArray[i] <= rightArray[j]) {
                nums[k++] = leftArray[i++];
            } else {
                nums[k++] = rightArray[j++];
                
                // In this case, add count
                //System.out.println("Put "+rightArray[j-1]+" into the "+k+" element. Add count:"+(leftArrayLength - i));
                count += leftArrayLength - i;
            }
        }
        
        while (i < leftArrayLength) {
            nums[k++] = leftArray[i++];
            
        }
        
        return count;
    }
    
    public long mergeSort(long count, int[] nums, int l, int r) {
        
        if (l < r) {
            int m = (r - l) / 2 + l;
            
            count = mergeSort(count, nums, l, m);
            count = mergeSort(count, nums, m+1, r);
            
            count = merge(count,nums, l, m, r);
        }
        
        return count;
    }
    
}

```

