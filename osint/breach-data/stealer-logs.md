# Stealer Logs

{% hint style="info" %}
I will be using CrowdStrike's definition of OSINT: "While most open source data is accessed via the open internet and may be indexed with the help of a search engine like Google, it can also be accessed via more closed forums that are not indexed by search engines. Though most deep web content is inaccessible to general users because it lives behind a paywall or requires a login to access, it is still considered part of the public domain" \[4].
{% endhint %}

## What are stealer logs?

"Information stealer (infostealer) malware—malicious software designed to steal victim information, including passwords—has become one of the most discussed malware types on the cybercriminal underground in 2022" \[1]. Stealer logs are the byproduct of this strain of malware. After the data is exfiltrated out of a victims device(s), they can be reused in other attacks against the victim (logging into a bank account, etc.). If not, then this data is offered for sale on dark web marketplaces. There are currently some popular infostealer malware out there, such as Redline, Vidar, and Raccoon. As more and more infostealer malware are deployed, the more stealer logs are created. This leads to more information being offered for sale. Similar to data breaches, after some time, these are available for free. From my research in stealer logs, they are mostly available through dark-web marketplaces and through Telegram channels. They are now also popping up on surface web breach forums as well. The next portion of this blog will dive into what types of information are in these logs, and what that can mean from an OSINT data source perspective.

## OSINT Data Source

{% hint style="warning" %}
I will be looking into a sample of stealer logs from the Redline infostealer. The following information does not define all the information in stealer logs, only the information found in the sample I had access to. Each infostealer and sample is different, but have similar qualities.&#x20;
{% endhint %}

I wanted to look into stealer logs to see how it compared to traditional data breaches. I was able to obtain a sample of Redline stealer logs. There were 2000+ files in this sample, so I will describe the types of information in these logs. Just for clarification, this sample did not have one users' information, but is a mix of a lot of people's information. With that being said, let me jump right into the data types.&#x20;

{% hint style="info" %}
I will not discuss all data types, only the logs I saw that were more prevalent and important.
{% endhint %}

### Applications

One thing I noticed was that there were file names that indicated where the data was taken from, for example: `Thunderbird_xxxxxxxx.default-release.txt`, `Vivaldi_Default.txt`, and `Firefox_xxxxxxxx.default.txt`. These have cookies and metadata relating to that respective application. The following is a snippet from a random chosen Firefox log file (redacted cookies and session tokens):

<figure><img src="../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

