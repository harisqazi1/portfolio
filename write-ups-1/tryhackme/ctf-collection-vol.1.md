# CTF collection Vol.1

#### This is my write-up for the TryHackMe room located at: [https://tryhackme.com/room/ctfcollectionvol1](https://tryhackme.com/room/ctfcollectionvol1)

### Task 2: What does the base said?

The question asks us to decode a code: VEhNe2p1NTdfZDNjMGQzXzdoM19iNDUzfQ==. The command I ran was: 

```text
echo "VEhNe2p1NTdfZDNjMGQzXzdoM19iNDUzfQ==" | base64 -d
```

This decodes the string in the quotes using base64 encoding. After the command, I got the flag:

![](../../.gitbook/assets/screenshot-2021-03-10-170140.png)

### Task 3: Meta Meta



