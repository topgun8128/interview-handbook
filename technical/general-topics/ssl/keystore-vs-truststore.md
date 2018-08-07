# Keystore vs Truststore

{% embed data="{\"url\":\"http://www.java67.com/2012/12/difference-between-truststore-vs.html\",\"type\":\"link\",\"title\":\"Difference between trustStore vs keyStore in Java SSL\",\"description\":\"Main difference between trustStore vs keyStore is that trustStore  \(as name suggest\) is used to store certificates from trusted Certificat...\",\"icon\":{\"type\":\"icon\",\"url\":\"http://www.java67.com/favicon.ico\",\"aspectRatio\":0},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://2.bp.blogspot.com/-DDI-dnz9Tlw/UCpMkPdTsII/AAAAAAAAAdc/6p0zCWN0TGA/w1200-h630-p-k-no-nu/7.jpg\",\"width\":244,\"height\":128,\"aspectRatio\":0.5245901639344263}}" %}



Let's see difference between truststore vs keystore in point format which is much clear and easy to understand :  
1\) Keystore is used to store your credential \(server or client\) while truststore is used to store others credential \(Certificates from CA\).  
2\) Keystore is needed when you are setting up server side on SSL, it is used to store server's identity certificate, which server will present to a client on the connection while trust store setup on client side must contain to make the connection work. If you browser to connect to any website over SSL it verifies certificate presented by server against its truststore.  
3\) Though I omitted this on the last section to reduce confusion but you can have both keystore and truststore on client and server side if the client also needs to authenticate itself on the server. In this case, client will store its private key and identify certificate on keystore and server will authenticate the client against certificate stored on server's trust store.  
4\) In Java -javax.net.ssl.keyStore property is used to specify keystore while -javax.net.ssl.trustStore is used to specify trustStore.  
5\) In Java, one file can represent both keystore vs truststore but it's better to separate private and public credential both for security and maintenance reason.  
6\) When you install [JDK or JRE](http://javarevisited.blogspot.sg/2011/12/jre-jvm-jdk-jit-in-java-programming.html) on your machine, Java comes with its own truststore \(collection of certificate from well known CA like Verisign, goDaddy, thwarte etc. you can find this file inside  
JAVA\_HOME/JRE/Security/cacerts where [JAVA\_HOME](http://javarevisited.blogspot.sg/2012/02/how-to-set-javahome-environment-in.html) is your JDK Installation directory.  
7\) [keytool  command](http://java67.blogspot.sg/2012/09/keytool-command-examples-java-add-view-certificate-ssl.html) \(binary comes with JDK installation inside JAVA\_HOME/bin\) can be used to create and view both keyStore and trustStore.  
If you are still not clear with what is truststore and keystore in Java or difference between keystore and truststore than just remember one line keystore is used to store server's own certificate while truststore is used to store the certificate of other parties issued by CA like Verisign or goDaday or even self-signed certificates.  
  
Read more: [http://www.java67.com/2012/12/difference-between-truststore-vs.html\#ixzz5NSPgxm](http://www.java67.com/2012/12/difference-between-truststore-vs.html#ixzz5NSPgxmRH)

