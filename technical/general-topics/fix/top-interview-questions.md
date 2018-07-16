# Top Interview Questions

## [https://javarevisited.blogspot.com/2011/03/top-20-fix-protocol-interview-questions.html](https://javarevisited.blogspot.com/2011/03/top-20-fix-protocol-interview-questions.html)

**1. What is the difference between OrdStatus and ExecType in FIX4.4?**  
OrdStatus \(39\) indicates the current status of the order. ExecType \(150\) was introduced in 4.2 to indicate the type of execution report received. For example, if you replace an order that is already partially filled, the order status is Partially Filled but the ExecType for the replace confirmation is Replaced \(FIX 4.2\).  
  
**2. What is the minimum length of ClOrdID?**  
ClOrdID is a mandatory string field so the minimum length is 1. Obviously, one character doesn't make much sense because of limited flexibility. Length is determined by the combination of the executing party / vendor / exchange.  
  
  
**3. Which tag in FIX 4.4 can be used to denote Smart Order Routing?**  
There is no specific tag in FIX 4.4 that denotes Smart Order Routing. Brokers and/or executing destinations can decide from different options. They can simply go with a user-defined tag or with a standard tag such as HandlInst \(21\), ExDestination \(100\) or SecurityExchange \(207\).  
  
  
**4. What is business message reject in FIX Protocol?**  
The Business Message Reject \(j\) message can reject an application-level message which fulfills session-level rules and cannot be rejected via any other means. For example, if the fix engine accepts a tag that is not supported by the FIX application, this type of reject should be sent. See [here ](http://javarevisited.blogspot.com/2011/02/fix-protocol-tutorials-difference.html)for more detailed answer  
  
[![](https://2.bp.blogspot.com/-_7kkhXutUzs/VJLnHh8xqXI/AAAAAAAACN4/gr7Y48S_St4/s400/FIX%2BProtocol%2BInterview%2BQuestions%2Band%2BAnswers.png)](https://2.bp.blogspot.com/-_7kkhXutUzs/VJLnHh8xqXI/AAAAAAAACN4/gr7Y48S_St4/s1600/FIX%2BProtocol%2BInterview%2BQuestions%2Band%2BAnswers.png)  
**5. What do you mean by DK \(Don't know\) Trade?**  
Don't Know Trade is a MessageType \(Q\) that indicates a reject of a received execution report. For example, if you only sent one order today with ClOrdID = 10 but you received an execution report from an execution destination for ClOrdID = 20, your FIX application should reject this execution report.  
  
  
**6. Which tag is used in FIX Protocol to denote an order is for equity or for future options?**  
FIX tag 167 \(SecurityType\) should be used to identify asset type. In FIX 4.4, you are recommended to use CFICode \(461\).  
  
  
**7. What is tag RoutingID and why does it use in FIX Protocol?**  
RoutingID \(217\) is used to specify a specific routing destination. It is part of a repeating group so it's convenient if you want to specify more than one destination. It's only defined for 3 MessageTypes \(Email, News, IOI\); and only IOI is commonly used. If you are asked this in an interview, well, good luck with that one.  
  
  
**8. Can you have different OrderID on NewOrder and Modification and Cancel messages?**  
OrderID is the identifier generally provided on execution reports from the exchange / execution destination. It should remain the same throughout a trade's lifecycle regardless if you replace or cancel.  
  
  
**9. What is FIX Session?**  
It's linked to facilitate communication between FIX engines. At low level, it's just a TCP/IP connection with client authentication detail. See [here](http://javarevisited.blogspot.com/2011/02/fix-protocol-session-or-admin-messages.html) for a more detailed answer.  
  
  
**10. What do you mean by EOD of FIX Session?**  
EOD stands for End of Day and indicates a reset of sequence numbers to 1/1 in regards to an FIX session. Both incoming and outgoing sequence numbers are reset as part of EOD. Commercial FIX engines like QuickFIX, Appia or Cameron FIX allows you to specify different EOD time for the different client session.  
  
  
**11. Which FIX tag is used to denote "CARE" order in FIX Protocol?**  
FIX tag 21 \(HandlInst\) is used to indicate a CARE order. A care order is handled manually by either a trader or someone on the execution side; therefore, 21=3 \(manual\) is meant for this.  
  
  
**12. Which tag is used to denote trading capacity of order e.g. Prop or Agency?**  
Up until FIX 4.2, Rule80A \(47\) was used to indicate order capacity. Starting in FIX 4.3, OrderCapacity \(528\) was introduced.  
  
  
**13. How do you identify FIX version of an FIX message?**  
This can be done either at the FIX engine configuration level or looking at the FIX message header. FIX tag 8 indicates FIX version.  
  
  
**14. Which tag is used to denote MsgType in FIX protocol?**  
MessageType is tag 35. Different types of messages e.g. NewOrderSingle, OrderCancelRequest, OrderReplaceRequest are just different values of tag 35 e.g. 35=D is a new order, 35=G is modification and 35=F are cancel the request.  
  
  
**15. How do you handle out of sequence messages e.g. you received Canceled ack and then a fill?**  
The handling of out of sequence messages varies per firm. If a cancel is received prior to a fill, the cancel could close the order and the fill can get rejected. Some firms will allow the fill to be processed. There really is no standard here.  
  
  
**16. What do you do if your session gets disconnected intraday?**  
Pray. This is not a good thing. Just don't reset the sequence numbers; that could lead to a very costly error and possibly the loss of your job. Coordinate with the counterparty to get things back to normal. Most fix engine configurations support automatic reconnections so be careful.  
  
  
**17. What are heartbeat messages which tag you use to identify heartbeat messages?**  
Heartbeat messages are keep-alive messages; letting the other FIX engine know that you are still alive and active. Heartbeat is a MessageType \(35=0\).  
  
  
**18. What is LeavesQty which tag is used to denote LeavesQty in fix message?**  
LeavesQty \(151\) indicates how much quality is left to be executed on the order. If value of tag 151 is zero it means order is fully executed and order status would be filled, while if value of LeavesQty is greater than zero means trade is only partially executed and order status would be partial fill.  
  
  
**19. What is the equivalent of tag 20 ExecTransType in FIX 4.4?**  
ExecTransType was removed in FIX 4.3 to eliminate confusion since ExecType also is used to indicate the type of execution report received. The old values of ExecTransType have been merged into ExecType \(150\). 20=1 --&gt; 150=H \| 20=2 --&gt; 150=G \| 20=3 --&gt; 150=I. See [here](http://javarevisited.blogspot.com/2011/01/difference-between-fix-42-vs-fix-44-in.html) to learn more.  
  
  
**20. What are various FIX tags which are used for symbology identification?**  
The most common FIX tag used for symbology identification is tag 55 \(Symbol\). You can also use the combination of tag 22 \(IDSource\) and 48 \(SecurityID\).  
  
  
  


#### New FIX Protocol interview Question

I have created this new FIX Protocol Interview section to include new questions contributed by my reader and different sources. Please let me know if you have asked an FIX Protocol interview question which is not present here and I will include it for community’s benefit.  
  
**1. You placed a new order and then modification and before modification a cancel, what would be the OrigClOrdid of Cancel?**  
Since Modification request is not accepted yet so ClOrdID of original order will be in place So Cancel Request must contain OrigClOrdID \(Tag 41\) same as ClOrdID of Original Order.  
  
  
**2. You placed a new order and then place a replace request and received Pending replace message and then a fill, what would be ClOrdID of the fill?**  
Since OrderCancelReplaceRequest \(tag 35=F\) is not accepted, ClOrdID of NewOrder will be in force and fill will contain ClOrdID of the NewOrderSingle \(35=A\). It’s only after your received ExecutionReport with ExecType=Replaced your ClOrdID of the active order gets updated. Pending Replace is a just indication that broker or exchange received a Replace Request but not yet accepted or rejected it.  
  
  
**3. You placed a new order got a partial fill and place a replace and got replaced what would be the value of tag 39 and tag 150.**  
Since Order is in Partial fill status so tag 39 OrdStatus will contain partial fill and ExecType will be Replaced I thing 150=5 and 39=1.  
  
**Further Learning**  
[The Fix Guide: Implementing the FIX Protocol 2nd Edition](http://www.amazon.com/The-Fix-Guide-Implementing-Protocol/dp/1413475094?tag=javamysqlanta-20)  
[Building Winning Algorithmic Trading Systems](https://www.amazon.com/Building-Winning-Algorithmic-Trading-Systems/dp/1118778987/?tag=javamysqlanta-20)  
[Trading Systems and Methods by Perry J. Kaufman ](https://www.amazon.com/Trading-Systems-Methods-Website-Wiley/dp/1118043561?tag=javamysqlanta-20)  
[Linux Command Line Interface \(CLI\) Fundamentals](http://www.shareasale.com/m-pr.cfm?merchantID=53701&userID=880419&productID=546412444)  
[TCP/IP Networking for Developers](http://www.shareasale.com/m-pr.cfm?merchantID=53701&userID=880419&productID=546412061)  
  
These books will not only provide more details about FIX protocols but also give you good knowledge about electornic trading and Algorithmic trading, two of the key domain for any Java developer looking for good work in Investment banks.   
  
Related post:  
[Financial Information Exchange \(FIX\) Protocol Interview Questions Answers](http://javarevisited.blogspot.com/2010/12/fix-protocol-interview-questions.html)   
[FIX protocol and fix messaging interview questions](http://javarevisited.blogspot.com/2010/12/fix-protocol-interview-question.html)   
[Interview question asked on FINANCIAL INFORMATION EXCHANGE \(FIX\) Protocol](http://javarevisited.blogspot.com/2010/12/finance-interview-questions.html)   
[FIX Protocol Tutorial for beginners](http://javarevisited.blogspot.com/2011/04/fix-protocol-tutorial-for-beginners.html)   
[FIX Protocol Session or Admin messages tutorial](http://javarevisited.blogspot.com/2011/02/fix-protocol-session-or-admin-messages.html)   
[Fix Session is not connecting how to diagnose it?](http://javarevisited.blogspot.com/2011/01/fix-protocol-tutorial-fix-session-is.html)  


### By Others

Question:  
What is difference between Limit Order, Enhanced Limit Order and Special Limit order of HongKong Stock Exchange?

Answer:  
Main difference between LIMIT, Enhanced LIMIT and Special Limit order is how they executed, LIMIT order executes only at specified price or better price. Enhanced LIMIT order can **go upto 10 price queues to fill the order**, for example if want to buy HSBC at 1HKD, and price tick is 0.01HKD, it can go up to price queue 1.10 HKD to fill your order. Remaining order will be treated as Limit order, on the other hand Special limit order also goes upto 10 Price Queue to fill the order but remaining order get cancelled and not stored in market.

More references:  
[https://its.bochk.com/investment/securities/help/ibs\_sec\_glossary\_c.html](https://its.bochk.com/investment/securities/help/ibs_sec_glossary_c.html)

What are the FIX messages which is used in FX trading? \(msgtype W and X are some examples\) R, Q, V, W, D, 8  
What are the FIX messages which is used for publishing market data? W  


On morgan stanley interview :  
  
1\) what is difference between Odd lot and Round lot in Stocks? in   
Answer : An odd lot is a number of shares less than 100 \(1-99\), while a "Round Lot" is 100 shares of stock. Any number of shares that is a multiple of 100 is also a round lot \(i.e. 100, 600, 1,600, etc\)  
  
2\) Name two exchange traded derivatives?  
Answer - Futures and Options  
  


What does tag 1 represent in FIX protocol?  
  
Read more: [https://javarevisited.blogspot.com/2011/03/top-20-fix-protocol-interview-questions.html\#ixzz5LRBmKZER](https://javarevisited.blogspot.com/2011/03/top-20-fix-protocol-interview-questions.html#ixzz5LRBmKZER)Account mnemonic as agreed between buy and sell sides, e.g. broker and institution or investor/intermediary and fund manager.  


If you place an order and then make modification to it before ack from sell side. What happens to orginal order?  
  
It depends upon how Sell side handles it, until sell side broker ack it, order is in Pending NEW state and any modification will likely to be rejected, but some broker may queue it, returning first NEW and then REPLACED messages.  
  


Can we send a Cancel followed by NEW instead of sending Modification?

Yup, you can. But in case of MOD, many exchange keep the original position of order, but when you cancel you lose the position and NEW will always be added at the end of price queue \(tail\).  
  
  
  




## [https://www.wisdomjobs.com/e-university/fix-protocol-interview-questions.html](https://www.wisdomjobs.com/e-university/fix-protocol-interview-questions.html)

**Question 1. What Do You Mean By Warrant?**

**Answer :**

Warrant is a financial product which gives right to holder to Buy or Sell underlying financial security, its similar to option with some differences e.g. Warrants are normally issued by banks while options are primarily traded in exchange.

**Question 2. What Is Mean By Settlement Of Securities? When Settlement Does Occur?**

**Answer :**

In Simple term **Settlement means money will deducted from buyers and account and security\(Shares\) will be credited to his account , normally Settlement occurs after few days of trade date for most of the exchange its T+3** \(i.e. Three days after trade date\) , T denotes Trade date means the date on which transaction has taken place.

For some of the exchanges e.g. NSE India, **SEHK Hongkong its T+2**.

**Question 3. What Is Newordersingle, Order Cancel Replace And Order Cancel Reject Message?**

**Answer :**

* These are the basic, most commonly used messages in Electronic trading via FINANCIAL INFORMATION EXCHANGE \(FIX\) protocol.
* NewOrderSingle message is denoted by MsgType=D and its used to place an Order, Order Cancel Replace Request is modification request denoted by MsgType=G in FINANCIAL INFORMATION EXCHANGE \(FIX\) protocol and used to modify Order e.g for changing quantity or price of Order.
* Order Cancel Request is third in this category denoted by MsgType=F in FINANCIAL INFORMATION EXCHANGE \(FIX\) protocol and used to cancel Order placed into Market.

**Question 4. What Are Most Common Issues Encounter When Two Fix Engine Communicates?**

**Answer :**

When Clients connect to broker via FINANCIAL INFORMATION EXCHANGE \(FIX\) protocol, there FIX engine connects to each other, while setting up and during further communication many issues can occur below are some of most common ones:

* Issues related to network connectivity.
* Issues related to **Firewall rules**.
* Issue related to **incorrect host/port** name while connecting.
* **Incorrect SenderCompID and TargetCompID.**
* **Sequence Number mismatch.**
* Issue related to FINANCIAL INFORMATION EXCHANGE **\(FIX\) version mismatch**.
* **Heartbeat interval not match &lt;- added by me**

**Question 5. What Do You Mean By Incoming Sequence No And Outgoing Sequence No? Which Tag Is Used To Carry Sequence No?**

**Answer :**

Sequence Number is very important concept of FINANCIAL INFORMATION EXCHANGE \(FIX\) protocol which essentially provides it Recovery and replay functionality and ensures that no message will lose during transmission or communication. In FINANCIAL INFORMATION EXCHANGE \(FIX\) protocol every message contains a unique sequence number defined in tag 34. Logically we can divide sequence number into two Incoming and Outgoing Sequence number. 

Incoming sequence number is the number any FIX Engine expecting from Counter Party and Outgoing sequence number is the number any FIX engine is sending to Counter Party.

**Question 6. What Happens If Client Connects With Sequence No Higher Than Expected?**

**Answer :**

If Client FIX Engine connects to Broker Fix Engine with Sequence Number higher than expected \(e.g. broker is expecting Sequence Number = 10 and Client is sending = 15\). As per FINANCIAL INFORMATION EXCHANGE \(FIX\)  protocol **Broker will accept the connection and issue a Resend Request \(MsgType=2\) asking Client to resend missing messages \(from messages 10 -15\)** , Now Client can either replay those messages or can issue a **Gap Fill** Message \(MsgType=4 as per FINANCIAL INFORMATION EXCHANGE \(FIX\)  protocol\) in case replaying those messages  doesn't make any sense \(could be admin messages e.g. Heartbeat etc\).

**Question 7. What Do You Mean By Funari Order Type?**

**Answer :**

**Funari** is very popular Order type commonly used in Japanese and Korean market , its denoted by **OrdType=I**  in FIX protocol , In Funari Order type Order will remain in Market as Limit Order but during Market Closing period , if there is any unexecuted quantity then it will turn into a Market Order.

**A** _**Funari order**_ **is a type of smart order that requires any unfilled order quantity be executed as a market order at the close of the trading session.**

**Question 8. What Do You Mean By Odd Lot And Board Lot?**

**Answer :**

A **board lot** is a standardized number of shares defined by a stock exchange as a trading unit. In most cases, this means 100 shares.   
If Clients sends any Order which is **not a multiple of Board lot then its called Odd lot.**

**Question 9. What Happens If Client Connects With Sequence No Lower Than Expected?**

**Answer :**

If Client FIX engine connects to broker FIX engine with Sequence No lower than expected than broker FIX engine will **disconnect** the connection. As per FINANCIAL INFORMATION EXCHANGE \(FIX\) protocol client then may try by i**ncreasing its sequence Number until broker accepts its connection.**

**Question 10. What Is The Difference Between Possdup And Poss Resend?**

**Answer :**

The quick answer is that the FIX engine takes care of setting tag 43 \(without the application's knowledge in most cases\). When you receive a message flagged 43=Y, your \(the receiving side's\) FIX engine will be able to determine if the message was indeed a duplicate because it keeps track of the sequence numbers. PossDup messages have duplicate sequence numbers! The engine can then forward \(or not\) any messages to the application without the application having to know about the PossDupFlag at all.  
  
If the PossResend flag is set in a message, the sequence number of the message is most likely not a duplicate. If you receive such a message, you \(the engine\) should forward it to the application since the engine will not be able to determine whether this information is old in a new envelope - that's up to the application.

**As per FINANCIAL INFORMATION EXCHANGE \(FIX\) protocol PossDupFlag \(tag 43\): indicates possible retransmission of message with this sequence number valid value:**

                                    Y = Possible duplicate

                                    N = Original transmission

**PossResend \(tag 97\)**: Indicates that message may contain information that has been sent under another sequence number.

                                    **Valid Values:**

                                    Y=Possible resend

                                    N=Original transmission

* In Simple terms PossDup is when you are resending a message and PossResend is when you are sending a new message with same data you have already sent in a previous message.
* Let’s consider below use case for clarity with PossDup, you can send out an order with Sequence number of 10. Then you send another message with a number of 11. For some reason the counter party did not receive message 10 and will request a resend. You will then resend this same message with a sequence number of 10 marking tag 43=Y.
* In case of PossResend, you may send out an order, again let's say with a sequence number of 10. After 5 seconds, you have not gotten an acknowledgement. So you may decide to try again because maybe the counterparty didn't receive or got the first time. So you will send out a message with a different sequence number like 11, which will contain all of the same data message number 10 did. You will then mark it as a PossResend. So this is saying you have already sent this order before, which Counter party may or may not have processed. 
* While handling of **PossDup is dictated by session logic, PossResend are dictated by business logic** \(e.g. Reject an Order if ClOrdID is duplicate etc\). This is because it is up to the business layer to determine if a particular business object has been processed \(by checking the order id for instance\).

**Question 11. You Have Bought A Stock At Inr 100 And Want To Sell It As Soon As It Hits Inr 110. If You Want To Guarantee That Your Sell Order Is Filled, Which Of The Following Types Of Order Should You Place?**

**Answer :**

In this case you cannot use limit order because limit order doesn't guarantee execution if there are similar LIMIT order exists then it will wait for its turn. You cannot either use Market Order because it didn't give you Price guarantee and will fill on current price. Solution is to use "STOP" order with stop price 110, as soon as price reaches 110 it will get activate but in case of high volatility it can fill more or less 110 if price is moving very fast.

With a stop order, your trade will be executed only when the security you want to buy or sell reaches a particular price \(the stop price\). Once the stock has reached this price, a stop order essentially becomes a [market order](https://www.investopedia.com/terms/m/marketorder.asp) and is filled. For instance, if you own shares of JC Penney \([JCP](https://www.investopedia.com/markets/stocks/jcp/)\), which currently trades at $5.60, and you place a stop order to sell it at $5.00, your order will only be filled if stock JCP drops below $5.00. Also known as a "[stop-loss order](https://www.investopedia.com/terms/s/stop-lossorder.asp)", this strategy allows you to limit your losses. However, this type of order can also be used to guarantee profits. For example, assume that you bought stock JCP at $4.50 per share and now the stock is trading at $5.60 per share. Placing a stop order at $5.00 will guarantee profits of approximately $0.50 per share, depending on how quickly the market order can be filled.  
  


**Question 12. Which Of The Following Orders Would Be Automatically Canceled If Not Executed Immediately?**

**Answer :**

**Fill or Kill \(FOK\) and Immediate or Cancel \(IOC**\) orders are types of order which either executed immediately or get cancelled by exchange. **TimeInForce** \(tag 59\) in FINANCIAL INFORMATION EXCHANGE \(FIX\) protocol is used to mark an order as FOK or IOC.

**Question 13. What Is The Difference Between Fok Order And Ioc Order?**

**Answer :**

Main difference between FOK and IOC Order is that **FOK demands full execution of order i.e. all quantity has to be filled while IOC order is ready to accept partial fills also**.

**Question 14. What Is Stp \(straight Through Processing\) Systems?**

**Answer :**

STP is abbreviation of "Straight through processing" which denotes trading systems which requires either **no manual interaction** or some manual interaction for whole trade life cycle e.g. everything after submission of Order e.g. processing, execution, booking, allocation, settlement occurs automatically. 

Usually HandlInst \(21\) equals to 1.

Instructions for order handling on Broker trading floor

Valid values:

1 = Automated execution order, private, no Broker intervention

2 = Automated execution order, public, Broker intervention OK

3 = Manual order, best execution



