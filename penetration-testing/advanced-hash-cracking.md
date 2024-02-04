# Advanced Hash Cracking

{% hint style="info" %}
I mention commands using hashcat in this post. John the Ripper does have [rule](https://github.com/openwall/john/blob/bleeding-jumbo/doc/RULES) and [mask](https://github.com/openwall/john/blob/bleeding-jumbo/doc/MASK) attacks as well. Their syntax might be a bit different, but the idea behind these are the same as hashcat. JtR even has the ["Purple Rain Attack"](https://github.com/openwall/john/blob/bleeding-jumbo/doc/PRINCE) mentioned below.
{% endhint %}

I have been doing hash cracking for a couple of years now, and have heard the terms rules and masks thrown around here and there. This blog post will be my journey through learning what rules and masks are, as well as showing some use cases that might not be apparent at first glance.&#x20;

## Rules

Rule-based attacks are considered a programming language in the sense that they notify the cracking software of what to do once the software receives a string. hashcat's Wiki has some great examples of this:

<table><thead><tr><th width="156">Name</th><th width="97">Function</th><th>Description</th><th width="129">Example Rule</th><th width="124">Input Word</th><th>Output Word</th></tr></thead><tbody><tr><td>Nothing</td><td>:</td><td>Do nothing (pass-through)</td><td>:</td><td>p@ssW0rd</td><td>p@ssW0rd</td></tr><tr><td>Lowercase</td><td>l</td><td>Lowercase all letters</td><td>l</td><td>p@ssW0rd</td><td>p@ssw0rd</td></tr><tr><td>Uppercase</td><td>u</td><td>Uppercase all letters</td><td>u</td><td>p@ssW0rd</td><td>P@SSW0RD</td></tr><tr><td>Capitalize</td><td>c</td><td>Capitalize the first letter and lower the rest</td><td>c</td><td>p@ssW0rd</td><td>P@ssw0rd</td></tr></tbody></table>

These are a couple of examples I had copied over from [https://hashcat.net/wiki/doku.php?id=rule\_based\_attack](https://hashcat.net/wiki/doku.php?id=rule\_based\_attack). Using the above rows as examples, we can create a file that has one rule for the lowercase letters:

```python
# Lowercase rule
l
```

That's it. We have created one rule that will lowercase all of the strings we pass through to the cracking software. We can test it by creating a MD5 hash for the word "testing123". We can then create a dictionary list with the following strings in it:

```
Hello
This
Testing123
Hashcat
Cracking
```

If our rule works as it should, it will lowercase "Testing123" to "testing123", and crack our hash for us. We can use hashcat with the following command to get this output: `hashcat -m 0 hash -r rule_created.rule hash.dict`.

<figure><img src="../.gitbook/assets/image (747).png" alt=""><figcaption></figcaption></figure>

We have now created one rule. Rule files usually have a rule on each line and can look like the following (excerpt taken from [https://github.com/hashcat/hashcat/blob/master/rules/best66.rule](https://github.com/hashcat/hashcat/blob/master/rules/best66.rule)):

```
r
$0
$1
$2
$e
D2 D2
+5 ] } } } } '4
```

I would encourage you to check out the [aforementioned hashcat Wiki documentation](https://hashcat.net/wiki/doku.php?id=rule\_based\_attack). They give a great breakdown of what each of the values mean. There are some rule files that are well known in the industry, such as: [OneRuleToRuleThemAll](https://github.com/NotSoSecure/password\_cracking\_rules), [OneRuleToRuleThemStill](https://github.com/stealthsploit/OneRuleToRuleThemStill), and [Hob0Rules](https://github.com/praetorian-inc/Hob0Rules). They are created based on statistics and industry standards for passwords. NotSoSecure has some great rule comparison charts on their website: [https://notsosecure.com/one-rule-to-rule-them-all](https://notsosecure.com/one-rule-to-rule-them-all).&#x20;

## Mask

Mask attacks work by trying all combinations in a specified key-space. I do want to specify that mask attacks do use brute-force, but just in a more "smarter" manner. I'll quote the example hashcat uses on their [site](https://hashcat.net/wiki/doku.php?id=mask\_attack) to describe this, as I believe it is pretty well written. I have modified the following to be more concise:

> Here is a single example. We want to crack the password: _Julia1984_
>
> In traditional Brute-Force attack we require a charset that contains all upper-case letters, all lower-case letters and all digits (aka “mixalpha-numeric”). The Password length is 9, so we have to iterate through 62^9 (13.537.086.546.263.552) combinations. Lets say we crack with a rate of 100M/s, this requires more than **4 years** to complete.
>
> In Mask attack we know about humans and how they design passwords. The above password matches a simple but common pattern. A name and year appended to it. We can also configure the attack to try the upper-case letters only on the first position ... To make it short, with Mask attack we can reduce the keyspace to 52\*26\*26\*26\*26\*10\*10\*10\*10 (237.627.520.000) combinations. With the same cracking rate of 100M/s, this requires just **40 minutes** to complete.

You can think of this as permutation with repetition as well. You have to get the order of each position correctly to crack the hash. However, since we know the types of values used (lowercase char, uppercase char, integer, special character, etc.), we can then reduce the amount of brute-forcing we will have to do. hashcat has a list of built-in charsets for mask attacks:

```
?l = abcdefghijklmnopqrstuvwxyz
?u = ABCDEFGHIJKLMNOPQRSTUVWXYZ
?d = 0123456789
?h = 0123456789abcdef
?H = 0123456789ABCDEF
?s = «space»!"#$%&'()*+,-./:;<=>?@[\]^_`{|}~
?a = ?l?u?d?s
?b = 0x00 - 0xff
```

If we know a password is 8 characters long, and we know the password is all lowercase alphabets, we can set a mask to account for that exactly that. Let use the password "password" as an example:

`hashcat -m 0 -a 3 5f4dcc3b5aa765d61d8327deb882cf99 ?l?l?l?l?l?l?l?l`

The `-m 0` means that we are cracking an MD5 hash. The `-a 3` means that we are using the brute-force attack. `5f4dcc3b5aa765d61d8327deb882cf99` is the MD5 hash for "password".  `?l?l?l?l?l?l?l?l` is the mask.&#x20;

<figure><img src="../.gitbook/assets/image (748).png" alt=""><figcaption></figcaption></figure>

During my research, I did see a repository of masks that are claimed to be used in corporate environments: [https://github.com/golem445/Corporate\_Masks](https://github.com/golem445/Corporate\_Masks). I have not used it myself, but it does seem to be promising.

## Rule vs. Mask

While rules modify a complete string (by either appending to it or modifying it completely), a mask modifies each part of a string. In addition, a mask allows you to specify which types of values are at which location. There are times where a mask can be used to crack the same hash as a rule, but they each have different end goals. Based on my research, mask attacks do not use a wordlist, as the mask is the wordlist per-se.&#x20;

## Hybrid

While doing research for this blog, I also discovered an attack that caught my attention. This is an attack that combines a dictionary on one side, and a brute-force value on the other. The brute-force can be done with masks or rules, according to the hashcat Wiki. This would be attack mode 6 or 7 (depending on position of wordlist and mask) from the hashcat arguments:

<figure><img src="../.gitbook/assets/image (749).png" alt=""><figcaption></figcaption></figure>

Lets go with the example of "testing123" again. For this we create a .dict file to specify that this will be our dictionary/wordlist to be used for this attack. I will add "testing" to this file. Since we know the password has three numbers at the end, we will use the mask of \`?d?d?d\`. The full command would be the following:`hashcat -a 6 -m 0 hash hash.dict ?d?d?d`

<figure><img src="../.gitbook/assets/image (750).png" alt=""><figcaption></figcaption></figure>

The perk of using the Hybrid mode for me is saving the time I would need to use cewl or crunch to generate multiple wordlists and then run each wordlist through hashcat.

## Purple Rain (princeprocessor)

I had not heard about this attack until I saw it referenced on [J3rryBl4nks' PasswordCrackingMethodology repo](https://github.com/J3rryBl4nks/PasswordCrackingMethodology). This can be considered as an advanced Combinator attack. The description provided by [netmux](https://www.netmux.com/blog/purple-rain-attack) explains it really well:

> It's gonna give you password patterns and rule sets that you'd NEVER come up with on your own. It is very useful at chaining together usable patterns and passwords, and when left on it's own, supplied with a simple dictionary, could easily crack 75% of the LinkedIn dataset in 24hrs. Are ALL the patterns and passwords going to be useful? No. Are you going to run this attack many times and NOT crack the critical hashes you need? Yes. BUT one day, at some point, this attack will run and these magical cracked hashes will start slowly trickling down from the password gods filling your terminal.

If you want to read more about it, I would recommend checking out [https://reusablesec.blogspot.com/2014/12/tool-deep-dive-prince.html](https://reusablesec.blogspot.com/2014/12/tool-deep-dive-prince.html) as well.

I liked the idea behind this attack because it does two things: assists with removing bias and "randomizes" the potential password generation. This definitely has the "so crazy that it might just work" feel to it. The following three items are needed for this attack:

* The hashcat binary
* [princeprocessor Hashcat Utility](https://github.com/hashcat/princeprocessor) (does not seem to be default in Kali installs)
* List of dictionaries
  * J3rryBl4nks mentions they use the hashkiller-dict.txt wordlist from [https://hashkiller.io/download](https://hashkiller.io/download)

We can then use the following command:

```bash
shuf dict.txt | pp64.bin --pw-min=8 | hashcat -a 0 -m MODE_HERE -w 4 -O hashes.txt -g 300000
# shuf - shuffle our dictionary before piping into PRINCEprocessor
# dict.txt - Targeted or General Purpose dictionary of your choosing
# pp64.bin - PRINCEprocessor utility from Hashcat
# --pw-min=8 - tells PRINCE to generate minimum length of 8+ character passwords
# hashcat -a 0 - starts Hashcat in Straight mode in order to take stdin input
# -m MODE_HERE - specify hash mode number, for instance -m 1000 is NTLM
# -w 4 - tells Hashcat to use highest workload setting
# hashes.txt - your file containing hashes to crack
# -g 300000 - tells Hashcat to generate 300,000 random rules
```

{% hint style="warning" %}
Based on [Matt Weir's research](https://reusablesec.blogspot.com/2014/12/tool-deep-dive-prince.html), it seems that dictionary attacks perform much better than the princeprocessor attack over their cracking sessions. However, since princeprocessor generates its rules automatically on the go, it keeps on running even after a rule set is completed. Based on my own research, this attack seems to be a last resort for cracking hashes, since you can run it and "forget about it". As pen-tests have deadlines, I would recommend running this as soon as you can, while still running the standard dictionary attacks separately (if you have the bandwidth).&#x20;
{% endhint %}

## Alternative Dictionaries

When it comes to password cracking wordlists, the defaults that people go to are rockyou.txt and any of the [SecLists](https://github.com/danielmiessler/SecLists/tree/master/Passwords) wordlists. There were times I had participated in Capture The Flag events where rockyou.txt or some of the SecLists wordlists did not crack the hash. This was without any rules or masks attached. For those instances, I relied on other dictionaries. I have listed a couple others below:

{% embed url="https://weakpass.com/" %}

{% embed url="https://github.com/berzerk0/Probable-Wordlists/tree/master/Real-Passwords" %}

There were also times where I had created my own dictionaries based on the scenario. For those, I ended up using [crunch](https://www.kali.org/tools/crunch/), [Mentalist](https://github.com/sc0tfree/mentalist), or [my own wordlist generator](https://www.harisqazi.com/scripts/wordlist-generator).

## Password Analysis

Password analysis can be used to create rules and masks that might be more specific to an organization or an individual. People are known to re-use passwords for multiple accounts, so analyzing that data can be a great source to work off of for us. Analysis can be done manually or automatically. I personally have done it manually by using other breaches a person has been in to assume what passwords they might use next. Using that assumption, you can create a possible password wordlist, and then add a rule or mask to that to crack the new hash(es). I have found some automated tools that might assist with password analysis as well:

{% embed url="https://github.com/shmuelamar/cracken" %}

{% embed url="https://github.com/digininja/pipal" %}

## Use Cases

There are two main use cases that comes to mind when it comes to using advanced hash cracking strategies. From a pen-testing perspective, it gives you a way to break free from the dictionary itself and be able to manipulate it the way you want. Using masks, or even tools such as cewl and crunch, you can create your own wordlists and are able to do more. If we are given a corporate password policy, we can use that to remove all passwords from rockyou.txt that would not be possible with the policy (ex. passwords that are too short). Using something like the "Purple Rain" attack, we can then have an automated way of creating rules, thus giving us an opportunity to crack unique password formats.

From an OSINT perspective, you can use hashes cracked from other breaches to assume passwords for other platforms. If you are trying to crack a hash of a whale (high profile executive), you can use items related to them (kids, parents, pets, etc.) to create possible passwords. In addition to that, you can use platforms such as DeHashed and HaveIBeenPwned to see what breaches their personal email was part of. Then cracking those hashes we can assume what their password can be for this platform as well. There have been times where a password has told me more than I needed about a person. First, it told me if they might have other email accounts as well (since you can see other accounts in a breach with the same hash/password). It also told me keywords that might mean something to the user. Those keywords can be pivot points themselves or reemphasize other pivot points you had previously discovered. The goal is not to log into other platforms using a subject's/target's credentials, **that is NOT OSINT and is illegal**. Using the information in just the password to create pivot points is OSINT.

Are these use cases really niche. Yes, yes they are. They are also realistic and cases I have encountered myself or personally know others who have.

## Sources

* [https://hashcat.net/wiki/doku.php?id=rule\_based\_attack](https://hashcat.net/wiki/doku.php?id=rule\_based\_attack)
* [https://www.4armed.com/blog/hashcat-rule-based-attack/](https://www.4armed.com/blog/hashcat-rule-based-attack/)
* [https://www.kitploit.com/2022/08/awesome-password-cracking-curated-list.html](https://www.kitploit.com/2022/08/awesome-password-cracking-curated-list.html)
* [https://hashcat.net/wiki/doku.php?id=mask\_attack](https://hashcat.net/wiki/doku.php?id=mask\_attack)
* [https://hashcat.net/wiki/doku.php?id=hybrid\_attack](https://hashcat.net/wiki/doku.php?id=hybrid\_attack)
* [https://github.com/J3rryBl4nks/PasswordCrackingMethodology](https://github.com/J3rryBl4nks/PasswordCrackingMethodology)
* [https://www.netmux.com/blog/purple-rain-attack](https://www.netmux.com/blog/purple-rain-attack)
* [https://reusablesec.blogspot.com/2014/12/tool-deep-dive-prince.html](https://reusablesec.blogspot.com/2014/12/tool-deep-dive-prince.html)
