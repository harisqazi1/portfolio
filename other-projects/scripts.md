# Scripts

This is a location where I will display scripts I work on, as well as scripts I learn about from other CTF players, for future usage.

### Wordlist Generator (v1):

```python
#/usr/bin/env python3
from itertools import product
import sys
import argparse

parser=argparse.ArgumentParser(
    description='''This is a wordlist generator based on a word of your choice. You can add any ASCII character based on pre-defined lists.''',
    epilog="""Example: 'wordlist.py hello 3 4' => [hello<<<, hello<<!, hello?@#, etc.]""")
parser.add_argument("<word>", help="the word you will base the wordlist on",
                    type=str)
parser.add_argument("<size>", help="how many extra characters do you want?",
                    type=int)                
parser.add_argument("<choice>", help="do you want characters to be numbers [1], lowercase letters [2], uppercase letters [3], special characters [4], or all of the above [5]",
                    type=int)
args = parser.parse_args()

OUTPUT_FILE_NAME="output.txt" #not needed
numbers=list("1234567890")
lowercase =list("abcdefghijklmnopqrstuvwxyz")
uppercase=list("ABCDEFGHIJKLMNOPQRSTUVWXYZ")
special=list("<>?!@#$%^&*()")
all=list("1234567890abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ<>?!@#$%^&*()")

if len (sys.argv) != 4 :
    print("Only 3 arguments (ex. hello 3 1)")
    sys.exit (1)

word = sys.argv[1]
size = int(sys.argv[2])
choice_input = int(sys.argv[3])

if choice_input == 1:
    charlist = numbers
    for c in product(charlist, repeat=size):
        print(word + ''.join(c))
elif choice_input == 2:
    charlist = lowercase
    for c in product(charlist, repeat=size):
        print(word + ''.join(c))
elif choice_input == 3:
    charlist = uppercase
    for c in product(charlist, repeat=size):
        print(word + ''.join(c))
elif choice_input == 4:
    charlist = special
    for c in product(charlist, repeat=size):
        print(word + ''.join(c))
else:
    charlist = all
    for c in product(charlist, repeat=size):
        print(word + ''.join(c))
```

This script creates word-lists based on a word you give it, and the selections you make. It is meant to be a simpler **crunch**, in a way. Simply stated, you choose a word ("test"), choose the amount of extra characters you want (3), and finally choose the character list you want (4 = special characters). This leads you to a 7 character list, with the first 4 letters being **test** and the other three characters being iterated through by the code. This code is meant to creating custom wordlists for cracking Wi-Fi passwords (based on a bit of knowledge) , or passwords in general.

### ELF Brute-forcing:

```python
// Some code#!/usr/bin/env python3
from pwn import *
from itertools import product

uppercase=list("ABCDEFGHIJKLMNOPQRSTUVWXYZ")
numbers=list("1234567890")

binary_file = './rock' #the binary I will be brute-forcing
context(arch='amd64') # the architecture you are running the binary on


charlist = uppercase #use uppercase as the list for the characters
for c in product(charlist, repeat=4): #iterates from AAAA to ZZZZ
    x=999 				#for going from 999-9999
    while (x != 9999):
        elf = ELF(binary_file) #runs the binary every loop 
        p = process(binary_file) #makes the process every loop
        print(p.recvuntil(b'\n')) # gets the output until a new line
        string = "SKY-" + ''.join(c) + "-{}".format(x) # formats the string in a SKY-ABCD-1234 format 
        print("Testing out " + string) #output for user to see what is being tested
        p.sendline(string) #sends the string to the binary
        print(p.recvall()) # gets all of the output from the binary to check if the string is correct
        x = x+1 #goes to next number
```

This script was something I wrote for the National Cyber League. It was meant to brute-force the values from **SKY-AAAA-0000** all the way until **SKY-ZZZZ-9999**. The goal was for it to eventually hit the flag and give the output from the binary that the string was correct. The code did work, but it would take too long to brute-force. However, I still do believe that this code will come in handy later on for another CTF, so this is why I have it saved here.
