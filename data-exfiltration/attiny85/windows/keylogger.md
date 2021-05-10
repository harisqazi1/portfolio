# Keylogger

### Windows

The following are code that I have modified from [this website](https://www.andreafortuna.org/2019/05/22/how-a-keylogger-works-a-simple-powershell-example/) to fit my needs. I have made the Powershell script to use the keylogger code, but then send the code to a webhook.site website. This is meant to be used with a rubber ducky that way you can plug it in, and in 10 seconds be out of there with the keylogger up and running. The rubber ducky I will be using is a ATTINY85 Micro-controller. Using the Arduino IDE you can flash the code onto the micro-controller.

Keylogger that sends the text to a web-hook **after** the user has typed 20 characters:

{% file src="../../../.gitbook/assets/text-keylogger.txt.txt" caption="Text-only keylogger" %}

I do not endorse using this illegally or unethically. If you were to use this ethically, along with a **ATTINY85 Rubber Ducky**, this would be the **.ino** code for it:

{% file src="../../../.gitbook/assets/bypass-and-script.txt" caption=".ino file for the ATTINY85 \(BYPASS, EXECUTE, PATCH\)" %}

{% hint style="warning" %}
This ATTINY85 script **ONLY** works for computers with Windows Defender/Security on it. If you want to use it for a computer with Malwarebytes/Kaspersky/Avast, you will have to modify the code yourself.
{% endhint %}

#### Sources I used for research:

* https://www.andreafortuna.org/2019/05/22/how-a-keylogger-works-a-simple-powershell-example/​
* https://docs.microsoft.com/en-us/microsoftteams/platform/webhooks-and-connectors/how-to/connectors-using​
* https://thinkrethink.net/2017/12/14/call-a-webhook-from-inline-powershell/​
* https://stackoverflow.com/questions/2426993/run-a-batch-file-every-x-number-of-seconds-using-powershell​
* https://0x00-0x00.github.io/research/2018/10/28/How-to-bypass-AMSI-and-Execute-ANY-malicious-powershell-code.html​
* https://github.com/yokokho/another-rubber-duck-payloads/blob/master/payload/Turn-Off-UAC-ETC.md​
* https://inc0x0.com/2018/10/budget-usb-rubber-ducky-digispark-attiny85/​
* https://github.com/CedArctic/DigiSpark-Scripts/blob/master/Create\_Account/Create\_Account.ino​
* https://github.com/danielbohannon/Invoke-Obfuscation​
* https://www.netspi.com/blog/technical/network-penetration-testing/15-ways-to-bypass-the-powershell-execution-policy/​

