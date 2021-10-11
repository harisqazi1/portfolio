# CTF Resources

These are resources such as scripts and software I find which assist in CTF Competitions, usually from write-ups from other players. I will organize it when I have a lot of various topics being covered.

## General:

### Forensics / Steganography

* Exiftool - prints out metadata from an image 
* Exif - prints out metadata as well 
* Hexeditor - you can use this to "repair" an image by editing the file signatures of a file
* Strings -  allows you to see all the letters in a file
* Stegsolve - a steganography tool written in java for finding hidden text in images
  * Can be found at [https://www.aldeid.com/wiki/Stegsolve](https://www.aldeid.com/wiki/Stegsolve)
* Digital Invisible Ink Toolkit - Another tool written in java for finding hidden text in images
  * Can be found at [http://diit.sourceforge.net](http://diit.sourceforge.net)
* Autopsy - digital forensic software for disk imaging
  * Can be found at [https://www.autopsy.com](https://www.autopsy.com)
* Binwalk - you are able to see and extract hidden files in a file
* zsteg - detect stegano-hidden data in PNG & BMP
  * Can be found at [https://github.com/zed-0xff/zsteg](https://github.com/zed-0xff/zsteg)
* Foremost - recover files using the file headers
* jstego - hide and seek secret information to/from JPEG images.
  * Can be found at [https://sourceforge.net/projects/jstego/](https://sourceforge.net/projects/jstego/)
* Sonic Visualizer - audio steganography tool
  * Can be found at [https://www.sonicvisualiser.org/index.html](https://www.sonicvisualiser.org/index.html)
* Audacity - another audio steganography tool
  * Can be found at [https://www.audacityteam.org/](https://www.audacityteam.org)
* Cyberchef - can be used to extract files from another file
  * Can be found at [https://gchq.github.io/CyberChef/#recipe=Extract_Files(true,true,true,true,true,true,false,true)](https://gchq.github.io/CyberChef/#recipe=Extract_Files%28true,true,true,true,true,true,false,true%29)

### Cryptography

* fcrackzip - program to crack zip files using bruteforce or a dictionary
  * Can be found at [https://github.com/hyc/fcrackzip](https://github.com/hyc/fcrackzip)
* Crackstation - website to crack hashes
  * Can be found at [https://crackstation.net/](https://crackstation.net)
* Rainbow tables - website that uses rainbow tables to crack hashes
  * Can be found at [http://rainbowtables.it64.com/](http://rainbowtables.it64.com)
* hashcat - a hash cracking program built into Kali
* John The Ripper - another hash cracking program built into Kali
* CyberChef- Basic hash decoding 
  * [https://gchq.github.io/CyberChef/](https://gchq.github.io/CyberChef/)

### Web Exploitation

* Nmap - used to find what ports are open on a server
* Burpsuite - used to intercept/modify web requests
* Curl - a tool for data transfering and reception
* Dirbuster - a directory bruteforcing with a wordlist
* Gobuster - another directory bruteforcing program using a wordlist
* Nikto - a web server scanner
* Wget - used to download a website and its contents

### Reverse Engineering

* ltrace - simple command for viewing what is going on in the background of a binary file
* IDA Freeware - A debugger which reveals all the behind the scenes for a binary file
* Ghidra - a debugger, which attempts to reconstruct code from where the binary was composed of

## Specific:

### Pwntools (respond to server):

I learned about this script from [https://github.com/W3rni0/DownUnderCTF\_2021#general-skills-quiz](https://github.com/W3rni0/DownUnderCTF\_2021#general-skills-quiz), and this was a solution to a CTF question where a server gave you questions and you had to answer them. The questions were asking you to encode or decode a string, and you had to do it in under 30 seconds to get the flag. I have copied the script here:

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

### Bluetooth PCAP

This is a script I got from [https://github.com/apoirrier/CTFs-writeups/blob/master/PBCTF2021/Misc/BTLE.md](https://github.com/apoirrier/CTFs-writeups/blob/master/PBCTF2021/Misc/BTLE.md), which is used to decode Bluetooth Data from  a PCAP file.

```python
from scapy.all import *

current = []
for packet in rdpcap('btle.pcap'):
    if "Prepare Write Response" in packet:       
        offset = packet["Prepare Write Response"].offset
        data = bytes(packet["Prepare Write Response"].data)
        if offset + len(data) > len(current):
            current = current + ([0] * (offset + len(data) - len(current)))
        for i in range(offset, offset + len(data)):
            current[i] = data[i - offset]
print(current)
print("".join([chr(x) for x in current]))
```

### Radio Frequency Decoding

I learned about the website [http://uz7.ho.ua/packetradio.htm](http://uz7.ho.ua/packetradio.htm), through a write-up for a CTF Competition: [https://www.sohio.net/markdown.php?pbctf21\_isthatyourpacket.md](https://www.sohio.net/markdown.php?pbctf21\_isthatyourpacket.md). The software allows you to decode **Winlink** data.

### Keystroke decoding from audio file

I was reading a write-up for how to decode keystrokes from an audio file from [https://github.com/apoirrier/CTFs-writeups/blob/master/PBCTF2021/Misc/GhostWriter.md](https://github.com/apoirrier/CTFs-writeups/blob/master/PBCTF2021/Misc/GhostWriter.md). They point out a GitHub page [https://github.com/shoyo/acoustic-keylogger](https://github.com/shoyo/acoustic-keylogger). The code below is from the CTF write-up and not from the keylogger GitHub page itself:

```
//DOCKER:
//Create docker with installed libraries
docker exec -it acoustic-keylogger_env_1 apt update
docker exec -it acoustic-keylogger_env_1 apt install libsndfile1-dev
//get token for accessing the notebook
docker exec -it acoustic-keylogger_env_1 jupyter notebook list
```

```
//PYTHON in Docker
//import libraries
from acoustic_keylogger.audio_processing import *
from acoustic_keylogger.unsupervised import *
from sklearn.preprocessing import MinMaxScaler

data = wav_read("output.wav") //input file here

keystrokes = detect_keystrokes(data, threshold=100) //have to play around with this
//normalize the features
X = [extract_features(x) for x in keystrokes]
X_norm = MinMaxScaler().fit_transform(X)
//differentiate keystrokes
len(set([x[0] for x in X_norm])) //gives you a number of different keystrokes
//group similar keystrokes
letters = {}
phrase = []
current_letter = ord('a')
for x in X_norm:
    if x[0] not in letters:
        letters[x[0]] = current_letter
        current_letter += 1
    phrase.append(letters[x[0]])
print("".join([chr(x) for x in phrase]).replace("d", " "))
//NOTE: Output could be encrypted with Monoalphabetic Substitution or another cipher
```
