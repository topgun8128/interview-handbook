# Apply SSL Steps



### First Time Apply New Certificate



1. Generating Key Pairs `keytool -genkeypair -keystore hk-uat-rmbi-kaazing1.hk.hsbc.jks -keypass changeit -keysize 2048 -storepass changeit -alias hk-uat-rmbi-kaazing1.hk.hsbc -dname "CN=hk-uat-rmbi-kaazing1.hk.hsbc, OU=Global Banking and Markets, O=HSBC Bank PLC" -keyalg RSA -validity 3650`
2. Generating Cert Request `keytool -keystore hk-uat-rmbi-kaazing1.hk.hsbc.jks -keypass changeit -storepass changeit -certreq -alias hk-uat-rmbi-kaazing1.hk.hsbc > hk-uat-rmbi-kaazing1.hk.hsbc.csr.txt`
3. Raise GSR BS - Encryption Key Requests - SSL Certificates - Certificate Request - Internal / Certificate Request - External
4. Merge GSR Replied Cert with ROOT CA Cert `cat "hk-uat-rmbi-kaazing1.hk.hsbc.HBAP.21Jan2016.cer" hsbcissuingca02.crt hsbcrootca.crt > hk-uat-rmbi-kaazing1.signed.cert`
5. Create the Keystore from origin `cp hk-uat-rmbi-kaazing1.hk.hsbc.jks hk-uat-rmbi-kaazing1-keystore.jks`
6. Import Cert into Keystore`keytool -importcert -rfc -keystore hk-uat-rmbi-kaazing1-keystore.jks -alias hk-uat-rmbi-kaazing1.hk.hsbc -storetype JCEKS -storepass changeit -file hk-uat-rmbi-kaazing1.signed.cert`
7. Create the Truststore from origin `cp truststore.db hk-uat-rmbi-kaazing1-truststore.db`
8. Import Cert into Truststore `keytool -importcert -keystore hk-uat-rmbi-kaazing1-truststore.db -storepass changeit -trustcacerts -alias hk-uat-rmbi-kaazing1.hk.hsbc -file hk-uat-rmbi-kaazing1.signed.cert`
9. Create a Keystore password file `touch hk-uat-rmbi-kaazing1-keystore.pw insert the file with content "changeit"`
10. Modify the gateway-config.xml to apply the changes.



### Renew Certificate

1. Raise GSR

   Business Systems - Encryption Key Requests - SSL Certificates - Certificate Request - Internal

1\)Fill the original CSR text and upload the original CSR file   
2\)Select "renew" as the request type   
3\)Fill the serial number according to the email which informs you the cert will be expired   
4\)For other fields, fill the same as the new cert request

1. You will receive the re-newed cer file by email, for example the file name is "hk-uat-rmbi-kaazing1.hk.hsbc.cer"
2. Download "SHA2 HSBC Root CA Certificate"\(Root.cer\) and "SHA2 HSBC Issuing Certificate"\(Int.cer\) from [http://teams.europe.hsbc/risk/isr/EncryptionKeyManagement/SitePages/SSL Certificate Support.aspx](http://teams.europe.hsbc/risk/isr/EncryptionKeyManagement/SitePages/SSL%20Certificate%20Support.aspx)

if the link is invalid, then just use the Root.cer and Int.cer in conf folder

3. Backup existing files 

`mv hk-prod-rmbi-kaazing1.signed.cert hk-prod-rmbi-kaazing1.signed.cert.bak.[date]   
mv hk-prod-rmbi-kaazing1-keystore.jks hk-prod-rmbi-kaazing1-keystore.jks.bak.[date]   
mv hk-prod-rmbi-kaazing1-truststore.db hk-prod-rmbi-kaazing1-truststore.db.bak.[date]`

4. Merge GSR Replied Cert with ROOT CA Cert `rm hk-prod-rmbi-kaazing1.signed.cert cat "hk-prod-rmbi-kaazing1.hk.hsbc.cer" Int.cer Root.cer > hk-prod-rmbi-kaazing1.signed.cert`

5. Create the Keystore from origin `rm hk-prod-rmbi-kaazing1-keystore.jks cp hk-prod-rmbi-kaazing1.hk.hsbc.jks hk-prod-rmbi-kaazing1-keystore.jks`

6. Import Cert into Keystore `keytool -importcert -rfc -keystore hk-prod-rmbi-kaazing1-keystore.jks -alias hk-prod-rmbi-kaazing1.hk.hsbc -storetype JCEKS -storepass gf1x2015 -file hk-prod-rmbi-kaazing1.signed.cert`

7. Import Cert into Truststore 

`rm hk-prod-rmbi-kaazing1-truststore.db   
keytool -importcert -keystore hk-prod-rmbi-kaazing1-truststore.db -storepass gf1x2015 -trustcacerts -alias hk-prod-rmbi-kaazing1.hk.hsbc -file hk-prod-rmbi-kaazing1.signed.cert`

8. There should be no change in gateway-config.xml if the keystore, truststore and password file names are the same.

