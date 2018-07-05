# HashMap

## [https://netjs.blogspot.com/2015/05/how-hashmap-internally-works-in-java.html?m=1](https://netjs.blogspot.com/2015/05/how-hashmap-internally-works-in-java.html?m=1)

### How HashMap Internally Works in Java

This post talks about HashMap internal implementation in Java, which is also a favourite [Java Collections interview question](https://netjs.blogspot.com/2015/11/java-collections-interview-questions.html).

There are four things you should know about HashMap before going into internal working of HashMap in Java-

* **HashMap** works on the principal of hashing.
* **Map.Entry interface** - This interface gives a map entry \(key-value pair\). HashMap in Java stores both key and value object, in bucket, as an object of Entry class which implements this nested interface Map.Entry.
* **hashCode\(\)** - HashMap provides put\(key, value\) for **storing** and get\(key\) method for **retrieving** values from HashMap. When **put\(\)** method is used to store \(Key, Value\) pair, HashMap implementation **calls hashcode** on Key object to calculate a hash that is used to find a bucket where Entry object will be stored. When **get\(\)** method is used to retrieve value, again key object \(passed with the get\(\) method\) is used to calculate a hash which is then used to find a bucket where that particular key is stored.
* **equals\(\)** - equals\(\) method is used to **compare objects for equality**. In case of HashMap key object is used for comparison, also using equals\(\) method Map knows how to handle **hashing collision** \(hashing collision means more than one key having the same hash value, thus assigned to the same bucket\). In that case objects are stored in a linked list, refer [figure](https://netjs.blogspot.com/2015/05/how-hashmap-internally-works-in-java.html?m=1#hashmap) for more clarity.  Where **hashCode\(\)** method helps in finding the bucket where that key is stored, **equals\(\)** method helps in finding the right key as there may be more than one key-value pair stored in a single bucket.

**\*\*** Bucket term used here is actually an index of array, that array is called **table** in HashMap implementation. Thus table\[0\] is referred as bucket0, table\[1\] as bucket1 and so on.

* Refer [Overriding hashCode\(\) and equals\(\) method in Java](http://netjs.blogspot.com/2015/06/overriding-hashcode-and-equals-method.html) to know more about hashCode\(\) and equals\(\) method

How important it is to have a proper **hash code and equals method** can be seen through the help of the following program - 

```text
public class HashMapTest {
    public static void main(String[] args) {
        Map <Key, String> cityMap = new HashMap<Key, String>();
        cityMap.put(new Key(1, "NY"),"New York City" );
        cityMap.put(new Key(2, "ND"), "New Delhi");
        cityMap.put(new Key(3, "NW"), "Newark");
        cityMap.put(new Key(4, "NP"), "Newport");

        System.out.println("size before iteration " + cityMap.size());
        Iterator <Key> itr = cityMap.keySet().iterator();
        while (itr.hasNext()){
            System.out.println(cityMap.get(itr.next()));     
        }
        System.out.println("size after iteration " + cityMap.size());    
    }
 
}

// This class' object is used as key
// in the HashMap
class Key{
 int index;
 String Name;
 Key(int index, String Name){
  this.index = index;
  this.Name = Name;
 }
 
 @Override
 // A very bad implementation of hashcode
 // done here for illustrative purpose only 
 public int hashCode(){
  return 5;
 }
 
 @Override
 // A very bad implementation of equals
 // done here for illustrative purpose only 
 public boolean equals(Object obj){
  return true;
 }
 
}

```

**Output** 

```text
size before iteration 1
Newport
size after iteration 1
```

**Understanding the Code** 

Lets get through the code to see what is happening, this will also help in understanding how put\(\) method of HashMap works internally.

Notice that I am inserting 4 values in the [HashMap](http://netjs.blogspot.com/2015/05/how-to-loop-iterate-hash-map-in-java.html), still in the output it says size is 1 and iterating the map gives me the last inserted entry. **Why is that**?

Answer lies in, how hashCode\(\) and equals\(\) method are implemented for the key Class. Have a look at the hashCode\(\) method of the class Key which **always returns "5"** and the equals\(\) method which is **always returning "true"**.

When a value is put into HashMap **it calculates a hash using key object** and for that it **uses the hashCode\(\)** method of the key object class \(or its parent class\). Based on the calculated hash value HashMap implementation decides which bucket should store the particular Entry object.

In my code the **hashCode\(\)** method of the key class always returns "5". This effectively means, calculated hash value, is same for all the entries inserted in the HashMap. Thus all the entries are stored in the same bucket.

**Second thing**, a HashMap implementation does is to **use equals\(\) method** to see if the key is equal to any of the already inserted keys \(**Recall that there may be more than one entry in the same bucket**\). Note that, with in a bucket key-value pair entries \(Entry objects\) are stored in a linked-list \(Refer figure for more clarity\). In case hash is same, but equals\(\) returns false \(which essentially means more than one key having the same hash or hash collision\) Entry objects are stored, with in the same bucket, in a linked-list.

In my code, I am always **returning true for equals\(\) method** so the HashMap implementation "thinks" that the keys are equal and overwrites the value. So, in a way using hashCode\(\) and equals\(\) I have "tricked" HashMap implementation to think that all the keys \(even though different\) are same, thus overwriting the values.

In a nutshell there are three scenarios in case of HashMap put\(\) method- 

* Using **hashCode\(\)** method, hash value will be calculated. In which bucket particular entry will be stored is ascertained using that hash.
* **equals\(\)** method is used to find if such a key already exists in that bucket, if not found then a new node is created with the map entry and stored within the same bucket. A linked-list is used to store those nodes.
* If **equals\(\)** method returns true, it means that the key already exists in the bucket. In that case, the new value will overwrite the old value for the matched key.

Following image shows how Entry \(key-value pair\) objects are stored internally in table array of the HashMap class.[![HashMap internal implementation in Java](https://4.bp.blogspot.com/-x_U5Yjgsg8c/VVNgwvz7WNI/AAAAAAAAAHg/BreaAUMvJpc/s1600/hashMap%2Binternal.png)](https://4.bp.blogspot.com/-x_U5Yjgsg8c/VVNgwvz7WNI/AAAAAAAAAHg/BreaAUMvJpc/s1600/hashMap%2Binternal.png)

**How HashMap get\(\) method works internally**

As we already know how Entry objects are stored in a bucket and what happens in the case of Hash Collision it is easy to understand what happens when key object is passed in the **get\(\)** method of the HashMap to retrieve a value. 

Using the key \(passed in the get\(\) method\) again hash value will be calculated to determine the bucket where that Entry object is stored, in case there are more than one Entry object with in the same bucket \(stored as a linked-list\) **equals\(\)**method will be used to find out the correct key. As soon as the matching key is found get\(\) method will return the value object stored in the Entry object.

**In case of null Key**

**HashMap in Java also allows null as key**, though there can only be one null key in HashMap. While storing the Entry object HashMap implementation checks if the key is null, in case key is null, it always map to bucket 0, as **hash is not calculated for null keys.** 

### **HashMap changes in Java 8**

Though HashMap implementation in Java provides constant time performance **O\(1\)** for get\(\) and put\(\) methods but that is in the **ideal case** when the Hash function distributes the objects evenly among the buckets. 

But the performance may worsen in the case **hashCode\(\)** used is not proper and there are lots of hash collisions. As we know now that in case of hash collision entry objects are stored as a node in a linked-list and **equals\(\)** method is used to compare keys. That comparison to find the correct key with in a linked-list is a linear operation so in a worst case scenario the complexity becomes O\(n\).

To address this issue in Java 8 hash elements **use balanced trees instead of linked lists after a certain threshold is reached**. Which means HashMap starts with storing Entry objects in linked list but after the number of items in a hash becomes larger than a certain threshold, the hash will change from using a linked list to a balanced tree, this will improve the worst case performance **from O\(n\) to O\(log n\).**

### **Points to note**

* HashMap works on the principal of hashing.
* HashMap uses the hashCode\(\) method to calculate a hash value. Hash value is calculated using the key object. This hash value is used to find the correct bucket where Entry object will be stored.
* HashMap uses the equals\(\) method to find the correct key whose value is to be retrieved in case of get\(\) and to find if that key already exists or not in case of put\(\).
* **Hashing collision means more than one key having the same hash value**, in that case Entry objects are stored as a linked-list with in a same bucket.
* With in a bucket values are stored as Entry objects which contain both key and value.
* In Java 8 hash elements use balanced trees instead of linked lists after a certain threshold is reached while storing values. This improves the worst case performance from O\(n\) to O\(log n\). 

That's all for this topic **How HashMap Internally Works in Java**. If you have any doubt or any suggestions to make please drop a comment. Thanks!



## [https://javarevisited.blogspot.com/2011/02/how-hashmap-works-in-java.html?m=1](https://javarevisited.blogspot.com/2011/02/how-hashmap-works-in-java.html?m=1)

### **1\) Why String, Integer and other wrapper classes are considered good keys?**

String, Integer and other wrapper classes are natural candidates of HashMap key, and String is most frequently used key as well because [String is immutable and final](http://javarevisited.blogspot.sg/2010/10/why-string-is-immutable-in-java.html), and overrides equals and hashcode\(\) method. Other wrapper class also shares similar property. Immutability is required, in order to prevent changes on fields used to calculate hashCode\(\) because if key object returns different hashCode during insertion and retrieval than it won't be possible to get an object from HashMap.   
  
Immutability is best as it offers other advantages as well like [thread-safety](http://javarevisited.blogspot.sg/2012/01/how-to-write-thread-safe-code-in-java.html), If you can  keep your hashCode same by only making certain fields final, then you go for that as well. Since equals\(\) and hashCode\(\) method is used during retrieval of value object from HashMap, it's important that key object correctly override these methods and follow contact. If unequal object returns different hashcode than chances of collision will be less which subsequently improve the performance of HashMap.

### **2\) Can we use any custom object as a key in HashMap?**

This is an extension of previous questions. Of course you can use any Object as key in Java HashMap provided it follows equals and hashCode contract and its hashCode should not vary once the object is inserted into [Map](http://javarevisited.blogspot.sg/2011/12/how-to-traverse-or-loop-hashmap-in-java.html). If the custom object is Immutable than this will be already taken care because you can not change it once created. 

### **3\) Can we use ConcurrentHashMap in place of Hashtable?**

This is another question which getting popular due to increasing popularity of ConcurrentHashMap. Since we know Hashtable is synchronized but ConcurrentHashMap provides better concurrency by only locking portion of map determined by concurrency level. ConcurrentHashMap is certainly introduced as Hashtable and can be used in place of it, but Hashtable provides stronger thread-safety than ConcurrentHashMap. See my post [difference between Hashtable and ConcurrentHashMap](http://javarevisited.blogspot.sg/2011/04/difference-between-concurrenthashmap.html) for more details.

## [https://javarevisited.blogspot.com/2011/04/difference-between-concurrenthashmap.html](https://javarevisited.blogspot.com/2011/04/difference-between-concurrenthashmap.html)

### **ConcurrentHashMap vs Hashtable vs Synchronized Map**

Though all three collection classes are thread-safe and can be used in multi-threaded, concurrent Java application, there is a significant difference between them, which arise from the fact that how they achieve their thread-safety. **Hashtable** is a **legacy** class from JDK 1.1 itself, which **uses synchronized methods to achieve thread-safety.** All methods of Hashtable are synchronized which makes them quite slow due to contention if a number of thread increases. **Synchronized Map** is also not very different than Hashtable and provides **similar** performance in concurrent Java programs. The only difference between Hashtable and Synchronized Map is that later is **not a legacy** and you can **wrap any Map to create it's synchronized version by using Collections.synchronizedMap\(\) method**.  
  
  
On the other hand, **ConcurrentHashMap** is specially designed for concurrent use i.e. more than one thread. By default it simultaneously allows 16 threads to read and write from Map without any external synchronization. It is also very scalable because of stripped locking technique used in the [internal implementation of ConcurrentHashMap](http://javarevisited.blogspot.sg/2013/02/concurrenthashmap-in-java-example-tutorial-working.html) class. Unlike Hashtable and Synchronized Map, it never locks whole Map, instead, it divides the map into segments and locking is done on those. Though it performs better if a number of reader threads are greater than the number of writer threads.

## [https://javarevisited.blogspot.com/2013/02/concurrenthashmap-in-java-example-tutorial-working.html](https://javarevisited.blogspot.com/2013/02/concurrenthashmap-in-java-example-tutorial-working.html)

### Summary

1. ConcurrentHashMap allows concurrent read and thread-safe update operation.
2. During the update operation, ConcurrentHashMap only locks a portion of Map instead of whole Map.
3. The concurrent update is achieved by internally dividing Map into the small portion which is defined by concurrency level.
4. Choose concurrency level carefully as a significantly higher number can be a waste of time and space and the lower number may introduce thread contention in case writers over number concurrency level.
5. All operations of ConcurrentHashMap are [thread-safe](http://javarevisited.blogspot.com/2012/12/how-to-create-thread-safe-singleton-in-java-example.html).
6. Since ConcurrentHashMap implementation doesn't lock whole Map, there is chance of read overlapping with update operations like put\(\) and remove\(\). In that case result returned by get\(\) method will reflect most recently completed operation from there start.
7. Iterator returned by ConcurrentHashMap is weekly consistent, [fail-safe](http://javarevisited.blogspot.com/2012/02/fail-safe-vs-fail-fast-iterator-in-java.html) and never throw ConcurrentModificationException. In Java.
8. ConcurrentHashMap doesn't allow null as key or value.
9. You can use ConcurrentHashMap in place of [Hashtable](http://javarevisited.blogspot.com/2010/10/difference-between-hashmap-and.html) but with caution as CHM doesn't lock whole Map.
10. During putAll\(\) and clear\(\) operations, the concurrent read may only reflect insertion or deletion of some entries. 

## [http://javabypatel.blogspot.com/2016/09/concurrenthashmap-interview-questions.html](http://javabypatel.blogspot.com/2016/09/concurrenthashmap-interview-questions.html)

### Question 1.  What is the need of ConcurrentHashMap when there is HashMap and Hashtable already present?

**Performance and Thread safety are 2 parameter on which ConcurrentHashMap is focused.Imagine a scenario where we have frequent reads\(get\) and less writes\(put\) and need thread safety,**  
  
**Can we use Hashtable in this scenario?**  
No. Hashtable ****is thread safe but give poor performance in case of multiple thread reading from hashtable because all methods of Hashtable including get\(\) method is synchronized and due to which invokation to any method has to wait until any other thread working on hashtable complete its operation\(get, put etc\).  
  
**Can we use HashMap in this scenario?**  
No. Hashmap will solve performance issue by giving parallel access to multiple threads reading hashmap simultaneously.  
But Hashmap is not thread safe, so what will happen if one thread tries to put data and requires Rehashing and at same time other thread tries to read data from hashmap, It will go in infinite loop.  
**Infinite loop problem discussed in detail:** [**Infinite loop in HashMap**](http://javabypatel.blogspot.in/2016/01/infinite-loop-in-hashmap.html)  
 ****  
**ConcurrentHashMap combines good features of hashmap and hashtable and solves performance and thread safety problem nicely.**  


### Question 2.  HashMap and Hashtable uses Array and Linkedlist as datastructure to store data, How is it different in ConcurrentHashMap?

If you are not familiar with HashMap and Hashtable, Please go through it first:  
[How HashMap works](http://javabypatel.blogspot.in/2015/09/hashmap-data-structure-and-hashcode.html)   
  
Below diagram shows how hashtable/hashmap look like,  
[![](https://2.bp.blogspot.com/-9JEpgm-_nVw/V-pS4HnlajI/AAAAAAAABPE/p3t6GfbM4hsYrJvRhbs0u2Y8yrCu8-4tACLcB/s400/hashmap-internal-structure.png)](https://2.bp.blogspot.com/-9JEpgm-_nVw/V-pS4HnlajI/AAAAAAAABPE/p3t6GfbM4hsYrJvRhbs0u2Y8yrCu8-4tACLcB/s1600/hashmap-internal-structure.png)  
ConcurrentHashMap added one Array on top of it and each index of this additional array represents complete HashMap. Additional array is called Segment in ConcurrentHashMap.  
  
Architecture of ConcurrentHashMap looks like below,  
[![](https://4.bp.blogspot.com/-1W2vuBYf740/V-pnAFenccI/AAAAAAAABPU/lKSaVIfAzpMmrpRanTydHXSAA5uhC_p7wCLcB/s640/consurrenthashmap-internal-structure.png)](https://4.bp.blogspot.com/-1W2vuBYf740/V-pnAFenccI/AAAAAAAABPU/lKSaVIfAzpMmrpRanTydHXSAA5uhC_p7wCLcB/s1600/consurrenthashmap-internal-structure.png)  
  
**Putting key-value pair:**   
  
1. Putting key-value pair in ConcurrentHashMap requires first identifying exact index in   
    Segment array.   
    \(Once Segment array index is identified, Now flow will be exactly same as putting the data in   
    hashmap/hashtable.\)   
2. After identifying index in Segment array, next task is to identify index of internal bucket/array   
    present in internal hashmap as shown in figure above.   
 3. After identying bucket\(internal array index\), iterate key-value pairs and check each key with key   
    to store, wherever match is found replace stored value with value to store.  
    If there is no match, store key-value pair at the last of list.  
  
  
**Getting key-value pair:**  
  
1. Getting key-value pair in ConcurrentHashMap requires first identifying exact index in   
    Segment array.   
    \(Once Segment array index is identified, Now flow will be exactly same as getting the data from   
    hashmap/hashtable.\)   
2. After identifying index in Segment array, next task is to identify index of internal bucket/array   
    present in internal hashmap as shown in figure above.   
3. After identying bucket\(internal array index\), iterate key-value pairs and match each key with   
    given key, wherever match is found return value stored against key.  
    If there is no match, return null.  
  
  


### Question 3.  How ConcurrentHashMap is efficient in terms of Performance and Thread safety?

ConcurrentHashMap provides better Performance by replacing the Hashtable's map wide lock to Segment level lock.  
  
Hashtable is not efficient beacause it uses map wide lock, it means lock is applied on map object itself,   
  
**So if 2 threads tries to call hashtable.get\(key\),**   
Thread T1 calls to get\(\) method will acquire a lock on hashtable object and then execute get\(\) method. \(Lock is on complete 'hashtable object'\)  
  
Now if Thread T2 calls hashtable.get\(key\) method, then it will also try to acquire lock on hashtable object, but T2 will not able to acquire lock as lock on 'hashtable' is currently held by T1,   
  
So T2 waits until T1 finishes get\(\) operation and release lock on hashtable object.  
  
[![](https://3.bp.blogspot.com/-aD_Z4JJtrhc/V-rLvoLHVaI/AAAAAAAABPk/M1VpuzyvZcY56vjBlnBGaczd-HifRuwvwCEw/s640/hashtable-vs-concurrenthashmap-lock.png)](https://3.bp.blogspot.com/-aD_Z4JJtrhc/V-rLvoLHVaI/AAAAAAAABPk/M1VpuzyvZcY56vjBlnBGaczd-HifRuwvwCEw/s1600/hashtable-vs-concurrenthashmap-lock.png)  
ConcurrentHashMap works bit different here and instead of locking complete map object it Locks per Segment.   
It means instead of single map wide lock, it has multiple Segment level lock.  
  
So 2 Threads can execute put operation simultaneously by acquiring lock on different Segments.  
  
Thread T1 calls concurrentHashMap.put\(key, value\), It acquires lock on say Segment 1 and invokes put method.  
Thread T2 calls concurrentHashMap.put\(key, value\), It acquires lock on say Segment 4 and invokes put method.  
  
Both threads doesn't interfere with each other and both can proceed simultaneously as they are working on separate Segment locks.  
  
**This is how ConcurrentHashMap improves Performance and provide Thread safety as well.**   


### Question 4.  Can multiple threads read and write from same or different Segments of ConcurrentHashMap simultaneously?

**Read Operation: get\(key\)**   
**Same Segment/Different Segment : Yes.**   
Two threads T1 and T2 both can simultaneously read data from same Segment or different Segment of CHM simultaneously without blocking each other.  
  
  
**Write Operation: put\(key, value\)**   
**Different Segment :Yes**   
Multiple threads can write data to different Segment of CHM simultaneously without blocking each other.  
**Same Segment : No**     
Multiple threads CANNOT write data to same Segment of CHM simultaneously and need to wait for one thread to come write operation and then only other write operation can be proceed.   
     
  
**Read-Write Operation: get and put**  
**Say T1 is writing data in Segment 1 and T2 is reading data from same Segment 1, can read be allowed while writing operation is going on?**  
**YES.**   
Both operation that is T1 writing and T2 reading can be done parallely**.**   
  
**What data will T2 read if T1 is updating same data?**  
Retrieval operations \(including get\) generally do not block, so may overlap with update operations \(including put and remove\).   
Latest updated value present will be returned by get operation that is value updated by most recently completed update operationswill be returned.  
  
**Note: Get operations are lock free and can be performed simulateneously irrespective of other thread writing data on same or different Segment of CHM.**  


### Question 5.  What is the default size of Segment array? how it is tuned? What is ConcurrenyLevel in case of CHM?

**Default size of Segment array is 16.**   
 ****   
ConcurrentHashMap differes from Hashtable in terms of Performance by introducing Segment array.  
Each index of Segment array is guarded by a lock for put operation.  
  
[![](https://4.bp.blogspot.com/-icREX-PLyG0/V_YsC15kxrI/AAAAAAAABRc/CDLvHf-xyRkeu8U_UaZbfos55t0E5NvAgCLcB/s640/concurrency-level-concurrenthashmap.png)](https://4.bp.blogspot.com/-icREX-PLyG0/V_YsC15kxrI/AAAAAAAABRc/CDLvHf-xyRkeu8U_UaZbfos55t0E5NvAgCLcB/s1600/concurrency-level-concurrenthashmap.png)  
Threads working on separate Segments index doesn't affect each other.   
By default Segments array size is 16, So maximum 16 threads can simultaneously put data in map considering each thread is working on separate Segment array index.  


#### How Segment array size is tuned? 

Segment size decides the number of Threads that can paralley write to a map.  
Segment array size is configured using ConcurrencyLevel parameter as shown below,  
[?](http://javabypatel.blogspot.com/2016/09/concurrenthashmap-interview-questions.html#)

| 1 | `ConcurrentHashMap m =` `new` `ConcurrentHashMap(initialCapacity, loadFactor, concurrencyLevel)` |
| --- |


  
[![](https://4.bp.blogspot.com/-0HRMmJEuDys/V_YtFEKQ8BI/AAAAAAAABRk/Z195nyfoOwgwBc53mHn9pkVsFAm4ORYMQCLcB/s640/consurrenthashmap-concurrency-level.png)](https://4.bp.blogspot.com/-0HRMmJEuDys/V_YtFEKQ8BI/AAAAAAAABRk/Z195nyfoOwgwBc53mHn9pkVsFAm4ORYMQCLcB/s1600/consurrenthashmap-concurrency-level.png)  
**It takes 3 parameters,**     
  
**Example:**   
[?](http://javabypatel.blogspot.com/2016/09/concurrenthashmap-interview-questions.html#)

| 1 | `ConcurrentHashMap m =` `new` `ConcurrentHashMap(200` `,` `0.75f,` `10);` |
| --- |


**Initial capacity** is 200, it means CHM make sure it has space for adding 200 key-value pairs after creation.  
  
**Load factor** is 0.75, it means when average number of elements per map exceeds 150 \(intital capacity \* load factor = 200 \* 0.75 = 150\) at that time map size will be increased and existing items in map are rehashed to put in new larger size map.  
For more details on Load Factor: [**Load factor in Map**](http://javabypatel.blogspot.in/2015/10/what-is-load-factor-and-rehashing-in-hashmap.html)  
  
**Concurrency level** is 10, it means at any given point of time Segment array size will be 10 or greater than 10, so that 10 threads can able to parallely write to a map.  


### Question 6.  What will happen if the size of Segment array is too small or too large?

Choosing correct **ConcurrencyLevel** is very important because ConcurrencyLevel decides what will be the size of Segment array.  
  
Segment array size will decide how many parallel Threads will be able to execute put operation on map parallely.  
[![](https://2.bp.blogspot.com/-Dpl-QWxn8jM/V_CbnodjnOI/AAAAAAAABQc/TZwMkByf6RcEK6iC6BTcklkTnNIeiirnACLcB/s640/concurrency-level-too-small-large.png)](https://2.bp.blogspot.com/-Dpl-QWxn8jM/V_CbnodjnOI/AAAAAAAABQc/TZwMkByf6RcEK6iC6BTcklkTnNIeiirnACLcB/s1600/concurrency-level-too-small-large.png)  
So Segment array size should not be too big or should not be too small because,   
Using a significantly higher value than we will waste space and time, and a significantly lower value can lead to thread competition.   


### Question 6.  If we choose ConcurrenyLevel as 10 then what will be size of Segment array? Is Segment array size exactly same as concurrenyLevel? If No, then how is the Segment array size calculated?

Segment array size is calculated based on concurrenyLevel specified but it doesn't mean it will be exactly same as concurrenyLevel.  
  
**If concurrenyLevel is 10 then Segment array size will be 16.**   
  
Segment array size = 2 to the power x, where result should be &gt;= concurrenyLevel\(in our case it is 10\)  
Segment array size = 2 to the power x &gt;= 10  
  
Segment array size = 2 ^ 1 = 2   &gt;= 10 \(False\)  
Segment array size = 2 ^ 2 = 4   &gt;= 10 \(False\)  
Segment array size = 2 ^ 3 = 8   &gt;= 10 \(False\)  
Segment array size = 2 ^ 4 = 16 &gt;= 10 **\(True\)**   
  
**Segment array size is 16.**  
  
**Example: 2**  
concurrenyLevel = 8 then Segment array size = ?  
Find 2 ^ x &gt;= 8   
  
2 ^ 1 &gt;= 2   
2 ^ 2 &gt;= 4   
2 ^ 3 &gt;= 8   
Segment array size will be 8.  


### Question 7.  What is HashEntry array and how is the size of HashEntry decided?

Default initial capacity of CHM is 16. It means CHM make sure there is sufficient space to accomodate 16 key-value pairs after CHM is created.  
  
**What is HashEntry\[\]?**  
  
We saw that each index of Segment array itself i**s** complete HashMap,   
[![](https://2.bp.blogspot.com/-5XRckpIGUFU/V_Hk_Hvj9AI/AAAAAAAABQ8/qhwhnlGcrrscqhIcBrBJRXcQu34eJKL-wCLcB/s640/consurrenthashmap-capacity.png)](https://2.bp.blogspot.com/-5XRckpIGUFU/V_Hk_Hvj9AI/AAAAAAAABQ8/qhwhnlGcrrscqhIcBrBJRXcQu34eJKL-wCLcB/s1600/consurrenthashmap-capacity.png)    
  
**In HashMap the bucket/array is of class Entry\[\] and in CHM the array is of class HashEntry\[\].**  
    
[?](http://javabypatel.blogspot.com/2016/09/concurrenthashmap-interview-questions.html#)

| 12345678910 | `static` `final` `class` `Segment<K,V>` `extends` `ReentrantLock` `implements` `Serializable { transient` `volatile` `HashEntry<K,V>[] table;}` `static` `final` `class` `HashEntry<K,V> { final` `int` `hash; final` `K key; volatile` `V value; volatile` `HashEntry<K,V> next;}` |
| --- |


  
**Lets see how internal architecture of CHM looks like,**   
  
[![](https://2.bp.blogspot.com/-SHDKO9Lb3ok/V_HuycFrD0I/AAAAAAAABRM/N7qw1kN2WcEou4zE99hjycjs2AdNAZ8UwCLcB/s640/segment-hashentry-array.png)](https://2.bp.blogspot.com/-SHDKO9Lb3ok/V_HuycFrD0I/AAAAAAAABRM/N7qw1kN2WcEou4zE99hjycjs2AdNAZ8UwCLcB/s1600/segment-hashentry-array.png)  
  
**How HashEntry\[\] array size will is calculated?**   
[?](http://javabypatel.blogspot.com/2016/09/concurrenthashmap-interview-questions.html#)

| 1 | `ConcurrentHashMap m =` `new` `ConcurrentHashMap(initialCapacity, loadFactor, concurrencyLevel)` |
| --- |


  
HashEntry\[\] array size  =   2 ^ x   &gt;=  \(initialCapacity / concurrenyLevel\)  
  
**Eg: ConcurrentHashMap\(32,   0.75f,   4\);**   
HashEntry\[\] array size  =  2 ^ 1 = 2   &gt;=  8\(32/4\) \(False\)  
HashEntry\[\] array size  =  2 ^ 2 = 4   &gt;=  8 \(False\)  
HashEntry\[\] array size  =  2 ^ 3 = 8   &gt;=  8 **\(True\)**  
  
****HashEntry\[\] array size = ****8**.**   
**It means there will always be capacity of 8 key-value pairs that can be put in CHM after its creation.**  


### Question 8.  How does ConcurrentHashMap handle rehashing while another thread is still writing on another segment/partition?

For understanding rehashing, please understand Load Factor first.  
**Load Factor** is a measure, which decides when exactly to increase the HashMap/CHM capacity\(buckets\) to maintain get and put operation complexity of O\(1\).  
  
Default load factor of Hashmap/CHM is 0.75f \(i.e 75% of current map size\).  
  
**For more details on Load factor, Please refer:** [**Load Factor in HashMap**](http://javabypatel.blogspot.in/2015/10/what-is-load-factor-and-rehashing-in-hashmap.html) ****  
  
[![](https://3.bp.blogspot.com/-eHGD8zneRbI/VhNYVf6_AaI/AAAAAAAAAd8/BaP2VrEfD7QEnLICoHoOl27LZoUNzE7ywCPcB/s640/hashmapRehashing.png)](https://3.bp.blogspot.com/-eHGD8zneRbI/VhNYVf6_AaI/AAAAAAAAAd8/BaP2VrEfD7QEnLICoHoOl27LZoUNzE7ywCPcB/s1600/hashmapRehashing.png)  
**In CHM, Every segment is separately rehashed so there is no collision between Thread 1 writing to Segment index 2 and Thread 2 writing to Segment index 5.**  
  
**Example:** If say Thread 1 which is putting data in Segment\[\] array index 2 finds that HashEntry\[\] array needs to be rehashed due to exceed Load factor capacity then it will rehash HashEntry\[\] array present at Segment\[\] array index 2 only.  
HashEntry\[\] array at other Segment indexes will still be intact, unaffected and continue to serve put and get request parallely.  


