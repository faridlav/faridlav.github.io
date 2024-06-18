# 🛡️ Event Horizon

Track: Intro to Blue Team

Link: [https://app.hackthebox.com/challenges/158](https://app.hackthebox.com/challenges/158)

## Summary

Our CEO's computer was compromised in a phishing attack. The attackers took care to clear the PowerShell logs, so we don't know what they executed. Can you help us?

## Write Up

This challenge gives us 324 EVTX files. Certainly, it is not feasible to open each EVTX file and go through every event log to find a trace of a PowerShell command. However, it is possible to open EVTX files related to the task and scan through the event logs for any clues about PowerShell. For example, `Microsoft-Windows-DiskDiagnosticDataCollector%40Operational.evtx` is probably not helpful to us, but "Microsoft-Windows-PowerShell%40Operational.evtx" could be.

Another approach for this challenge would be using PowerShell itself to search through the EVTX files for clues. We can search for `.ps1` or `PowerShell`. Here is the command:

{% code overflow="wrap" lineNumbers="true" %}
```
$files = 'Event Horizon\Logs'
foreach ($file in $files) {
    Get-WinEvent -Path $file -ErrorAction SilentlyContinue
    | Where-Object {
        $_.Message -like "*.ps1*"
        | Format-List TimeCreated, Message
        | Out-Host -Paging
    }
}
```
{% endcode %}

Here is the breakdown of the command:

* `$files = Get-ChildItem -Path 'Event Horizon\Logs' -Filter *.evtx`: Store all the EVTX files from the 'Event Horizon\Logs' folder in an array.
* `foreach ($file in $files)`: Iterate over every file ($file) in the `$files` array.
* `Get-WinEvent -Path $file.FullName -ErrorAction SilentlyContinue`: Load all the event logs from the current EVTX file (specified by `$file.FullName`). '-ErrorAction SilentlyContinue' suppresses any errors to avoid clogging the screen with error text.
* `Where-Object { $_.Message -like "*.ps1*" }`: This cmdlet is used to filter event logs that contain ".ps1" in the Message part.
* `Format-List TimeCreated, Message`: Print the time the event was logged and the full message.
* `Out-Host -Paging`: Prints the message gradually in case the output is long.
* `|` is called a pipe. It pipes the output of one cmdlet to the next cmdlet.

After running the command above, a suspicious PowerShell script shows up in the very beginning:

<figure><img src="../../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

The script name is encoded in base64. Here is how to decode it in PowerShell:

{% code overflow="wrap" lineNumbers="true" %}
```
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String('SFRCezhMdTNfNzM0bV9GMHIzdjNSfSAg'))
```
{% endcode %}

<figure><img src="../../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

I also noticed that the flag is visible in plaintext in the `Microsoft-Windows-PowerShell%40Operational.evtx` file.

<figure><img src="../../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

You can also use the PowerShell command earlier to search for `HTB{`.

Answer: `HTB{8Lu3_734m_F0r3v3R}`
