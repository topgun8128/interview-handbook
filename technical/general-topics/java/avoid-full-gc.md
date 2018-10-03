# Avoid long GC pause time



{% embed data="{\"url\":\"https://dzone.com/articles/how-to-reduce-long-gc-pause\",\"type\":\"link\",\"title\":\"How to Reduce Long GC Pauses - DZone Java\",\"description\":\"Garbage collection is necessary, but it can be a performance killer if not done well. Take these steps to make sure your GC pauses are minimal and short.\",\"icon\":{\"type\":\"icon\",\"url\":\"https://dzone.com/themes/dz20/images/favicon.png\",\"aspectRatio\":0},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://dz2cdn1.dzone.com/storage/article-thumb/3903632-thumb.jpg\",\"width\":750,\"height\":469,\"aspectRatio\":0.6253333333333333}}" %}

## How to Reduce Long GC Pauses

### 1. High Object Creation Rate

If your application’s object creation rate is very high, then to keep up with it, the garbage collection rate will also be very high. A high garbage collection rate will increase the GC pause time as well. Thus, optimizing the application to create fewer objects is THE EFFECTIVE strategy to reduce long GC pauses. This might be a time-consuming exercise, but it is 100% worth doing. In order to optimize the object creation rate in the application, you can consider using Java profilers like [JProfiler](https://www.ej-technologies.com/products/jprofiler/overview.html), [YourKit](https://yourkit.com/), or JVisualVM. These profilers will report:

* What are the objects that have been created?
* What is the rate at which these objects are created?
* What is the amount of space they are occupying in memory?
* Who is creating them?

Always try to optimize the objects that occupy the most amount of memory. Go after the big fish in the pond.

### 2. Undersized Young Generation

When the young generation is undersized, objects will be prematurely promoted to the old generation. Collecting garbage from the old generation takes more time than collecting it from the young generation. Thus, increasing the young generation size has the potential to reduce long GC pauses. The young generation's size can be increased by setting one of the two JVM arguments

**-Xmn**: specifies the size of the young generation.

**-XX:NewRatio**: Specifies the size of the young generation relative to the old generation. For example, setting -XX:NewRatio=2 means that the ratio between the old and young generation is 1:2. The young generation will be half the size of the overall heap. So, if the heap size is 2 GB, then the young generation size would be 1 GB.

  
**Q: What should be the value of -Xmn?**

Even though IBM does not make any recommendations, IBM recommends a value of 1/4 of the total heap size for -Xmn.

### 3. Choice of GC Algorithm

Your GC algorithm has a major influence on the GC pause time. If you are a GC expert or intend to become one \(or someone on your team is a GC expert\), you can tune GC settings to obtain optimal GC pause time. If you don’t have a lot of GC expertise, then I would recommend using the G1 GC algorithm because of its **auto-tuning** capability. In G1 GC, you can set the GC pause time goal using the system property ‘-XX:MaxGCPauseMillis.’ Example:

```text
 -XX:MaxGCPauseMillis=200
```

As per the above example, the maximum GC pause time is set to 200 milliseconds. This is a soft goal, which JVM will try it’s best to meet it. 

There are five widely used garbage collector solutions for OpenJDK:

* G1
* Parallel
* ConcMarkSweep \(CMS\)
* Serial
* Shenandoah

### 4. Process Swapping

If you find your processes are swapping, then do one of the following:

* Allocate more RAM to the serve.
* Reduce the number of processes that running on the server so that it can free up the memory \(RAM\).
* Reduce the heap size of your application \(which I wouldn’t recommend, as it can cause other side effects. Still, it could potentially solve your problem\).

