---
description: 'source:hackerearth'
---

# Linear Search

**Linear search** is used on a collections of items. It relies on the technique of traversing a list from start to end by exploring properties of all the elements that are found on the way.

For example, consider an array of integers of size N. You should find and print the position of all the elements with value x. Here, the linear search is based on the idea of matching each element from the beginning of the list to the end of the list with the integer x, and then printing the position of the element if the condition is \`True'.

**Implementation:**

The pseudo code for this example is as follows :

```text
for(start to end of array)
{
    if (current_element equals to 5)  
    {
        print (current_index);
    }
}
```

**For example, consider the following image:**

![enter image description here](https://he-s3.s3.amazonaws.com/media/uploads/f0fdd3b.png)

If you want to determine the positions of the occurrence of the number 7 in this array. To determine the positions, every element in the array from start to end, i.e., from index 1 to index 10 will be compared with number 7, to check which element matches the number 7.

**Time Complexity:**

The time complexity of the linear search is O\(N\) because each element in an array is compared only once.

