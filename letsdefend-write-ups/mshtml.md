---
description: May 4th, 2024
---

# MSHTML

<figure><img src="../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

Link: [https://app.letsdefend.io/challenge/mshtml](https://app.letsdefend.io/challenge/mshtml)

## Summary

Investigate the following files in the lab environment and answer the questions.&#x20;

## Write Up

1. Examine the Employees\_Contact\_Audit\_Oct\_2021.docx file, what is the malicious IP in the docx file?

We can use oleid Employees\_Contact\_Audit\_Oct\_2021.docx command to see if there is any external relationships found in this file:

<figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

Next command would be using oleobj Employees\_Contact\_Audit\_Oct\_2021.docx command to see the contents of external relationship:

<figure><img src="../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

Answer: 175.24.190.249

Note: Since OLETools were not install on the machine, you should use sudo -H python3 setup.py install.

***

2. Examine the Employee\_W2\_Form.docx file, what is the malicious domain in the docx file?

Similar to the previous question, first we use oleid Employee\_W2\_Form.docx command to confirm there are external relationships, and then we run oleobj Employee\_W2\_Form.docx command to unveil the external domain:

<figure><img src="../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

Answer: arsenal.30cm.tw

***

3. Examine the Work\_From\_Home\_Survey.doc file, what is the malicious domain in the doc file?

Similar to the previous question, we run oleid Work\_From\_Home\_Survey.doc command to confirm external relationships, and then run oleobj Work\_From\_Home\_Home\_Survey.doc command to display the domain name:

<figure><img src="../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

Answer: trendparlye.com

***

4. Examine the income\_tax\_and\_benefit\_return\_2021.docx, what is the malicious domain in the docx file?

Exact same steps as the previous questions:

<figure><img src="../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

Answer: hidusi.com

***

5. What is the vulnerability the above files exploited?

The very last line on the screenshot from the previous question shows the CVE vulnerability.

<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

Answer: CVE-2021-40444