There is a bit of information we can gather from this log. The `.ir`  ccTLD belongs to the country of Iran, so we can assume that some of these logs potentially belong to someone from Iran (no proof of this other than ccTLDs). One of the redacted information in the image above was a Json Web Token. "JWTs are credentials, which can grant access to resources" \[2]. These are noticeable by the `eyJ` prefix. I put the JWT onto [https://jwt.io/](https://jwt.io/), and it was able to tell me the initialization vector, value, and mac for this token:

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Theoretically, I should be able to use that token and access resources the victim had access to, but that is beyond this blog and **illegal**. If you are interested in what a threat actor can do with this information, I recommend checking out the following: [https://developer.okta.com/blog/2018/06/20/what-happens-if-your-jwt-is-stolen#what-happens-if-your-json-web-token-is-stolen](https://developer.okta.com/blog/2018/06/20/what-happens-if-your-jwt-is-stolen#what-happens-if-your-json-web-token-is-stolen).

### Crypto

There were multiple wallet files in this log collection. Here is the redacted output of one of them to show the types of information:

<figure><img src="../../.gitbook/assets/image (752).png" alt=""><figcaption></figcaption></figure>

I personally do not know too much about Cryptocurrency, so I reached out to Brandon O'Shea who I know works on crypto projects. He mentioned the following: "Assuming they are real and accurate, a threat actor would gain complete control of the funds and could immediately liquidate the balance of the wallets the phrases are for". This put into perspective for me of what this log and others like it really meant. If the infostealer malware is deployed on your system, all the money in your crypto wallet can be depleted and transferred out. One point to add here was that if the aforementioned wallet information was legitimate, the threat actor releasing the logs would most likely have cleaned those out before releasing the logs to the public.

### Valve Data File

In the sample, I noticed a lot of `.vdf` files. I was unsure what these were, so I looked them up. These are plaintext files that are used by the Valve Corporation for graphic settings and configurations for games \[3]. You can see information like the following about gyroscopic data:

<figure><img src="../../.gitbook/assets/image (753).png" alt=""><figcaption></figcaption></figure>

The Steam game-pad binding configuration was also in the logs:

<figure><img src="../../.gitbook/assets/image (755).png" alt=""><figcaption></figcaption></figure>

You are also also able to see more personal information as well, such as Steam account name:

<figure><img src="../../.gitbook/assets/image (754).png" alt=""><figcaption></figcaption></figure>

By this information, I can tell who got hit by the malware, by tracking the username of the Steam account. There are multiple Steam account names in this log sample, which enforces the idea of this sample stemming from multiple individuals.

### Device Information

There was a file in the sample that contained device information, down to the uuid of the user. The file is too large in image height to show all of it, but here is a snippet of it (please forgive the Kali Linux background logo):

<figure><img src="../../.gitbook/assets/image (756).png" alt=""><figcaption></figcaption></figure>

This was not as important as the previous data type, but it still provides more context on not only the victims, but also the context of stealer logs. Based on my research, this information helps the threat actor in two-fold: it provides them with one more log of information, but more importantly, it helps them decide if the device is infected already or not, as they use this information for fingerprinting.

### Auto-fill Information

There is a lot of auto-fill information obtained in this sample. This includes the following type of information:

```
account[email]
account[first_name]
account[last_name]
address
address1
address-1-city
address-1-state
address-1-street_address
address2
addressLine1
addressLine2
address_postalCode
addressSub
addressSugg
address-ui-widgets-enterAddressCity
address-ui-widgets-enterAddressLine1
address-ui-widgets-enterAddressLine2
address-ui-widgets-enterAddressPostalCode
admin_email
billing_address_1
billing_email
billing_first_name
billing_last_name
bill_to_address_city
bill_to_address_line1
bill_to_address_postal_code
bill_to_address_state
bill_to_email
citizen_address
create_order_form[contact_info][address][city]
create_order_form[contact_info][address][line1]
create_order_form[contact_info][address][postal_code]
create_order_form[contact_info][first_name]
create_order_form[contact_info][last_name]
data[User][email]
data[UsersProfile][first_name]
data[UsersProfile][last_name]
dob
dob-day
dob_day
dob-month
dob-year
dob_year
email
email-1
email-address
email_address
emailAddress
email_address[email]
emailAgain
email_confirm
EmailForm[email]
emailLogin
email_or_username
first_name
firstName
input_billing_address_city
input_billing_address_one
input_billing_address_zip
input_billing_phone_number
input_recipient_email
last_name
lastName
log-in-email
login_email
phone_number
pin
register_email
securityCode
security_question_1
security_question_2
security_question_3
security_question_4
security_question_5
sign-in-email
signup[email]
SignupForm[email]
ssn_last_4
```

My assumption is that the aforementioned data are from the users' browsers' cache, which will later be used to auto-fill the data.&#x20;

### Passwords

There was a file in this sample that just had combinations of usernames, passwords, and the URL those credentials would work on. I have not tested the credentials, as that would be beyond OSINT, and getting into **unlawful activity**. With that being said, the file had rows and rows of what looked like the following:

<figure><img src="../../.gitbook/assets/image (757).png" alt=""><figcaption></figcaption></figure>

If multi-factor authentication is not enabled on these accounts, anyone with the credentials can log in. This is especially significant for banking-related apps.

## Conclusion&#x20;

There are a couple other types of files as well in the logs, but I wanted to look into the data that would be the fruitful from an OSINT investigation purpose. I wanted this blog to be partially CTI-adjacent as well, to show what information script kiddies and threat actors have after successfully deploying an information stealer malware on a system. Breaches and stealer logs each have their own benefits and downsides, but this was a great learning experience for me overall in the stealer log space.

## Recommendations&#x20;

I wanted to provide some recommendations for those trying to prevent being in these logs, based on my knowledge of CTI.&#x20;

* **Do not click on unknown links or download unknown files**: One of the biggest vectors for malware deployment is users clicking on phishing links and/or downloading phishing attachments. If attachments or links **need** to be clicked on, use services like VirusTotal to look into the item before you proceed further. If an item is too good to be true...it probably is.
* **Do not use auto-fill**: Auto-fill has gained popularity by allowing ease to users by caching their credentials and information for using on websites. These caches are usually stored locally on the machine. If an infostealer malware was deployed on a machine with information stored in these caches, it will copy that data and provide it to the threat actor.&#x20;

Those are just a couple of suggestions I had off the top of my head. Will these suggestions prevent all malware deployment? Absolutely not. However, this will lessen the amount of threats by a good amount.&#x20;

## Sources

1. [https://www.accenture.com/us-en/blogs/security/information-stealer-malware-on-dark-web](https://www.accenture.com/us-en/blogs/security/information-stealer-malware-on-dark-web)
2. [https://jwt.io/](https://jwt.io/)
3. [https://techstk.com/what-is-a-vdf-valve-data-file-and-how-do-i-open-it/](https://techstk.com/what-is-a-vdf-valve-data-file-and-how-do-i-open-it/)
4. [https://www.crowdstrike.com/cybersecurity-101/osint-open-source-intelligence/](https://www.crowdstrike.com/cybersecurity-101/osint-open-source-intelligence/)
