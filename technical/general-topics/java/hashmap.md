# HashMap

## [https://netjs.blogspot.com/2015/05/how-hashmap-internally-works-in-java.html?m=1](https://netjs.blogspot.com/2015/05/how-hashmap-internally-works-in-java.html?m=1)

#### How HashMap Internally Works in Java

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

**HashMap changes in Java 8**

Though HashMap implementation in Java provides constant time performance **O\(1\)** for get\(\) and put\(\) methods but that is in the **ideal case** when the Hash function distributes the objects evenly among the buckets. 

But the performance may worsen in the case **hashCode\(\)** used is not proper and there are lots of hash collisions. As we know now that in case of hash collision entry objects are stored as a node in a linked-list and **equals\(\)** method is used to compare keys. That comparison to find the correct key with in a linked-list is a linear operation so in a worst case scenario the complexity becomes O\(n\).

To address this issue in Java 8 hash elements **use balanced trees instead of linked lists after a certain threshold is reached**. Which means HashMap starts with storing Entry objects in linked list but after the number of items in a hash becomes larger than a certain threshold, the hash will change from using a linked list to a balanced tree, this will improve the worst case performance **from O\(n\) to O\(log n\).**

**Points to note -**

* HashMap works on the principal of hashing.
* HashMap uses the hashCode\(\) method to calculate a hash value. Hash value is calculated using the key object. This hash value is used to find the correct bucket where Entry object will be stored.
* HashMap uses the equals\(\) method to find the correct key whose value is to be retrieved in case of get\(\) and to find if that key already exists or not in case of put\(\).
* **Hashing collision means more than one key having the same hash value**, in that case Entry objects are stored as a linked-list with in a same bucket.
* With in a bucket values are stored as Entry objects which contain both key and value.
* In Java 8 hash elements use balanced trees instead of linked lists after a certain threshold is reached while storing values. This improves the worst case performance from O\(n\) to O\(log n\). 

That's all for this topic **How HashMap Internally Works in Java**. If you have any doubt or any suggestions to make please drop a comment. Thanks!

