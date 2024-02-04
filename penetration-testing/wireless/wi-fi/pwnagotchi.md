# Pwnagotchi

In order to setup your Pwnagotchi, read [https://pwnagotchi.ai/installation/](https://pwnagotchi.ai/installation/). I have this on a Pi Zero W, and just have that connected to a Power Brick with a MicroUSB charger. This page is meant for what I do **after** the Pwnagotchi is set up.

### Accessing Handshakes

The handshakes are located in `/root/handshakes/`. The way I go about accessing the files is by SSH-ing into the machine as root. I then run `cp -r /root/handshakes/ /home/pi/handshakes/`. This makes a copy of the handshakes folder in the pi home directory. I then use [FileZilla](https://filezilla-project.org/), to then log in to the pi user and download the files (from port 22).

### Handshake to Hash file

Once you have the handshakes, they will all be in .pcap format. In order to convert all of them at once to hashcat-readable hashes, I ran the following command:

`hcxpcapngtool -o hash.hc22000 -E wordlist.txt *`

The hcxpcapngtool command is part of the hcxtools install (apt install hcxtools). The `-o` is for where the output to be PMKID/EAPOL hash file should go. The `-E` is to output the ESSIDs as a potential word-list for cracking later. This part is not necessary, but helps if you want to add those just as a precaution. The `*` is a wildcard which will convert all of the pcaps in the current directory.

### Cracking

For cracking I run the following hashcat command:

`hashcat -m 22000 <hash_file> <wordlist>`

The `-m 22000` specifies the hash to be WPA-PBKDF2-PMKID+EAPOL format. I usually use rockyou.txt as the default wordlist.

In order to read the cracked passwords you can run the following:

`hashcat -m 22000 <hash_file> <wordlist> --show`

OR you can read the potfile by running `cat hashcat.potfile` where you have hashcat downloaded.

More info can be found: [https://hashcat.net/wiki/doku.php?id=cracking\_wpawpa2](https://hashcat.net/wiki/doku.php?id=cracking\_wpawpa2).
