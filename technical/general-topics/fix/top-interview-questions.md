# Top Interview Questions

## [https://javarevisited.blogspot.com/2011/03/top-20-fix-protocol-interview-questions.html](https://javarevisited.blogspot.com/2011/03/top-20-fix-protocol-interview-questions.html)

**1. What is the difference between OrdStatus and ExecType in FIX4.4?**  
OrdStatus \(39\) indicates the current status of the order. ExecType \(150\) was introduced in 4.2 to indicate the type of execution report received. For example, if you replace an order that is already partially filled, the order status is Partially Filled but the ExecType for the replace confirmation is Replaced \(FIX 4.2\).  
  






## [https://www.wisdomjobs.com/e-university/fix-protocol-interview-questions.html](https://www.wisdomjobs.com/e-university/fix-protocol-interview-questions.html)

**Question 1. What Do You Mean By Warrant?**

**Answer :**

Warrant is a financial product which gives right to holder to Buy or Sell underlying financial security, its similar to option with some differences e.g. Warrants are normally issued by banks while options are primarily traded in exchange.

**Question 2. What Is Mean By Settlement Of Securities? When Settlement Does Occur?**

**Answer :**

In Simple term Settlement means money will deducted from buyers and account and security\(Shares\) will be credited to his account , normally Settlement occurs after few days of trade date for most of the exchange its T+3 \(i.e. Three days after trade date\) , T denotes Trade date means the date on which transaction has taken place.

For some of the exchanges e.g. NSE India, SEHK Hongkong its T+2.

**Question 3. What Is Newordersingle, Order Cancel Replace And Order Cancel Reject Message?**

**Answer :**

* These are the basic, most commonly used messages in Electronic trading via FINANCIAL INFORMATION EXCHANGE \(FIX\) protocol.
* NewOrderSingle message is denoted by MsgType=D and its used to place an Order, Order Cancel Replace Request is modification request denoted by MsgType=G in FINANCIAL INFORMATION EXCHANGE \(FIX\) protocol and used to modify Order e.g for changing quantity or price of Order.
* Order Cancel Request is third in this category denoted by MsgType=F in FINANCIAL INFORMATION EXCHANGE \(FIX\) protocol and used to cancel Order placed into Market.

**Question 4. What Are Most Common Issues Encounter When Two Fix Engine Communicates?**

**Answer :**

When Clients connect to broker via FINANCIAL INFORMATION EXCHANGE \(FIX\) protocol, there FIX engine connects to each other, while setting up and during further communication many issues can occur below are some of most common ones:

* Issues related to network connectivity.
* Issues related to Firewall rules.
* Issue related to incorrect host/port name while connecting.
* Incorrect SenderCompID and TargetCompID.
* Sequence Number mismatch.
* Issue related to FINANCIAL INFORMATION EXCHANGE \(FIX\) version mismatch.

**Question 5. What Do You Mean By Incoming Sequence No And Outgoing Sequence No? Which Tag Is Used To Carry Sequence No?**

**Answer :**

Sequence Number is very important concept of FINANCIAL INFORMATION EXCHANGE \(FIX\) protocol which essentially provides it Recovery and replay functionality and ensures that no message will lose during transmission or communication. In FINANCIAL INFORMATION EXCHANGE \(FIX\) protocol every message contains a unique sequence number defined in tag 34. Logically we can divide sequence number into two Incoming and Outgoing Sequence number. 

Incoming sequence number is the number any FIX Engine expecting from Counter Party and Outgoing sequence number is the number any FIX engine is sending to Counter Party.



**Question 6. What Happens If Client Connects With Sequence No Higher Than Expected?**

**Answer :**

If Client FIX Engine connects to Broker Fix Engine with Sequence Number higher than expected \(e.g. broker is expecting Sequence Number = 10 and Client is sending = 15\). As per FINANCIAL INFORMATION EXCHANGE \(FIX\)  protocol Broker will accept the connection and issue a Resend Request \(MsgType=2\) asking Client to resend missing messages \(from messages 10 -15\) , Now Client can either replay those messages or can issue a Gap Fill Message \(MsgType=4 as per FINANCIAL INFORMATION EXCHANGE \(FIX\)  protocol\) in case replaying those messages  doesn't make any sense \(could be admin messages e.g. Heartbeat etc\).

**Question 7. What Do You Mean By Funari Order Type?**

**Answer :**

Funari is very popular Order type commonly used in Japanese and Korean market , its denoted by OrdType=I  in FIX protocol , In Funari Order type Order will remain in Market as Limit Order but during Market Closing period , if there is any unexecuted quantity then it will turn into a Market Order.

**Question 8. What Do You Mean By Odd Lot And Board Lot?**

**Answer :**

In Exchanges every Security traded in lot e.g. lot of 1, 10 or 100 or 1000. These are called Board lots and while placing order clients need to send Order quantity multiple of Board lot. If Clients sends any Order which is not a multiple of Board lot then its called Odd lot.

**Question 9. What Happens If Client Connects With Sequence No Lower Than Expected?**

**Answer :**

If Client FIX engine connects to broker FIX engine with Sequence No lower than expected than broker FIX engine will disconnect the connection. As per FINANCIAL INFORMATION EXCHANGE \(FIX\) protocol client then may try by increasing its sequence Number until broker accepts its connection.

**Question 10. What Is The Difference Between Possdup And Poss Resend?**

**Answer :**

**As per FINANCIAL INFORMATION EXCHANGE \(FIX\) protocol PossDupFlag \(tag 43\): indicates possible retransmission of message with this sequence number valid value:**

                                    Y = Possible duplicate

                                    N = Original transmission

PossResend \(tag 97\): Indicates that message may contain information that has been sent under another sequence number.

                                    **Valid Values:**

                                    Y=Possible resend

                                    N=Original transmission

* In Simple terms PossDup is when you are resending a message and PossResend is when you are sending a new message with same data you have already sent in a previous message.
* Let’s consider below use case for clarity with PossDup, you can send out an order with Sequence number of 10. Then you send another message with a number of 11. For some reason the counter party did not receive message 10 and will request a resend. You will then resend this same message with a sequence number of 10 marking tag 43=Y.
* In case of PossResend, you may send out an order, again let's say with a sequence number of 10. After 5 seconds, you have not gotten an acknowledgement. So you may decide to try again because maybe the counterparty didn't receive or got the first time. So you will send out a message with a different sequence number like 11, which will contain all of the same data message number 10 did. You will then mark it as a PossResend. So this is saying you have already sent this order before, which Counter party may or may not have processed. 
* While handling of PossDup is dictated by session logic, PossResend are dictated by business logic \(e.g. Reject an Order if ClOrdID is duplicate etc\). This is because it is up to the business layer to determine if a particular business object has been processed \(by checking the order id for instance\).

