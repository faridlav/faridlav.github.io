---
description: May 4th, 2024
---

# PCAP Analysis



<figure><img src=".gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

Link: [https://app.letsdefend.io/challenge/pcap-analysis](https://app.letsdefend.io/challenge/pcap-analysis)

## Summary

In this challenge, we are investigating network traffic from a computer. PCAP files include inbound and outbound network traffic captured on a device. PCAP stands for **P**acket **CAP**ture.

***

## Write Up

Let’s start by opening the PCAP file using Wireshark.

<figure><img src=".gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

***

1. In network communication, what are the IP addresses of the sender and receiver?

We have a string value that we can use to find the answer. Open Search in Wireshark, select Packet bytes and String, type in “P13” and click on Find.

<figure><img src=".gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

192.168.235.131 sent a message P13 (192.168.235.137). Since this PCAP file was captured on P13, we should flip the addresses to get them in the right order for the answer.

**Answer: 192.168.235.137,192.168.235.131**

***

2. P13 uploaded a file to the web server. What is the IP address of the server?

We have the IP of P13. Uploading a file to a web server requires the use of HTTP/HTTPS protocol. The HTTP method to upload a file to a webserver is POST. The following query will help us filter down the packets:

ip.src == 192.168.235.137 && http.request.method == POST

<figure><img src=".gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

If we right-click on the first packet and follow the HTTP stream, we can see the upload form:

<figure><img src=".gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

**Answer: 192.168.1.7**

***

3. What is the name of the file that was sent through the network?

The filename is at the beginning of the Follow HTTP Stream window:

<figure><img src=".gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

**Answer: file**

***

4. What is the name of the web server where the file was uploaded?

We can find the answer by looking at the “Server” in the HTTP header. Filter for:

ip.addr == 192.168.1.7 && http

<figure><img src=".gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

**Answer: Apache**

***

5. What directory was the file uploaded to?

The answer can be found in the HTTP response 200 from the server to P13. Use the filter below to find the packet:

ip.src == 192.168.1.7 && http.response.code == 200

Find all the packets with the source IP address of 192.168.1.7 and they must include HTTP response code of 200.

<figure><img src=".gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

**Answer: Uploads**

***

6. How long did it take the sender to send the encrypted file?

Since we have the IP addresses of both the server and P13, we can go to Conversations and look at the Duration of the conversation between them:

<figure><img src=".gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

**Answer: 0.0073**
