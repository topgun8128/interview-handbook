# Quick Summary

## Collections

**Name the core Collection classes.**

* ArrayList
* LinkedList
* PriorityQueue
* HashSet

{% hint style="info" %}
HashMap is not in Collection classes.

Maps work with key/value pairs, while the other collections work with just values. Collections have, for example, add\(myValue\) methods, where Maps have put\(myKey,myValue\) methods. The Interface Map doesn't extend the Interface Collection because it has a different interface. 
{% endhint %}

**What change does autoboxing bring to Collection?**

Since we know that collection **cannot store primitive types**, only **references**. Collection still store only references but autoboxing/unboxing facilitate it to **happen automatically**, with no need to wrap it manually.

```java
List<Integer> tempList = new ArrayList<Integer>();
// Storing in collection using autoboxing
tempList.add(1);
// wrapping primitive int 2 to Integer manually, with out autoboxing
tempList.add(new Integer(2));
```

**Does ArrayList allow duplicate elements?**

Yes. List allows duplicate elements to be added.

**Does ArrayList allow null?**

In ArrayList any number of nulls can be added.

**How to join two or more ArrayLists?**

List provides a method addAll to join two or more lists in Java, e.g.:

```text
cityList.addAll(secondCityList);
```

**How to get a synchronized ArrayList?**

```text
Collections.synchronizedList(myList);
```







## Java 8

