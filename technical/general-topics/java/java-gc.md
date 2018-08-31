# Java GC

{% embed data="{\"url\":\"https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html\",\"type\":\"link\",\"title\":\"Java Garbage Collection Basics\"}" %}





![](../../../.gitbook/assets/image%20%283%29.png)

Java 8 has no PermGen, however the MetaSpace provide the same function and works similar but no the same.

Java &lt;= 7 has a PermGen and while it is a Java managed space is it not the same heap which objects are allocated in. It doesn't count towards the maximum heap size nor does it count toward the 32 GB limit addressable by CompressOops for example. It is cleaned up as a part of the garbage collection.

It makes sense to have it in the same diagram, but it doesn't mean PermGen/MetaSpace is part of the same heap.

Also note that when the heap space runs out you get

```text
java.lang.OutOfMemoryError: Heap space space
```

However when you run out of PermGen you get a different error.

```text
java.lang.OutOfMemoryError: PermGen space
```

[https://plumbr.eu/outofmemoryerror/java-heap-space](https://plumbr.eu/outofmemoryerror/java-heap-space)

[https://plumbr.eu/outofmemoryerror/permgen-space](https://plumbr.eu/outofmemoryerror/permgen-space)

[https://plumbr.eu/outofmemoryerror/metaspace](https://plumbr.eu/outofmemoryerror/metaspace)

In short, Metaspace size auto increases in native memory as required to load class metadata if not restricted with `-XX:MaxMetaspaceSize`





{% embed data="{\"url\":\"https://www.quora.com/How-does-garbage-collection-work-internally-in-Java\",\"type\":\"link\",\"title\":\"How does garbage collection work internally in Java? - Quora\",\"icon\":{\"type\":\"icon\",\"url\":\"https://qsf.fs.quoracdn.net/-3-images.favicon.ico-26-ae77b637b1e7ed2c.ico\",\"aspectRatio\":0}}" %}

**Important points about Java Garbage Collection**

1. Objects are created on the heap in Java irrespective of their scope e.g. local or member variable. while it's worth noting that class variables or static members are created in method area of Java memory space and both heap and method area is shared between different thread.

2. Garbage collection is a mechanism provided by Java Virtual Machine to reclaim heap space from objects which are eligible for Garbage collection.

3. Garbage collection relieves Java programmer from memory management which is an essential part of C++ programming and gives more time to focus on business logic.

4. Garbage Collection in Java is carried by a daemon thread called Garbage Collector.

5. Before removing an object from memory garbage collection thread invokes finalize\(\) method of that object and gives an opportunity to perform any sort of cleanup required.

6. Java programmer can not force garbage collection in Java; it will only trigger if JVM thinks it needs a garbage collection based on Java heap size.

7. There are methods like System.gc\(\) and Runtime.gc\(\) which is used to send request of Garbage collection to JVM but it’s not guaranteed that garbage collection will happen.

8. If there is no memory space for creating a new object in Heap, then JVM throws java.lang.OutOfMemoryError heap space.

**When an Object becomes Eligible for Garbage Collection**

An object becomes eligible for Garbage collection if it's not reachable from any live threads or by any static references. In other words, you can say that an object becomes eligible for garbage collection if its all references are null. Cyclic dependencies are not counted as the reference so if object A has a reference to object B and object B has a reference to Object A and they don't have any other live reference then both Objects A and B will be eligible for Garbage collection. Generally, an object becomes eligible for garbage collection in Java on following cases:

1. All references to that object explicitly set to null e.g. object = null

2. The object is created inside a block and reference goes out scope once control exit that block.

3. Parent object set to null if an object holds the reference to another object and when you set container object's reference null, child or contained object automatically becomes eligible for garbage collection.

Java has four types of garbage collectors,

1. Serial Garbage Collector

2. Parallel Garbage Collector

3. CMS Garbage Collector

4. G1 Garbage Collector

**1. Serial Garbage Collector**

Serial garbage collector works by holding all the application threads. It is designed for the single-threaded environments. It uses just a single thread for garbage collection. The way it works by freezing all the application threads while doing garbage collection may not be suitable for a server environment. It is best suited for simple command-line programs. Turn on the -XX:+UseSerialGC JVM argument to use the serial garbage collector.

**2. Parallel Garbage Collector**

Parallel garbage collector is also called as throughput collector. It is the default garbage collector of the JVM. Unlike serial garbage collector, this uses multiple threads for garbage collection. Similar to serial garbage collector this also freezes all the application threads while performing garbage collection. Turn on the -XX:+UseParallelGC JVM argument to use the parallel garbage collector.

**3. CMS Garbage Collector**

Concurrent Mark Sweep \(CMS\) garbage collector uses multiple threads to scan the heap memory to mark instances for eviction and then sweep the marked instances. CMS garbage collector holds all the application threads in the following two scenarios only,

1. While marking the referenced objects in the tenured generation space.

2. If there is a change in heap memory in parallel while doing the garbage collection.

In comparison with parallel garbage collector, CMS collector uses more CPU to ensure better application throughput. If we can allocate more CPU for better performance then CMS garbage collector is the preferred choice over the parallel collector. Turn on the XX:+USeParNewGC JVM argument to use the CMS garbage collector.

**4. G1 Garbage Collector**

G1 garbage collector is used for large heap memory areas. It separates the heap memory into regions and does collection within them in parallel. G1 also does compacts the free heap space on the go just after reclaiming the memory. But CMS garbage collector compacts the memory on stop the world \(STW\) situations. G1 collector prioritizes the region based on most garbage first. Turn on the –XX:+UseG1GC JVM argument to use the G1 garbage collector.



{% embed data="{\"url\":\"https://www.dynatrace.com/resources/ebooks/javabook/how-garbage-collection-works/\",\"type\":\"link\",\"title\":\"How Garbage Collection Works - Java Enterprise Performance \| Dynatrace\",\"icon\":{\"type\":\"icon\",\"url\":\"https://dt-cdn.net/images/apple-touch-icon-180x180-180-80738a22c0.png\",\"width\":180,\"height\":180,\"aspectRatio\":1},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://dt-cdn.net/images/dynatrace-social-preview-600-a5d204df7d.jpg\",\"width\":600,\"height\":375,\"aspectRatio\":0.625}}" %}

