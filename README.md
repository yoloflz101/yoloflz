Denial of Service (DoS) refers to the phenomenon where a server is unable to provide services to legitimate users due to an overwhelming amount of junk or disruptive information sent to it.

Modsecurity (WAF) is an open-source web application firewall that uses CRS rules; When the value of the parameter requesting POST is huge and its content-length value reaches 1 million, it causes a denial of service attack.

For example, if my WAF server has an 8-core CPU and sends any 8 content strength ≥ 1 million requests to the host POST, the CPU utilization of Modsecurity (WAF) will increase to 100%, causing Modsecurity (WAF) to crash. Continuous sending will result in continuous paralysis.

Test and Verify:
set modsecurity.conf "SecRequestBodyNoFilesLimit 1310720"(This is the content-length control parameter.)
Enter name=any string, Until the content length is ≥ 1 million, send a request:
![image](https://github.com/user-attachments/assets/edd16904-f1aa-490d-acb5-466c3ce65c93)
![image](https://github.com/user-attachments/assets/7f601742-0eb6-4933-b8ef-6937ccb45f23)

The request process occupies 100% of a core and can take 5 minutes or more.


When sending 8 requests:

![image](https://github.com/user-attachments/assets/7c29e465-4ccd-4fce-81c6-4d166fba6851)

The CPU utilization rate of 8-core reaches 100%, but due to insufficient resources, other business requests cannot be processed. Causing DOS attacks.

Temporary repair suggestion:

①Close rule 941170, or ②utilize old version 941170, or ③restrict content length(This method has limited effectiveness,because some WEB's content-length requirements exceed 100,000.).
