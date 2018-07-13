# Top Interview Questions

## [https://javarevisited.blogspot.com/2011/03/top-20-fix-protocol-interview-questions.html](https://javarevisited.blogspot.com/2011/03/top-20-fix-protocol-interview-questions.html)

**1. What is the difference between OrdStatus and ExecType in FIX4.4?**  
OrdStatus \(39\) indicates the current status of the order. ExecType \(150\) was introduced in 4.2 to indicate the type of execution report received. For example, if you replace an order that is already partially filled, the order status is Partially Filled but the ExecType for the replace confirmation is Replaced \(FIX 4.2\).  
  


