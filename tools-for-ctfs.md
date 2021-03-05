# Tools for CTFs

{% hint style="info" %}
A lot of the tools I use are built into the Kali Linux operating system. Most of these will be Linux commands.
{% endhint %}

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
  * Can be found at [https://www.audacityteam.org/](https://www.audacityteam.org/)
* Cyberchef - can be used to extract files from another file
  * Can be found at [https://gchq.github.io/CyberChef/\#recipe=Extract\_Files\(true,true,true,true,true,true,false,true\)](https://gchq.github.io/CyberChef/#recipe=Extract_Files%28true,true,true,true,true,true,false,true%29)

### Cryptography

* fcrackzip - program to crack zip files using bruteforce or a dictionary
  * Can be found at [https://github.com/hyc/fcrackzip](https://github.com/hyc/fcrackzip)
* Crackstation - website to crack hashes
  * Can be found at [https://crackstation.net/](https://crackstation.net/)
* Rainbow tables - website that uses rainbow tables to crack hashes
  * Can be found at [http://rainbowtables.it64.com/](http://rainbowtables.it64.com/)
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



