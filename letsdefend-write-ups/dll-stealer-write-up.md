---
description: May 4th, 2024
---

# DLL Stealer Write-up

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

Link: [https://app.letsdefend.io/challenge/dll-stealer](https://app.letsdefend.io/challenge/dll-stealer)

## Summary

In this challenge, we are tasked with investigating a new DLL stealer malware that has infiltrated our system.

***

## Write Up

#### What is a DLL?

Dynamic Link Libraries (DLLs) are files on Windows systems that contain code. Applications can use the code in DLLs to provide various functions. Many DLLs are used by the OS itself.

#### What is a DLL Stealer?

In this scenario, a legitimate DLL was replaced with a malicious DLL that performs unauthorized actions on the computer.



Let’s start by extracting the challenge file. The challenge file is located at C:\Users\LetsDefend\Desktop\ChallengeFile\sample.zip. The password is infected.

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

***

1. What is the DLL that has the stealer code?

We can use dotPeek to open the sample file and extract its contents. Open dotPeek, go to **`Tools > Show PDB Content > Select the sample file`**.

Right-click on the sample file and select **`Extract Bundle Content to a Folder`**. In that folder, we can see **`Colorful.dll`**, which was embedded in the sample file.

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

**Answer: Colorful.dll**

***

2. What is the anti-analysis method used by the malware?

Malware authors always try to make it difficult for malware analysts to analyze their malware. If we expand the methods used in Colorful, we see IsVirusTotal() : bool method, which is designed to detect if the malware sample is submitted to VirusTotal.

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

**Answer: IsVirusTotal**

***

3. What is the full command used to gather information from the system into the “productkey.txt” file?

If we scroll down a bit in the Colorful() method, we find the following WMIC command. We can also press Ctrl + F and search for productkey.txt.

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

**Answer: wmic path softwareLicensingService get OA3xOriginalProductKey >> productkey.txt**

***

4. What is the full command used to gather information through the "ips.txt" file?

A few lines after the command found for the previous question, we can find the answer to this question. We can also press Ctrl + F and search for ips.txt.

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

**Answer: ipconfig /all >> ips.txt**

***

5. What is the webhook used by the malware?

A webhook is an HTTP callback method used by applications to callback to other applications. In this case, a webhook is used to call back to Discord. Since webhooks use HTTP, we can also press Ctrl + F and search for “http”.

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

**Answer: https://discord.com/api/webhooks/1165744386949271723/kFr6Cc0DSTK1jB8aV3820mBxji06gF2KorUuO2Rd2ckLkhUEHxdi6kv6UHwgJ\_W82fgZ**
