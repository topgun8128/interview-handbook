# Iterators

## [https://netjs.blogspot.com/2015/05/fail-fast-vs-fail-safe-iterator-in-java.html?m=1](https://netjs.blogspot.com/2015/05/fail-fast-vs-fail-safe-iterator-in-java.html?m=1)

### Fail-Fast Vs Fail-Safe Iterator in Java

Difference between fail-fast and fail-safe iterator in Java, apart from being an important [Java Collections interview questions](https://netjs.blogspot.com/2015/11/java-collections-interview-questions.html), is a very important concept to know. The collections which are there from Java 1.2 \(or even legacy\) like [ArrayList](https://netjs.blogspot.com/2015/09/arraylist-in-java.html), [Vector](http://netjs.blogspot.com/2015/12/difference-between-arraylist-and-vector-java.html), [HashSet](https://netjs.blogspot.com/2015/09/how-hashset-works-internally-in-java.html) all have **fail-fast iterator** whereas Concurrent collections added in Java 1.5 like [ConcurrrentHashMap](https://netjs.blogspot.com/2016/01/concurrenthashmap-in-java.html), [CopyOnWriteArrayList](https://netjs.blogspot.com/2016/01/copyonwritearraylist-in-java.html), [CopyOnWriteArraySet](https://netjs.blogspot.com/2016/03/copyonwritearrayset-in-java-concurrency.html) have **fail-safe iterator**.

So first let us see the key differences between the fail-fast and fail-safe iterator in Java and then we'll see some Java programs to explain those features.

**Differences between fail-fast and fail-safe iterator in Java**

1. **fail-fast** iterator throws **ConcurrentModificationException** if the underlying collection is structurally modified in any way except through the iterator's own remove or add methods. **fail-safe** iterator doesn't throw ConcurrentModificationException.
2. **fail-fast** iterator in Java works on the original collection. **fail-safe** iterator in Java makes a copy of the underlying structure and iteration is done over that snapshot. Drawback of using a copy of the collection rather than original collection is that the **iterator will not reflect additions, removals, or changes to the collection since the iterator was created**.
3. **fail-fast** iterator provides remove, set, and add operations. Note that not all the iterators support all these methods. As exp. [ListIterator](http://netjs.blogspot.com/2015/08/list-iterator-in-java.html) supports **add\(\)** method but the general iterator doesn't. With **fail-safe** iterator element-changing operations on iterators themselves \(remove, set, and add\) are not supported. These methods throw **UnsupportedOperationException**.

Now let us see some detailed explanation and supporting programs to see these features of both fail-fast and fail-safe iterators.

**fail-fast iterator in Java**

An iterator is considered **fail-fast** if it throws a **ConcurrentModificationException** under either of the following two conditions:

* In **multi-threaded environment**, if one thread is trying to modify a Collection while another thread is iterating over it.
* **Even** **with single thread**, if a thread modifies a collection directly while it is iterating over the collection with a fail-fast iterator, the iterator will throw this exception.

**fail-fast iterator** fails if the underlying collection is **structurally modified** at any time after the iterator is created. The iterator will **throw a ConcurrentModificationException** if the underlying collection is structurally modified in any way **except through the iterator's own remove or add** \(if applicable as in ListIterator\) methods.

**What will be termed as structural modification**

Note that structural modification is any operation that adds or deletes one or more elements; merely setting the value of an element \(in case of list\) or changing the value associated with an existing key \(in case of map\) is not a structural modification.

Mostly iterators from **java.util** [package](http://netjs.blogspot.com/2016/07/package-in-java.html) throw **ConcurrentModificationException** if collection was modified by collection's methods \(add / remove\) while iterating

**Example Java code of fail-fast iterator with an attempt to add new value to a map while the map is being iterated**

```text
public class FailFastModification {
    public static void main(String[] args) {
        // creating map
        Map <String,String> cityMap = new HashMap<String,String>();
        cityMap.put("1","New York City" );
        cityMap.put("2", "New Delhi");
        cityMap.put("3", "Newark");
        cityMap.put("4", "Newport");
        // getting iterator
        Iterator <String> itr = cityMap.keySet().iterator();
        while (itr.hasNext()){
            System.out.println(cityMap.get(itr.next()));
            // trying to add new value to a map while iterating it
            cityMap.put("5", "New City");
        }        
    }

}
```

**Output**

```text
New York City
Exception in thread "main" java.util.ConcurrentModificationException
 at java.util.HashMap$HashIterator.nextNode(Unknown Source)
 at java.util.HashMap$KeyIterator.next(Unknown Source)
 at org.netjs.examples.FailFastModification.main(FailFastModification.java:20)
```

Though **we can update the underlying collection**, let's see **an example** with the same hash map used above -

```text
public class FailFastModification {
    public static void main(String[] args) {
        // creating map
        Map <String,String> cityMap = new HashMap<String,String>();
        cityMap.put("1","New York City" );
        cityMap.put("2", "New Delhi");
        cityMap.put("3", "Newark");
        cityMap.put("4", "Newport");
        // getting iterator
        Iterator <String> itr = cityMap.keySet().iterator();
        while (itr.hasNext()){
            System.out.println(cityMap.get(itr.next()));
            // updating existing value while iterating
            cityMap.put("3", "New City");
        }        
    }
}
```

Here I have changed the value for the key "3", which is reflected in the output and no exception is thrown.

**Output**

```text
New York City
New Delhi
New City
Newport
```

Using **iterator remove method** I can remove the values, that is permitted.

```text
public class FailFastModification {
    public static void main(String[] args) {
        Map <String,String> cityMap = new HashMap<String,String>();
        cityMap.put("1","New York City" );
        cityMap.put("2", "New Delhi");
        cityMap.put("3", "Newark");
        cityMap.put("4", "Newport");
        System.out.println("size before iteration " + cityMap.size());
        Iterator <String> itr = cityMap.keySet().iterator();
        while (itr.hasNext()){
            System.out.println(cityMap.get(itr.next()));
            // removing value using iterator remove method
            itr.remove();
        }
        System.out.println("size after iteration " + cityMap.size());        
    }
}
```

**Output**

```text
size before iteration 4
New York City
New Delhi
Newark
Newport
size after iteration 0
```

Here after iteration the value is removed using the remove method of the iteartor, thus the size becomes zero after the iteration is done.

**Example Java code with multiple threads showing fail-fast iterator**

Let’s see a multi-threaded example, where [concurrency](http://netjs.blogspot.com/2016/05/java-concurrency-interview-questions.html) is simulated using [sleep](http://netjs.blogspot.com/2015/07/difference-between-sleep-and-wait-java-threading.html) method. 

In this example there are two threads one [thread](http://netjs.blogspot.com/2015/06/can-we-start-same-thread-twice-in-java.html) will [iterate the map](http://netjs.blogspot.com/2015/05/how-to-loop-iterate-hash-map-in-java.html) and print the values where as the second thread will try to remove the element from the same map.

```text
public class FailFastTest {
 public static void main(String[] args) { 
  final Map<String,String> cityMap = new HashMap<String,String>();
  cityMap.put("1","New York City" );
  cityMap.put("2", "New Delhi");
  cityMap.put("3", "Newark");
  cityMap.put("4", "Newport");
  //This thread will print the map values
  // Thread1 starts 
  Thread thread1 = new Thread(){ 
   public void run(){ 
    try{ 
     Iterator i = cityMap.keySet().iterator(); 
     while (i.hasNext()){ 
      System.out.println(i.next()); 
      // Using sleep to simulate concurrency
      Thread.sleep(1000); 
     }  
    }catch(ConcurrentModificationException e){ 
     System.out.println("thread1 : Concurrent modification detected on this map"); 
    }catch(InterruptedException e){
     
    } 
   } 
  }; 
  thread1.start(); 
  // Thread1 ends
   // This thread will try to remove value from the collection,
  // while the collection is iterated by another thread.
  // thread2 starts
  Thread thread2 = new Thread(){ 
   public void run(){ 
     try{ 
    // Using sleep to simulate concurrency
      Thread.sleep(2000);
      // removing value from the map
      cityMap.remove("2"); 
      System.out.println("city with key 2 removed successfully"); 
     }catch(ConcurrentModificationException e){ 
      System.out.println("thread2 : Concurrent modification detected on this map"); 
     } catch(InterruptedException e){}
    } 
  }; 
  thread2.start(); 
// thread2 end
 } // main end
} // class end
```

**Output**

```text
1
2
city with key 2 removed successfully
thread1 : Concurrent modification detected on this map
```

It can be seen that in thread 1 which is iterating the map, Concurrent modification exception is thrown.

If we use the same multi-threaded program for updating the values, it should update the values.

```text
public class FailFastTest {
 public static void main(String[] args) { 
  final Map<String,String> cityMap = new HashMap<String,String>();
  cityMap.put("1","New York City" );
  cityMap.put("2", "New Delhi");
  cityMap.put("3", "Newark");
  cityMap.put("4", "Newport");
  //This thread will print the map values
  Thread thread1 = new Thread(){ 
   public void run(){ 
    try{ 
     Iterator i = cityMap.keySet().iterator(); 
     while (i.hasNext()){ 
      System.out.println(cityMap.get(i.next())); 
      // Using sleep to simulate concurrency
      Thread.sleep(1000); 
     }  
    }catch(ConcurrentModificationException e){ 
     System.out.println("thread1 : Concurrent modification detected on this map"); 
    }catch(InterruptedException e){
     
    } 
   } 
  }; 
  thread1.start(); 
  
  Thread thread2 = new Thread(){ 
   public void run(){ 
     try{ 
    // Using sleep to simulate concurrency
      Thread.sleep(500);
      // Updating value in the map
      cityMap.put("2", "New City"); 
      System.out.println("city with key 2 updated successfully"); 
     }catch(ConcurrentModificationException e){ 
      System.out.println("thread2 : Concurrent modification detected on this map"); 
     } catch(InterruptedException e){}
    } 
  }; 
  thread2.start(); 
 } // main end
} // class end
```

**Output**

```text
New York City
city with key 2 updated successfully
New City
Newark
Newport
```

It can be seen while one thread is iterating through the map another thread is updating one of the value but that doesn't result in concurrentmodificationException.

**Fail Safe iterator in Java**

In case of **fail-safe iterator, ConcurrentModificationException** is not thrown as the fail-safe iterator makes a copy of the underlying structure and iteration is done over that snapshot.   
Since iteration is **done over a copy of the collection so interference is impossible** and the iterator is guaranteed not to throw ConcurrentModificationException. 

Drawback of using a copy of the collection rather than original collection is that the iterator will not reflect additions, removals, or changes to the collection since the iterator was created. Element-changing operations on iterators themselves \(remove, set, and add\) are not supported. These methods throw **UnsupportedOperationException**.

Iterator of [CopyOnWriteArrayList](http://netjs.blogspot.com/2016/01/copyonwritearraylist-in-java.html) is an example of fail-safe Iterator in Java, also iterator provided by [ConcurrentHashMap](http://netjs.blogspot.com/2016/01/concurrenthashmap-in-java.html)keySet is **fail-safe** and never throws ConcurrentModificationException.

**Example Java code with fail-safe iterator**

```text
import java.util.Iterator;
import java.util.List;
import java.util.concurrent.CopyOnWriteArrayList;

public class FailSafeTest {
 public static void main(String[] args) {

  List  cityList = new CopyOnWriteArrayList();
  cityList.add("New York City");
  cityList.add("New Delhi");
  cityList.add("Newark");
  cityList.add("Newport");  
  Iterator itr = cityList.iterator();
  boolean flag = false;
  while (itr.hasNext()){         
     System.out.println(itr.next());
     // add a new value into the list
     if(!flag){
           cityList.add("NewCity");
           flag = true;
     }
      
     //itr.remove();
   }
   System.out.println("After addition -- ");
   itr = cityList.iterator();
   while (itr.hasNext()){         
      System.out.println(itr.next());
   }
 }
}

```

**Output**

```text
New York City
New Delhi
Newark
Newport
After addition -- 
New York City
New Delhi
Newark
Newport
NewCity
```

This program **won't throw ConcurrentModificationException** as iterator used with CopyOnWriteArrayList is fail-safe iterator. 

If we uncomment the line **//itr.remove\(\)**; this program will throw UnsupportedOperationException as **fail-safe iterator does not support remove, set, and add operations**.

**Points to note -**

* An iterator is considered fail-fast if it throws a ConcurrentModificationException in case the underlying collection's structure is modified.
* While iterating a list or a map values can be updated, only if an attempt is made to add or remove from the collection ConcurrentModificationException will be thrown by fail-fast iterator.
* Fail-fast iterators throw ConcurrentModificationException on a best-effort basis and fail-fast behavior of an iterator cannot be guaranteed.
* Fail-safe iterator works with a copy of the collection rather than the original collection thus interference is impossible and the iterator is guaranteed not to throw ConcurrentModificationException.
* remove, set, and add operations are not supported in fail-safe iterator.

That's all for this topic **Fail-Fast Vs Fail-Safe Iterator in Java**. If you have any doubt or any suggestions to make please drop a comment. Thanks!

