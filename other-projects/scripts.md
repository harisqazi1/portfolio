# Scripts

This is a location where I will display scripts I work on, as well as scripts I learn about from other CTF players, for future usage.

### Wordlist Generator \(v1\):

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

This script creates word-lists based on a word you give it, and the selections you make. It is meant to be a simpler **crunch**, in a way. Simply stated, you choose a word \("test"\), choose the amount of extra characters you want \(3\), and finally choose the character list you want \(4 = special characters\). This leads you to a 7 character list, with the first 4 letters being **test** and the other three characters being iterated through by the code. This code is meant to creating custom wordlists for cracking Wi-Fi passwords \(based on a bit of knowledge\) , or passwords in general.

### Pwntools \(respond to server\):

I learned about this script from [https://github.com/W3rni0/DownUnderCTF\_2021\#general-skills-quiz](https://github.com/W3rni0/DownUnderCTF_2021#general-skills-quiz), and this was a solution to a CTF question where a server gave you questions and you had to answer them. The questions were asking you to encode or decode a string, and you had to do it in under 30 seconds to get the flag. I have copied the script here:

```python
from pwn import *
from urllib.parse import unquote
from base64 import b64decode, b64encode
from codecs import encode,decode

s = remote('hostname',  port)

# Ready to start
s.sendline()
# 1+1=?
s.recvuntil(': ')
s.sendline('2')
# Decode an hex string to decimal
s.recvuntil(': ')
s.sendline(str(int(s.recvline(),16)))
# Decode an hex string to ASCII letter
s.recvuntil(': ')
s.sendline(str(chr(int(s.recvline(),16))))
# Decode a URL encoded string
s.recvuntil(': ')
s.sendline(unquote(s.recvline(keepends=False).decode()))
# Base64 decode
s.recvuntil(': ')
s.sendline(b64decode(s.recvline()))
# Base64 encode
s.recvuntil(': ')
s.sendline(b64encode(s.recvline(keepends=False)))
# ROT13 decode
s.recvuntil(': ')
s.sendline(decode(s.recvline(keepends=False).decode(), 'rot_13'))
# ROT13 encode
s.recvuntil(': ')
s.sendline(encode(s.recvline(keepends=False).decode(), 'rot_13'))
# Binary decode
s.recvuntil(': ')
s.sendline(str(int(s.recvline(),2)))
# Binary encode
s.recvuntil(': ')
s.sendline(bin(int(s.recvline())))
# Best CTF competition
s.recvuntil('?')
# s.sendline('picoCTF')
s.sendline('DUCTF')
s.interactive()
```

