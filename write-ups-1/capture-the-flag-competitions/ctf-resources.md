# CTF Resources

These are resources such as scripts and softwares I find which assist in CTF Competitions.

## Pwn

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

## PCAP

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

