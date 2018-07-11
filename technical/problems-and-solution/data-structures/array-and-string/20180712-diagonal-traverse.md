---
description: 'source:leetcode level:medium'
---

# 20180712 Diagonal Traverse

## Problem



Given a matrix of M x N elements \(M rows, N columns\), return all elements of the matrix in diagonal order as shown in the below image.

**Example:**  


```text
Input:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
Output:  [1,2,4,7,5,3,6,8,9]
Explanation:

```

**Note:**  


1. The total number of elements of the given matrix will not exceed 10,000.

## Solution



### Java

#### Solution 1 - With boolean

```java
class Solution {
    public int[] findDiagonalOrder(int[][] matrix) {
        if (matrix == null || matrix.length == 0) {
            return new int[0];
        }
        
        int rows = matrix.length;
        int cols = matrix[0].length;
        int[] nums = new int[rows*cols];
        
        int i=0, j=0, k=0;
        boolean upright = true;
        
        while (k < nums.length) {
            
            if (i < rows && j < cols) {
                nums[k++] = matrix[i][j];
            }
            
            if (upright) {
                if (j==cols-1) {
                    if (i<rows-1) {
                        i++;
                    }
                    // turn in next round
                    upright = false;
                } else {
                    j++;
                    if (i==0) {
                        // turn in next round
                        upright = false;
                    } else {
                        i--;
                    }
                }
            } else {
                if (i==rows-1) {
                    if (j<cols-1) {
                        j++;
                    }
                    // turn in next round
                    upright = true;
                } else {
                    i++;
                    if (j==0) {
                        // turn in next round
                        upright = true;
                    } else {
                        j--;
                    }
                }
            }
        }
        
        return nums;
        
    }
}
```

#### Solution 2 - Without boolean

```java
class Solution {
    public int[] findDiagonalOrder(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return new int[0];
        }
        
        int rows = matrix.length;
        int cols = matrix[0].length;
        int[] nums = new int[rows*cols];
        
        int i=0, j=0, k=0;
        
        while (k < nums.length) {
            
            nums[k++] = matrix[i][j];
            
            if ((i+j) % 2 == 0) {
                if (j==cols-1) {
                    if (i<rows-1) {
                        i++;
                    }
                } else {
                    j++;
                    if (i>0) {
                        i--;
                    }
                }
            } else {
                if (i==rows-1) {
                    if (j<cols-1) {
                        j++;
                    }
                } else {
                    i++;
                    if (j>0) {
                        j--;
                    }
                }
            }
        }
        
        return nums;
        
    }
}
```

