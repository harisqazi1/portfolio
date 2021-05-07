# Keylogger

### Windows

The following are code that I have modified from [this website](https://www.andreafortuna.org/2019/05/22/how-a-keylogger-works-a-simple-powershell-example/) to fit my needs. I have made the Powershell script to use the keylogger code, but then send the code to a webhook.site website. This is meant to be used with a rubber ducky that way you can plug it in, and in 10 seconds be out of there with the keylogger up and running. The rubber ducky I will be using is a ATTINY85 Micro-controller. Using the Arduino IDE you can flash the code onto the micro-controller.

Keylogger that uploads a file to a web-hook:

{% file src="../.gitbook/assets/text-keylogger.txt.txt" caption="Keylogger file" %}

The aforementioned file **DOES NOT** bypass AMSI \(Microsoft Security\). I do not endore using this illegally or unethically. If you were to use this ethically, along with a **ATTINY85 Rubber Ducky**, this would be the **.ino** code for it:

{% file src="../.gitbook/assets/rubber-ducky-script.txt" caption="Rubber Ducky Script \(Not COMPLETE\)" %}

{% hint style="warning" %}
I have not found a way to bypass AMSI, if you have, you would be able to run this code and place a keylogger on a device. HOWEVER, I was able to run it on my own device, and I have Malwarebytes constantly running. Not sure if there is a connection there.
{% endhint %}

