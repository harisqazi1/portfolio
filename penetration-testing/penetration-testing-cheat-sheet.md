# Penetration Testing Cheat Sheet

## Reconnaissance

```bash
# Nmap TCP 
sudo nmap -sV -sC -O -n -p- -oA nmapscan 192.168.0.1/24
# -sV - Version detection
# -sC - equivalent to --script=default
# -O  - Enable OS detection
# -p- - Scan all ports 
# -oA - Output in the three major formats at once (normal, XML, Grepable)

# Nmap UDP
sudo nmap -Pn -n -vvv -p- -sU -oA nmapscan 192.168.0.1/24
# -Pn - Treat all hosts as online -- skip host discovery
# -n  - Never do DNS resolution/Always resolve [default: sometimes]
# -vv - Increase verbosity level
# -p- - Scan all ports
# -sU - UDP scan
# -oA - Output in the three major formats at once (normal, XML, Grepable)
```

{% embed url="https://github.com/codingo/Reconnoitre" %}

{% embed url="https://github.com/Tib3rius/AutoRecon" %}

## Hash Cracking / Password Attacks

### Online Cracking

{% embed url="https://crackstation.net/" %}

{% embed url="https://hashes.com" %}

{% embed url="https://www.cmd5.org/" %}

{% embed url="https://www.onlinehashcrack.com/" %}

{% embed url="https://hashmob.net/" %}

{% embed url="https://github.com/HashPals" %}

### Offline Cracking

{% embed url="https://hashcat.net/hashcat/" %}

{% embed url="https://www.openwall.com/john/" %}

{% embed url="https://github.com/openwall/johnny" %}

{% embed url="https://github.com/Ciphey/Ciphey" %}

Ciphey can also be run in Docker: `docker run -it --rm remnux/ciphey -t "Encrypted input"` (Normal way) or `docker run -it --rm remnux/ciphey -f encrypted.txt` (File Input)

### Hash Identification

{% embed url="https://hashcat.net/wiki/doku.php?id=example_hashes" %}

{% embed url="https://hashes.com/en/tools/hash_identifier" %}

{% embed url="https://www.kali.org/tools/hashid/" %}

{% embed url="https://www.kali.org/tools/hash-identifier/" %}

{% embed url="https://github.com/HashPals/Name-That-Hash" %}

### Dictionary

{% embed url="https://weakpass.com/" %}

{% embed url="https://github.com/ohmybahgosh/RockYou2021.txt" %}

{% embed url="https://github.com/danielmiessler/SecLists/tree/master/Passwords" %}

{% embed url="https://github.com/berzerk0/Probable-Wordlists/tree/master/Real-Passwords" %}

{% embed url="https://github.com/ignis-sec/Pwdb-Public/tree/master/wordlists" %}

{% embed url="https://labs.nettitude.com/tools/rocktastic/" %}

### Rules

{% embed url="https://github.com/NotSoSecure/password_cracking_rules" %}

{% embed url="https://github.com/stealthsploit/OneRuleToRuleThemStill" %}

{% embed url="https://github.com/praetorian-inc/Hob0Rules" %}

{% embed url="https://github.com/n0kovo/hashcat-rules-collection" %}

{% embed url="https://github.com/kaonashi-passwords/Kaonashi" %}

### Purple Rain Attack

I had never heard of this before until I found it on [https://github.com/J3rryBl4nks/PasswordCrackingMethodology](https://github.com/J3rryBl4nks/PasswordCrackingMethodology). I will post the link, but also the main command, in case the page ever gets taken down. Link: [https://www.netmux.com/blog/purple-rain-attack](https://www.netmux.com/blog/purple-rain-attack).

<pre class="language-bash"><code class="lang-bash">shuf dict.txt | pp64.bin --pw-min=8 | hashcat -a 0 -m MODE_HERE -w 4 -O hashes.txt -g 300000
<strong># shuf - shuffle our dictionary before piping into PRINCEprocessor
</strong># dict.txt - Targeted or General Purpose dictionary of your choosing
# pp64.bin - PRINCEprocessor utility from Hashcat
# --pw-min=8 - tells PRINCE to generate minimum length of 8+ character passwords
# hashcat -a 0 - starts Hashcat in Straight mode in order to take stdin input
# -m MODE_HERE - specify hash mode number, for instance -m 1000 is NTLM
# -w 4 - tells Hashcat to use highest workload setting
# hashes.txt - your file containing hashes to crack
# -g 300000 - tells Hashcat to generate 300,000 random rules
</code></pre>

### Word-list Generation

{% embed url="https://www.kitploit.com/2022/08/awesome-password-cracking-curated-list.html" %}
