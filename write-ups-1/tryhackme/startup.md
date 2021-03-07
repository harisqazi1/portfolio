# Startup

To find out what is on the server, we will have to run an nmap scan on the IP address. I will be using the following command:

```csharp
nmap -T4 -A 10.10.201.52 -oN nmap.txt
```

After the scan is done, we can see that port 21 \(usually for FTP\) is open:

```csharp
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| drwxrwxrwx    2 65534    65534        4096 Nov 12 04:53 ftp [NSE: writeable]
| -rw-r--r--    1 0        0          251631 Nov 12 04:02 important.jpg
|_-rw-r--r--    1 0        0             208 Nov 12 04:53 notice.txt
```

These files look interesting, so I will download them. I following [this website](https://stackoverflow.com/questions/113886/how-to-recursively-download-a-folder-via-ftp-on-linux) to find out how to download files from ftp recursively, instead of one-by-one. This is the command I entered:

```csharp
wget -r ftp://anonymous:@10.10.201.52
```

I then had 3 items in my local directory: an image, a text file, and a directory called "ftp". I now have to find any useful information from these files. I noticed that in the text file, called "notice.txt, there is a name being used:

> Whoever is leaving these damn Among Us memes in this share, it IS NOT FUNNY. People downloading documents from our website will think we are a joke! Now I dont know who it is, but Maya is looking pretty sus.

Here the name Maya could be pointing us to a way inside the system, where the username is Maya or maya.

