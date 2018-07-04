# Java NIO

## [http://tutorials.jenkov.com/java-nio/nio-vs-io.html](http://tutorials.jenkov.com/java-nio/nio-vs-io.html)

### Main Differences Betwen Java NIO and IO 

| **IO** | **NIO** |
| --- | --- | --- | --- |
| Stream oriented | Buffer oriented |
| Blocking IO | Non blocking IO |
|   | Selectors |

### Stream Oriented vs. Buffer Oriented

The first big difference between Java NIO and IO is that IO is stream oriented, where NIO is buffer oriented. So, what does that mean?

**Java IO** being **stream oriented** means that you **read one or more bytes at a time, from a stream**. What you do with the read bytes is up to you. They are **not cached** anywhere. Furthermore, you **cannot move forth and back in the data in a stream**. If you need to move forth and back in the data read from a stream, you will **need to cache it in a buffer** first.

**Java NIO's buffer oriented** approach is slightly different. Data is read into a buffer from which it is later processed. You can move forth and back in the buffer as you need to. This gives you a bit more flexibility during processing. However, you also need to check if the buffer contains all the data you need in order to fully process it. And, **you need to make sure that when reading more data into the buffer, you do not overwrite data in the buffer you have not yet processed.**

### Blocking vs. Non-blocking IO

 Java IO's various streams are blocking. That means, that when a thread invokes a `read()` or `write()`, that thread is blocked until there is some data to read, or the data is fully written. The thread can do nothing else in the meantime.

Java NIO's non-blocking mode enables a thread to request reading data from a channel, and only get what is currently available, or nothing at all, if no data is currently available. Rather than remain blocked until data becomes available for reading, the thread can go on with something else.

The same is true for non-blocking writing. A thread can request that some data be written to a channel, but not wait for it to be fully written. The thread can then go on and do something else in the mean time.

What threads spend their idle time on when not blocked in IO calls, is usually performing IO on other channels in the meantime. That is, a single thread can now manage multiple channels of input and output.

### Selectors

Java NIO's selectors allow a **single thread to monitor multiple channels of input**. You can register multiple channels with a selector, then use a single thread to "select" the channels that have input available for processing, or select the channels that are ready for writing. This selector mechanism makes it easy for a single thread to manage multiple channels.

### Summary

**NIO allows you to manage multiple channels \(network connections or files\) using only a single \(or few\) threads, but the cost is that parsing the data might be somewhat more complicated than when reading data from a blocking stream.**

If you need to manage thousands of open connections simultanously, which each only send a little data, for instance a chat server, implementing the server in NIO is probably an advantage. Similarly, if you need to keep a lot of open connections to other computers, e.g. in a P2P network, using a single thread to manage all of your outbound connections might be an advantage. This one thread, multiple connections design is illustrated in this diagram:

| ![Java NIO: A single thread managing multiple connections.](http://tutorials.jenkov.com/images/java-nio/nio-vs-io-3.png) |
| --- | --- |
| **Java NIO: A single thread managing multiple connections.** |

If you have fewer connections with very high bandwidth, sending a lot of data at a time, perhaps a classic IO server implementation might be the best fit. This diagram illustrates a classic IO server design:

| ![Java IO: A classic IO server design - one connection handled by one thread.](http://tutorials.jenkov.com/images/java-nio/nio-vs-io-4.png) |
| --- | --- |
| **Java IO: A classic IO server design - one connection handled by one thread.** |

## [https://javarevisited.blogspot.com/2014/08/socket-programming-networking-interview-questions-answers-Java.html](java-nio.md#https-javarevisited-blogspot-com-2014-08-socket-programming-networking-interview-questions-answers-java-html)

### **1\) Difference between TCP and UDP protocol?**

* TCP is guaranteed delivery, UDP is not guaranteed.
* TCP guarantees order of messages, UDP doesn't.
* Data boundary is not preserved in TCP, but UDP preserves it.
* TCP is slower compared to UDP.
* UDP is a connectionless protocol, and point to point connection is not established before sending messages.

TCP is connection-oriented and offers its Clients in-order delivery. Of course this applies to the connection level: individual connections are independent. TCP "guarantees" that a receiver will receive the reconstituted stream of bytes as it was originally sent by the sender. However, between the TCP send/receive endpoints \(i.e., the physical network\), the data can be received out of order, it can be fragmented, it can be corrupted, and it can even be lost. TCP accounts for these problems using a handshake mechanism that causes bad packets to be retransmitted. The TCP stack on the receiver places these packets in the order in which they were transmitted so that when you read from your TCP socket, you are receive the data as it was originally sent.

###  **2\) How does TCP handshake works?**

Three messages are exchanged as part of TCP head-shake e.g. Initiator sends SYN,  upon receiving this Listener sends SYN-ACK, and finally initiator replied with ACK, at this point TCP connection is moved to ESTABLISHED state. This process is easily understandable by looking at following diagram.  
  
[![Java Networking Interview Questions and Answers](https://1.bp.blogspot.com/-NkyuYsoiOXE/U_NA-Y1W1MI/AAAAAAAABy8/1z0az2aawMo/s1600/TCP%2BHandshake%2BMessage%2BDiagram.jpg)](https://1.bp.blogspot.com/-NkyuYsoiOXE/U_NA-Y1W1MI/AAAAAAAABy8/1z0az2aawMo/s1600/TCP%2BHandshake%2BMessage%2BDiagram.jpg)  


###  **3\) How do you implement reliable transmission in UDP protocol?**

This is usually follow-up of previous interview question. Though UDP doesn't provide delivery guarantee at protocol level, you can introduce your own logic to maintain reliable messaging e.g. by introducing sequence numbers and retransmission. If receiver find that it has missed a sequence number, it can ask for replay of that message from Server. TRDP protocol, which is used [Tibco Rendezvous](http://javarevisited.blogspot.sg/2010/10/tibco-rv-messagging.html) \(a popular high speed messaging middle-ware\) uses UDP for faster messaging and provides reliability guarantee by using sequence number and retransmission.

###  **4\) What is Network Byte Order? How does two host communicate if they have different byte-ordering?**

little endian \(least significant byte at the starting address\)  
big endian \(most significant byte at the starting address\).   
Networking protocols such as TCP are based on a specific network byte order, which uses big-endian byte ordering.   
If two machines are communicating with each other and they have different byte ordering, they are converted to network byte order before sending or after receiving.   


### **5\) What is Nagle's algorithm?**

Nagle's algorithm is way of improving performance of TCP/IP protocol and networks by reducing number of TCP packets that needs to be sent over network. It works by buffering small packets until buffer reaches Maximum Segment Size. Since small packets, which contains only 1 or 2 bytes of data, has more overhead in terms of TCP header, which is of 40 bytes. The algorithm applies to data of any size. If the data in a single write spans 2n packets, the last packet will be withheld, waiting for the ACK for the previous packet.\[4\] In any request-response application protocols where request data can be larger than a packet, this can artificially impose a few hundred milliseconds latency between the requester and the responder, even if the requester has properly buffered the request data. Nagle's algorithm should be disabled by the requester in this case. If the response data can be larger than a packet, the responder should also disable Nagle's algorithm so the requester can promptly receive the whole response. Applications that expect real-time responses and low latency can react poorly with Nagle's algorithm. For this reason applications with low-bandwidth time-sensitive transmissions typically use TCP\_NODELAY to bypass the Nagle delay.\[5\] Another option is to use UDP instead. Read detailed explanation here:  
[https://howdoesinternetwork.com/2015/nagles-algorithm](https://howdoesinternetwork.com/2015/nagles-algorithm)

  




