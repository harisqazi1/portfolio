# Website Pop-up

#### This script pops up a chosen website onto the computer.

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
    DigiKeyboard.sendKeyStroke(KEY_SPACE, MOD_CMD_LEFT); //Command + Space
    DigiKeyboard.delay(500); //5 milisecond delay
    DigiKeyboard.print("Terminal"); //Opens the Terminal
    DigiKeyboard.sendKeyStroke(KEY_ENTER); 
    DigiKeyboard.delay(500);
    DigiKeyboard.print("open https://harisqazi1.gitbook.io/blog/"); //Enter your own website here
    DigiKeyboard.sendKeyStroke(KEY_ENTER);
    hack = false; //Changes the boolean to false
  }
}
```

### Notes

This code is used to pop open a website of any sort to the computer, as long as there is an internet connection. You can also use it to access your own server, thus getting the IP and maybe even fingerprint the computer. 

