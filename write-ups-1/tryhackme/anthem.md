# Anthem

This is my write-up for the TryHackMe box known as Anthem located at: [https://tryhackme.com/room/anthem](https://www.tryhackme.com/room/anthem). 

## Website Analysis

The firs thing I did was ran nmap onto the IP address. I did this with the following command:

```c
sudo nmap -Pn -T4 -A -sS -p- <IP Address> -oN output
```

I then got the following as the result:



