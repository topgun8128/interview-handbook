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

```text
if there is new data to send
  if the window size >= MAXPAYLOAD and available data is >= MAXPAYLOAD
    send complete MAXPAYLOAD segment now
  else
    if there is unconfirmed data still in the pipe
      enqueue data in the buffer until an acknowledge is received
    else
      send data immediately
    end if
  end if
end if
```

> where MAXPALOAD = maximum segment size

###  **7\) What is multicasting or multicast transmission? Which Protocol is generally used for multicast? TCP or UDP?**

Multi-casting or multicast transmission is **one to many distribution**, where message is delivered to a group of subscribers simultaneously in a single transmission from publisher. Copies of messages are automatically created in other network elements e.g. Routers, but only when the topology of network requires it. [Tibco Rendezvous supports multicast transmission](http://javarevisited.blogspot.sg/2011/01/tibco-tutorial-rvd-rendezeous-daemon-vs.html). **Multi-casting can only be implemented using UDP**, because it sends full data as datagram package, which can be replicated and delivered to other subscribers. Since **TCP is a point-to-point protocol**, it can not deliver messages to multiple subscriber, until it has link between each of them. Though, UDP is not reliable, and messages may be lost or delivered out of order. **Reliable multicast protocols** such as **Pragmatic General Multicast \(PGM\)** have been developed to **add loss detection and retransmission on top of IP multicast**. IP multicast is widely deployed in enterprises, commercial stock exchanges, and multimedia content delivery networks. A common enterprise use of IP multicast is for IPTV applications  


More on Multicast:  
[http://belkin1053.blogspot.com/2011/12/unicastbroadcastmulticast.html](http://belkin1053.blogspot.com/2011/12/unicastbroadcastmulticast.html)

#### Unicast/Broadcast/Multicast 的區別

1.Unicast\(單點傳播\)  
通常指的是特定的目的地位址，一般是主機之間互相傳遞封包的方式，也是最常見的網路通訊方式。  
因此我們有時稱之為One-to-One的通訊方式。  
[![](http://2.bp.blogspot.com/-VlgA_j24JKY/Ttop14KjGRI/AAAAAAAAABI/r58HC8Tshvs/s400/Unicast.jpg)](http://2.bp.blogspot.com/-VlgA_j24JKY/Ttop14KjGRI/AAAAAAAAABI/r58HC8Tshvs/s1600/Unicast.jpg)以Ethernet網路架構而言，封包\(Packet\)在同一個subnet中傳遞時，以收方位址來判別該由那台主機接收；若在不同的subnet時，就 要透過路由器Router根據收方位址，把這個packet送往收方主機所在的另一個subnet上。這就是Internet上最普遍、一對一方式傳 送的unicast。  
  
2.Broadcast\(廣播\)  
通常發生於 MultiAccess \(CSMA/CD\)  網路媒介中，例如區域網路\(Local Area Network\)。  
在Layer 2中表頭的MAC目的地位址通常是 FF-FF-FF-FF-FF-FF。  
在Layer 3中表頭的IP目的地位址通常是 255.255.255.255。  
連接至同一個網段\(Segment\)網路媒介上的所有主機及網路設備都會接收到這個封包並進行處理。  
[![](http://2.bp.blogspot.com/-qej9D7g7rIU/Ttop1Eord9I/AAAAAAAAAA8/ynZVXEifHSA/s400/Broadcast.jpg)](http://2.bp.blogspot.com/-qej9D7g7rIU/Ttop1Eord9I/AAAAAAAAAA8/ynZVXEifHSA/s1600/Broadcast.jpg)  
當 broadcast 時，同一subnet上所有主機都會收到 broadcast packet。  
但是 broadcast packet會被 Subnet Router 擋下來，不會傳送到另一個 subnet，否則網路就會被broadcast packet 癱瘓，因此我們稱之為One-to-All的通訊方式。  
  
3.Multicast\(多播/群播\)  
一般應用於相同的來源資料要同時傳送給一群特定的接收者\(Multicast Group Client\)，但是來源端只要發送一份資料，因此頻寬的使用量不會因為接收者增加而增加。  
[![](http://3.bp.blogspot.com/-vSFaVlWHRXE/Ttop1oUj31I/AAAAAAAAABA/JaAtAf2kKRw/s400/Multicast.jpg)](http://3.bp.blogspot.com/-vSFaVlWHRXE/Ttop1oUj31I/AAAAAAAAABA/JaAtAf2kKRw/s1600/Multicast.jpg)  
網路視訊\(如VoD/遠距教學/視訊會議\)的最佳解決方案。  
multicast是一對一個群組\(group\)的傳輸模式，不同於broadcast的是，同一subnet中只有參加multicast group的主機才會收到封包，其他的主機就不會受到無謂的干擾，而且multicast  packet會透會multicast router的運作將封包送到另一個subnet的multicast group。  
另一方面，multicast和broadcast相同的特性是，不管接收封包的主機有幾台，都只有一個資料流，也就是說，同一個subnet裏，不管接收主機的數量，所需的頻寬都是一樣的。  
因此我們稱之為One-to-Many\(or Many-to-Many\)的通訊方式。  
  
其中Class  D的start bits為"1110"，範圍從224.0.0.0~239.255.255.255，這段位址不屬於任何主機，是特別保留給 multicast  address。當一部主機要送multicast packet到某一個 group的主機時，他們之間要先選定一個閒置的Class D IP，並避免和其他群組的multicast packet IP 相同。  
然後這個multicast packet送出時，網路上參與這個 group 的主機，除了接收屬於自己IP的packet之外，也會接收自這個Class D IP 的packet。於是達成了 multicast 的目的了。

**Class D** Address Very first four bits of the first octet in Class D IP addresses are set to **1110**, giving a range of:

Class D has IP address rage from **224.0.0.0 to 239.255.255.255**. Class D is reserved for Multicasting. In multicasting data is not destined for a particular host, that is why there is no need to extract host address from the IP address, and **Class D does not have any subnet mask**.

###  **8\) What is difference between Topic and Queue in JMS?**

Main difference between Topic and Queue in Java Messaging Service comes when we have multiple consumers to consumer messages. **If we set-up multiple** [**listener thread**](http://javarevisited.blogspot.sg/2011/02/how-to-implement-thread-in-java.html) **to consume messages from Queue, each messages will be dispatched to only one thread and not all thread.** On the other hand in case of **Topic each subscriber gets it's own copy of message.**  


###  **11\) What is ephemeral port?**

In TCP/IP connection usually contains four things, Server IP, Server port, Client IP and Client Port. Out of these four, 3 are well known in most of the time, what is not known is client port, this is where ephemeral ports comes into picture. **ephemeral ports are dynamic port assigned by your machine's IP stack, from a specified range**, known as ephemeral port range, when a client connection explicitly doesn't specify a port number. These are short lived, temporary port, which can be reused once connection is closed, but most of IP software, doesn't reuse ephemeral port, until whole range is exhausted. Similar to TCP, UDP protocol also uses ephemeral port, while sending datagram . In **Linux** ephemeral port range is from **32768 to 61000**, while in **windows** default ephemeral port range is **1025 to 5000**. Similarly different operating system has different ephemeral port ranges

### **12\) What is sliding window protocol?**

Sliding window protocol is a technique for controlling transmitted data packets between two network computers where reliable and sequential delivery of data packets is required, such as provided by Transmission Control Protocol \(TCP\). In the sliding window technique, each packet includes a unique consecutive sequence number, which is used by the receiving computer to place data in the correct order. The objective of the sliding window technique is to use the sequence numbers to avoid duplicate data and to request missing data  
More on:  
[https://www.geeksforgeeks.org/sliding-window-protocol-set-1/](https://www.geeksforgeeks.org/sliding-window-protocol-set-1/)

###  **13\) When do you get "too many files open" error?**

Just like File connection, Socket Connection also needs file descriptors, Since every machine has limited number of file descriptors, it's possible that they may ran out of file descriptors. When it happen, you will see ["too many files open"](http://javarevisited.blogspot.sg/2013/08/how-to-fix-javanetsocketexception-too-many-open-files-java-tomcat-weblogic.html) error. You can check how many file descriptor per process is allowed on UNIX based system by executing `ulimit -n` command or simply count entries on `/proc//fd/`  
  
To display open file descriptors:

```text
# Method 1
lsof -p 28290
lsof -a -p 28290
# Method 2
cd /proc/28290/fd
ls -l | less
ls -l | wc -l
```

###  **14\) What is TIME\_WAIT state in TCP protocol? When does a socket connection goes to TIME\_WAIT state?**

When one end of TCP Connection closes it by making system call, it goes into TIME\_WAIT state. Since TCP packets can arrive in wrong order, the port must not be closed immediately to allow late packets to arrive. That's why that end of TCP connection goes into TIME\_WAIT state. For example, if client closes a socket connection than it will go to TIME\_WAIT state, similarly if server closes connection than you will see TIME\_WAIT there. You can check status of your TCP and UDP sockets by using these [networking commands in UNIX](http://javarevisited.blogspot.sg/2010/10/basic-networking-commands-in-linuxunix.html).

```text
   ESTABLISHED
          The socket has an established connection.
   SYN_SENT
          The socket is actively attempting to establish a connection.
   SYN_RECV
          A connection request has been received from the network.
   FIN_WAIT1
          The socket is closed, and the connection is shutting down.
   FIN_WAIT2
          Connection is closed, and the socket is waiting for  a  shutdown
          from the remote end.
   TIME_WAIT
          The socket is waiting after close to handle packets still in the
          network.
   CLOSE  The socket is not being used.
   CLOSE_WAIT
          The remote end has shut down, waiting for the socket to close.
   LAST_ACK
          The remote end has shut down, and the socket is closed.  Waiting
          for acknowledgement.
   LISTEN The  socket is listening for incoming connections.  Such sockets
          are  not  included  in  the  output  unless  you   specify   the
          --listening (-l) or --all (-a) option.
   CLOSING
          Both  sockets are shut down but we still don't have all our data
          sent.
   UNKNOWN
          The state of the socket is unknown.
```

###  **15\) What will happen if you have too many socket connections in TIME\_WAIT state on Server?**

When a socket connection or port goes into TIME\_WAIT state, it doesn't release file descriptor associated with it. File descriptor is only released when TIME\_WAIT state is gone i.e. after some specified configured time. If too many connections are in TIME\_WAIT state than your Server may ran out of file descriptors and start throwing "**too many files open**" error, and **stop accepting new connections.**  




## More References

{% embed data="{\"url\":\"https://crunchify.com/java-nio-non-blocking-io-with-server-client-example-java-nio-bytebuffer-and-channels-selector-java-nio-vs-io/\",\"type\":\"link\",\"title\":\"Java NIO \(Non-blocking I/O\) with Server-Client Example - java.nio.ByteBuffer and channels.Selector - Java NIO Vs. IO • Crunchify\",\"description\":\"Java NIO is my favorite topic. I have been working with NIO since last 2 years and would like to share simple Server-Client code for my readers who are\",\"icon\":{\"type\":\"icon\",\"url\":\"https://cdn.crunchify.com/wp-content/uploads/2017/07/cropped-favicon-small-1.png\",\"width\":192,\"height\":192,\"aspectRatio\":1},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://cdn.crunchify.com/wp-content/uploads/2016/02/Java-NIO-client-server-example-with-Selector-and-Channels.png\",\"width\":1424,\"height\":752,\"aspectRatio\":0.5280898876404494}}" %}

{% embed data="{\"url\":\"http://cs.lmu.edu/~ray/notes/javanetexamples/\",\"type\":\"link\",\"title\":\"javanetexamples\"}" %}

