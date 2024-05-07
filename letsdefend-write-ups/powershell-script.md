---
description: May 4th, 2024
---

# PowerShell Script

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

Link: [https://app.letsdefend.io/challenge/powershell-script](https://app.letsdefend.io/challenge/powershell-script)

## Summary

In this challenge we are investigating a script encoded in base64. We should decode the script and dissect to unveil its true nature!

## Write Up

Let’s open the script.txt on Desktop.

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

This script is encoded in base64. Base64 is an encoding mechanism for converting binary data to text which can later be transferred over the network for example. Base64 has legitimate use but also it is used by threat actors to obfuscate their code and/or avoid detection. Also, it is worth mentioning that base64 is a type of encoding and should not be mistaken by encrypting the data. Base64 encoded data can be easily decoded.

Firs thing we do is to grab the encoded text and paste it into CyberChef to decode it.

<figure><img src="../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

The output contain null bytes which makes it difficult to read. Use “Remove null bytes” operation to remove them. Before:

<figure><img src="../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

After:

<figure><img src="../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

***

1. What encoding is the malicious script using?

**Answer: Base64**

***

2. What parameter in the PowerShell script prevents the user from closing the process?

This command hides the PowerShell window from the user to ensure they are not aware of the PowerShell command executing on the machine.

**Answer: -W Hidden (-WindowStyle Hidden)**

***

3. What line of code allows the script to interact with websites and retrieve information from them?

PowerShell scripts can be interactive or non-interactive. Non-interactive mode ensures the script runs in the background without the user's knowledge. Since the user has no knowledge that the script is running, they can't close it.

**Answer: -NonI (-NonInteractive)**

***

4. What is the user agent string that is being spoofed in the malicious script?

As defined by MS Docs, the WebClient class provides common methods for sending data to and receiving data from a resource identified by a URI. The very first line of the script creates a new instance of WebClient and stores it in $WC variable.

**Answer: $WC=New-ObjEcT SySTeM.NET.WebCliENt**

***

5. What line of code is used to set the proxy credentials for authentication in the script?

* $wc.Proxy.Credentials - Accesses the credentials property of the WebClient object that was created earlier.
* \[System.Net.CredentialCache] - Per MS Docs, provides storage for multiple credentials. Applications that need to access resources store the credentials to those resources in CredentialCache.
* DefaultNetworkCredentials - Finally, the DefaultNetworkCredential represents the current security context in which the script is running. It could be the currently logged-in user if the script is being run under the currently logged-in user. It might also represent the credential of another user if it is being run under another account.
* :: - This operator is used to access static methods and properties of a .NET type or class. In this case, DefaultNetworkCredential is a method of System.Net.CredentialCache.

**Answer: $wc.PROxY.CrEdenTialS = \[SysTem.NEt.CRedeNTIAlCAcHE]::DeFAULTNetWOrKCredENTiAls**

***

6. When the malicious script is executed, what is the URL that the script contacts to download the malicious payload?

At the very end of the script, we see the DownloadString method called, and the URL provided to it is where we are downloading the payload from.

**Answer: http://98.103.103.170:7443/index.asp**

***

Tips: At first, the script may look difficult to read due to intentional bad indentation, spacing, and case. If you are new to reading scripts and understanding them, sometimes it is helpful to correct the spacing and the letter cases to make them more readable. Of course, this is an optional step and not required!

Before:

<figure><img src="../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

After:

<figure><img src="../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>
