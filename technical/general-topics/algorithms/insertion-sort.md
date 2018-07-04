---
description: 'source:hackerearth'
---

# Insertion Sort

Insertion sort is based on the idea that one element from the input elements is consumed in each iteration to find its correct position i.e, the position to which it belongs in a sorted array.

It iterates the input elements by growing the sorted array at each iteration. It compares the current element with the largest value in the sorted array. If the current element is greater, then it leaves the element in its place and moves on to the next element else it finds its correct position in the sorted array and moves it to that position. This is done by shifting all the elements, which are larger than the current element, in the sorted array to one position ahead

**Implementation**

```text
void insertion_sort ( int A[ ] , int n) 
{
     for( int i = 0 ;i < n ; i++ ) {
    /*storing current element whose left side is checked for its 
             correct position .*/

      int temp = A[ i ];    
      int j = i;

       /* check whether the adjacent element in left side is greater or
            less than the current element. */

          while(  j > 0  && temp < A[ j -1]) {

           // moving the left side element to one position forward.
                A[ j ] = A[ j-1];   
                j= j - 1;

           }
         // moving current element to its  correct position.
           A[ j ] = temp;       
     }  
}
```

Take array A\[\]=\[7,4,5,2\].

![enter image description here](https://he-s3.s3.amazonaws.com/media/uploads/46bfac9.png)

Since 7 is the first element has no other element to be compared with, it remains at its position. Now when on moving towards 4, 7 is the largest element in the sorted list and greater than 4. So, move 4 to its correct position i.e. before 7. Similarly with 5, as 7 \(largest element in the sorted list\) is greater than 5, we will move 5 to its correct position. Finally for 2, all the elements on the left side of 2 \(sorted list\) are moved one position forward as all are greater than 2 and then 2 is placed in the first position. Finally, the given array will result in a sorted array.

**Time Complexity:**

In worst case,each element is compared with all the other elements in the sorted array. For N elements, there will be N2 comparisons. Therefore, the time complexity is O\(N2\)  


#### VISUALIZER

[https://www.hackerearth.com/practice/algorithms/sorting/insertion-sort/visualize/](https://www.hackerearth.com/practice/algorithms/sorting/insertion-sort/visualize/)

