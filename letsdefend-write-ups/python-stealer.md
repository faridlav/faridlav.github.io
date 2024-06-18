---
description: May 5th, 2024
---

# Python Stealer

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Link: [https://app.letsdefend.io/challenge/python-stealer](https://app.letsdefend.io/challenge/python-stealer)

## Summary

This challenge involves identifying whether a Python script is a malicious data stealer.

## Write Up

1. What is the function that is used to decrypt the master key?

In Python, functions are declared with the `def` keyword. The second function in the script is `def decrypt(buff, master_key):`. This function decrypts data when called by the script. Here's a breakdown:

* `try:` - This keyword instructs Python to attempt the code inside the "try" block. If an error occurs, it moves to the "except" block. Otherwise, it proceeds with the code.&#x20;
* `return AES.new(CryptUnprotectData(master_key, None, None, None, 0)[1], AES.MODE_GCM, buff[3:15]).decrypt(buff[15:])[:-16].decode()` -  This line invokes the `AES.new` function to create a new AES cipher and returns it. Inside `AES.new`, `CryptUnprotectData` is called, responsible for decrypting the data.&#x20;
* `except:`  - If an error occurs within the `try` block, the code within the `except` block is executed.&#x20;
* `return "Error"` - This returns the string value `"Error"` to the function caller.

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

**Answer: CryptUnprotectData**

***

2. What is the mode that is used by the AES algorithm?

The answer to this question is in the same line of code as the previous one. The encryption mode is specified in the `AES.new()` function call. It utilizes the Galois/Counter Mode (GCM) for symmetric-key block ciphers.

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

**Answer: GCM**

***

3. What is the name of the author of the code?

This question requires a quick scan of the code. Look for keywords like "author," "made," "written," etc. Also, pay attention to string literal values enclosed in double quotes.

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

**Answer: Astraa**

***

4. What is the UserAgent in the code?

Similar to the previous question, scan the code and focus on string literal values enclosed in double quotes. Also, search for relevant keywords such as "User," "Agent," "Mozilla," "Chrome," and "Gecko."

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

**Answer: Mozilla/5.0 (X11; Linux x86\_64) AppleWebKit/537.11 (KHTML, like Gecko) Chrome/23.0.1271.64 Safari/537.11**

***

5. What is the hooked URL?

A webhook, or hooked URL, is utilized in the script to execute an HTTP callback to another application. The answer can be found in the `url` variable within the **get\_webhook\_url()** function.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

**Answer: https://fhrkvjbdhfdobdkjvh.netlify.app**

***

6. What is the name of the function that is used to collect data from the browser?

Once more, the easiest way to find the answer is by swiftly scanning through the code and examining string literal values. Notably, `the get_token()` function contains the names of popular browsers and their corresponding disk paths.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

**Answer: get\_token**

***

7. What is the ID paragraph element that allows an attacker to parse HTML?

The answer can be found in the `get_webhook_url()` function. This script utilizes the BeautifulSoup library, commonly used for web scraping purposes.&#x20;

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

**Answer: djhjvsdbk**

***

8. What is the function name that is used to retrieve IP address?

Search for "ip" to locate the answer.

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

**Answer: getip**

***

9. What is the regex that is used to dump tokens?

Regex (regular expression) is a sequence of strings that defines a specific pattern, enabling the search for particular pieces of information within strings. For example, regex patterns can be crafted to identify phone numbers or email addresses. Here are some indicators of regex patterns:

* Enclosed in quotes (single or double) as they are string values.
* Prefixed with 'r' to signify a raw string in Python.
* Involves numerous special characters like \ \* . $

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

**Answer: dQw4w9WgXcQ:\[^.**_**\['(.**_**)'].**_**$]\[^"]**_

***

10. What is the avatar\_url ?

Search for "avatar" in the code or examine the URLs within the code.

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

**Answer: https://cdn.discordapp.com/attachments/826581697436581919/982374264604864572/atio.jpg**
