---
description: Jan 4th, 2025
icon: robot
---

# DanaBot Lab

<figure><img src="../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

Link: [https://cyberdefenders.org/blueteam-ctf-challenges/danabot/](https://cyberdefenders.org/blueteam-ctf-challenges/danabot/)

## Summary

The SOC team has detected suspicious activity in the network traffic, revealing that a machine has been compromised. Sensitive company information has been stolen. Your task is to use Network Capture (PCAP) files and Threat Intelligence to investigate the incident and determine how the breach occurred.

## Write Up

Download the lab files and extract them. The password to access the files is `cyberdefenders.org`.

Before filtering for traffic in a PCAP file using Wireshark, I recommend examining the **Protocol Hierarchy** and **Conversations** views. This provides an overview of key areas of interest. For example, if there's significant SMB traffic, focus on it when filtering packets.

> **Note:** Open the PCAP file in an isolated environment. Avoid using your work or production machines.

***

1. What is the name of the malicious file used for initial access?

In the **Protocol Hierarchy** window, we observe both SMB and HTTP traffic. Since SMB traffic appears minimal, start with HTTP. Use the filter:

```
http
```

<figure><img src="../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

Three files are visible in this traffic. Start by following the "login.php" traffic at packet 6. The answer is:

<figure><img src="../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>

***

2. What is the SHA-256 hash of the malicious file used for initial access?

To extract the malicious file:

* Go to `File > Export Objects > HTTP`.
* Select the `login.php` file and click **Save**.

<figure><img src="../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

In Linux, use the following command to generate the SHA-256 hash:

<figure><img src="../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

***

3. Which process was used to execute the malicious file?

PCAP traffic and Wireshark won't reveal the process used. Open `allegato_708.js` in a text editor. The file appears obfuscated, but you can still identify the process without deobfuscation. At the bottom of the file, a line creates a new `ActiveXObject`, revealing that `Wscript.exe` executed the file.

<figure><img src="../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>

***

4. What is the file extension of the second malicious file utilized by the attacker?

Filter HTTP traffic again. This reveals that a DLL file was downloaded.

<figure><img src="../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>

***

5. What is the MD5 hash of the second malicious file?

Export the DLL file as done in Q2. Then, generate its MD5 hash in Linux using:

<figure><img src="../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>

***

6. Which IP address was used by the attacker during the initial access?

Finding the attacker's IP can be challenging due to numerous addresses in the PCAP file. A good starting point is identifying **TCP three-way handshake** packets. This involves:

```
tcp.flags.syn == 1
```

This reduces the displayed packets to 115. Narrow it down using the workstation's IP (`10.2.14.101`):

```
tcp.flags.syn == 1 and ip.src == 10.2.14.101
```

This reduces traffic to 63 packets. Locate the handshake after the download of `resources.dll` (packet 9952). The TCP handshake begins at packet 9966.

<figure><img src="../.gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>

We can see the address of the server in the screenshot above.

***

7. What is the final malicious IP address, found in the PCAP, that is used as the command and control (C2) server by DanaBot?

The failed connection attempt to the attacker’s IP was observed at packet 9966. Afterward, a successful handshake occurs at packet 10000. To identify this:

{% code overflow="wrap" %}
```
tcp.flags.syn == 1 or (tcp.seq == 1 and tcp.ack == 1 and tcp.len == 0 and tcp.analysis.initial_rtt)
```
{% endcode %}

This isolates completed TCP handshakes.

* `tcp.flags.syn == 1`: Finds initiating SYN packets.
* `tcp.seq == 1`: Matches relative sequence numbers after the SYN packet.
* `tcp.len == 0`: Ensures no payload (typical of handshake packets).
* `tcp.analysis.initial_rtt`: Confirms Wireshark observed the three-way handshake.

The IP of the C2 server is found in the screenshot:

> The SYN-ACK packet from the server also has a sequence number of 0. It is important to note that packets originating from the client maintain a separate sequence number from those originating from the server.

<figure><img src="../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>
