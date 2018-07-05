# ArrayList vs LinkedList

## [https://netjs.blogspot.com/2015/08/difference-between-arraylist-and-linkedlist-in-java.html?m=1](https://netjs.blogspot.com/2015/08/difference-between-arraylist-and-linkedlist-in-java.html?m=1)

### Difference Between ArrayList And LinkedList in Java

**LinkedList** is implemented using a **doubly linked list** concept where as **ArrayList** internally uses an [array](http://netjs.blogspot.com/2017/02/array-in-java.html) of Objects which can be **resized dynamically.** 

**ArrayList** class provides a [constructor](http://netjs.blogspot.com/2015/04/constructor-chaining-in-java-calling-one-constructor-from-another.html) **ArrayList\(int initialCapacity\)** even if the default constructor **ArrayList\(\)** is used an empty list is constructed with an initial capacity of ten. Where as LinkedList just provides one constructor **LinkedList\(\)**which constructs an empty list. Note that in LinkedList there is **no** provision of **initial capacity** or default capacity. 

What it means is that if elements are added at the last every time and **capacity is not breached then ArrayList will work faster** because it already has an initial capacity and just need to add that element at the index in the underlying array. Where as in **LinkedList** a new node has to be created and references for Next and Prev are to be adjusted to accommodate the new Node.

* Refer post [How ArrayList works internally in Java](http://netjs.blogspot.com/2015/08/how-arraylist-works-internally-in-java.html) to know more about internal working of ArrayList.
* Refer post [How Linked List class works internally in Java](http://netjs.blogspot.com/2015/08/how-linked-list-class-works-internally-java.html) to know more about internal working of LinkedList.

While using ArrayList we generally use add\(\) method, very rarely we use add method where index is also passed to add element at any specific position in the ArrayList and to fetch elements from List we use **get\(int index\)** which also is faster in ArrayList. That's why we normally use ArrayList, even JavaDocs advocate the same thing- 

**Refer**: [https://docs.oracle.com/javase/tutorial/collections/implementations/list.html](https://docs.oracle.com/javase/tutorial/collections/implementations/list.html)

Let's go through some of the operations to see which implementation of list shines where -

* **Adding an element** - If you are frequently adding element at the **beginning of the list** then LinkedList may be a better choice because in case of ArrayList adding at the beginning every time will result in shuffling all the existing elements within the underlying array by one index to create space for the new element. In the case of adding at the last ArrayList may give **O\(n\)** performance in the worst case. That will happen if you add more elements than the capacity of the underlying array, as in that case a new array \(**1.5** times the last size\) is created, and the old array is copied to the new one. So for LinkedList **add\(e Element\)** is always **O\(1\)** where as for ArrayList **add\(e Element\)** operation runs in amortized constant time, that is, adding n elements requires **O\(n\)** time. So the amortized time \(i.e. time per insertion\) is O\(1\).
* **Retrieving an element** - Retrieving an element from the list using get\(int index\) is **faster in ArrayList than in LinkedList**. Since ArrayList internally uses an array to store elements so **get\(int index\)** means going to that index directly in the array. In Linked list **get\(int index\)** will mean traversing through the linked list nodes.

  So, for **ArrayList** get\(int index\) is **O\(1\)** where as for **LinkedList** get\(int index\) is **O\(n\)**.

* **Removing an element** - If you are [removing element from a list](http://netjs.blogspot.com/2015/08/how-to-remove-elements-from-arraylist-java.html) using the **remove\(int index\)** method then for LinkedList class it will be **O\(n\)** as list has to be traversed to get to that index and then the element removed. Though the LinkedList provides methods like **removeFirst\(\)** and **removeLast\(\)** to remove the first or last element and in that case it will be O\(1\).  In case of ArrayList getting to that index is fast but removing will mean shuffling the remaining elements to fill the gap created by the removed element with in the underlying array. It ranges from **O\(1\)** for removing the last element to **O\(n\)**. Thus it can be said remove\(int index\) operation is **O\(n - index\)** for the ArrayList.
* **Removing an element while iterating** - if you are [iterating the list](http://netjs.blogspot.com/2015/08/how-to-loop-iterate-arraylist-in-java.html) and removing element using **iterator.remove\(\)** then for LinkedList it is **O\(1\)** as the iterator is already at the element that has to be removed so removing it means just adjusting the Next and Prev references. Where as for ArrayList it is again **O\(n - index\)**because of an overhead of shuffling the remaining elements to fill the gap created by the removed elements.
* One more difference is that **LinkedList** implementation provides a **descendingIterator\(\)** which comes from implementing the **Deque interface**. **descendingIterator\(\)** returns an iterator over the elements in this deque in reverse sequence.  In **ArrayList** there is no such iterator.

