# Breach Data Infrastructure

This blog is going to be a discussion on considerations for building a system for breached data. I will be referring to breach data, but this includes the following: breach data, ransomware data, leaks, and stealer logs. In my opinion, creating an infrastructure is a way to be proactive about breach data, which will in turn lead us to have the data we need at hand when an investigation comes along. I will be recommending tools that can assist with each part of what I consider to be the breach data life cycle. **Linux will be the primary operating system that will be discussed here. However, most tools should work on Windows and macOS as well. I do not discuss macOS at all here.**

{% hint style="warning" %}
I do not thoroughly vet/screen any of the software I am mentioning here. They are meant to be examples of tools you can use to accomplish a certain task. Use your own due diligence when using these programs.&#x20;
{% endhint %}

{% hint style="info" %}
Although, I mention a lot of tools here, the main point that I want to focus on is the infrastructure itself. Tools will come and go, but the topic of infrastructure will be much more sturdy and grounded. TLDR: Techniques > Tools
{% endhint %}

## Hardware

The type of hardware needed for a breach data infrastructure depends on your needs and your end goal. I have heard Michael Bazzell mention [on his podcast](https://inteltechniques.com/blog/2023/04/28/the-privacy-security-osint-show-episode-295/) that he has a 25 TB server for  breach data. This is definitely overkill for myself and most people, unless you have a business revolving around protecting clients' information/credentials. Although my recommendation for others would be to virtualize, I myself have mostly just downloaded breach data on my main machine and have played around with it there. My PC would be considered mid-tier, so it can handle some of the more RAM or CPU-intensive requests. Your mileage may vary. Here are some questions that will let you drill down on the hardware you are looking for:

* How much breach data do you plan on consuming?
* Do you want to keep backups?
* Do you prefer local or remote storage?
* Do you need your data to be portable?
* Do you plan on indexing the data?

These are just a couple of questions that can send you in the direction of what sort of hardware you are looking for. If you are just using breach data for events like TraceLabs or an OSINT CTF, then maybe your own PC is better suited for this. There is no "one size fits all" solution when it comes to this. The hardware can range from: microSD -> Hard Drive -> SSD -> NAS -> PC -> Server. You could realistically have a whole OS on a spare SSD and have your breach data be on there. If you do end up going for a separate PC route for this, I would recommend using something like [https://pcpartpicker.com/products/](https://pcpartpicker.com/products/) to get an estimate of the cost of the machine. Before purchasing a system for breach data ask yourself: Do you really need a whole system for breach data?

## Operating System

Windows seems to be the go-to OS for most people nowadays. Out of the box it provides a really easy to use experience for those who might not be technically inclined. The downside to Windows (in terms of breach data) is the limited ability to play around with data. If you look at Debian-based Linux distributions, you will see programs at the command line (cat, awk, sed, etc.) that let you play around with data in the stream. This makes it easy to find what you are looking for and modify the data as needed. You can definitely download programs for Windows to play around with data, but in my experience, Linux just has more benefits out of the box from a breach data perspective.

## Indexing

If you want to have results come back in seconds from queries into your data, I would recommend looking into data indexing. I have written a [post on breach data indexing](https://www.harisqazi.com/osint/breach-data/breach-data-parsing), where I go into setting up a couple of search engines just for breach data. Elasticsearch ended up being my favorite and the best choice for me. There is a big downside to indexing, and that is the amount of storage it takes. 9.4 GB of raw data ended up being 65.1 GB of indexed data when I was using the Elasticsearch. That is about 7x the size of the raw data. However, I was searching through hundreds of millions of lines of breach data and getting results in around 1 second. Before indexing the data, you have to format the data manually before passing it onto the search engine. Sometimes, you can just send the whole breach data to the search engine for processing, as-is. Other times, you have to spend time cleaning up each file to make sure it is appropriate to be read by the search engine. This is another downside of indexing.&#x20;

If you do not want a full-on search engine, but want to try this out at a small scale, check out [qgrep](https://github.com/zeux/qgrep). If you want a simple search engine, I would recommend looking into [Typesense](https://typesense.org/) and [Meilisearch](https://www.meilisearch.com/). I have not tried [Manticore Search](https://manticoresearch.com/), but it seems like a good alternative as well. For a full-on search engine, I would recommend [Elasticsearch](https://www.elastic.co/elasticsearch/).&#x20;

## Virtualization

I have used breach data on my personal machine, and on a virtual machine (which was on my personal machine). There are pros and cons of using virtualization that I have noticed :

### Pros

* **Snapshots**: You can snapshot data before modifying it to return to a clean slate in case you mess up the data.
* **Malicious Payload Protection**: If you accidentally download malware thinking it was legitimate breach data, you have a layer of protection between your host machine and your virtual machine.

### Cons

* **Resources**: Since you are using a virtual machine, you will not be using the resources directly from the host system. Instead it will be passed through the virtualization software to then translate to virtual system resources. I have heard of GPU pass-through setups, where people use an additional GPU on their PC, just to use it for a virtual machine. This is usually for something like virtual gaming, but this can be considered here as well.&#x20;

When it comes to virtualization (Type 2 Hypervisors), there are two names that stand out in the industry: VirtualBox and VMware. VirtualBox is open-source version and has some bugs here and there. VMware is not open source, but is well polished. However, VMware costs money, currently charging $199 for their Fusion Pro software and $149 for their Fusion Player software. I use VirtualBox mainly, and was able to resolve any bugs by doing a quick search online. There are a lot of VirtualBox and VMware comparisons online, so I will not include this here. Here are a couple alternatives to VirtualBox and VMware:

* Quickemu: [https://github.com/quickemu-project/quickemu](https://github.com/quickemu-project/quickemu)
* KVM (Type 1 Hypervisor): [https://ubuntu.com/blog/kvm-hyphervisor](https://ubuntu.com/blog/kvm-hyphervisor)

## Breach Life Cycle

<figure><img src="../../.gitbook/assets/Data Breach Life Cycle.cleaned (3).png" alt=""><figcaption></figcaption></figure>

This diagram is based off of the OSINT Life Cycle and is my attempt to summarize the data breach process.&#x20;

### Data Breach

There are various ways data can be breached or exfiltrated from an organization or an environment. Here are some of the common methods that are heard about in the Cybersecurity industry:

* **Breach**: Data breaches are usually one-offs in the sense that a threat actor accesses data on an environment, downloads that data, and then leaves. This data will usually end up on a marketplace later to be sold.
* **Leak**: Leaks are considered accidental breaches. These are usually done by an insider at a company who unintentionally sends corporate data to an unintended recipient. Leaks can also be caused due to software misconfiguration.
* **Ransomware**: Ransomware attacks occur when a threat actor is able to infiltrate an organization, exfiltrates data, and then encrypts data on the victim's system. This allows the threat actor to ransom the victim to get their data back or deleted from the threat actor's system.
* **Stealer Logs**: Stealer logs contain information gathered using malware placed on a system. Information can include browser cookies, saved credentials, screenshots of victim's desktop, etc. These are usually at the individual-level, and not at an organizational level. Although these are less common than breaches, they tend to have more sensitive data.&#x20;

### Collection

There are multiple locations where breached data is released to. I will not be linking sites directly here, although a good starting place might be: [https://github.com/fastfire/deepdarkCTI](https://github.com/fastfire/deepdarkCTI). Here are the following where they are commonly found:

* Breach Marketplaces
  * Sellers post links to the breach data, and you have to usually purchase the marketplaces' currency to obtain access to these breaches. Mostly the breaches here will be for profit, and some will be free.&#x20;
* Telegram Channels
  * A group might post breaches they have collected or breaches from organizations they have hit. These are a mix of some being free and some being asked for a price.
* Ransomware Group Sites
  * Ransomware data is not posted up-front. The group indicates their victim organization, and what they claim to have exfiltrated from the victim's infrastructure. This is in an attempt to get the victim to pay a price to get their exfiltrated data deleted. If negotiations do not go well, then the victim's data is posted publicly. These are usually free for people to download. Since these sites are hosted on TOR, the download can take a long time. Ransomware groups are now transitioning over to using torrents as well to make it easier for others to download data.

There are multiple ways about going about collecting breach data. You can grab data from every breach, or you can be intentional about the breach data you collect. My recommendation would always be the latter. I have never paid for any breach data, and I would not recommend you do either. With time, breaches usually move on from the initial marketplace to other marketplaces, and then end up being free. From an OSINT perspective, grabbing the largest breach data sets should provide us enough information for a subject/target. From there, if we need to grab specific breaches, we can. Here are the current largest breaches from Have I Been Pwned:

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

Having these in our arsenal will give us a good starting point when diving into a subject/target. Using a browser works well for small file downloads. However, as breaches can get big and can take a while to download, we have to look towards torrent software and applications meant specifically for downloading. If a file is on a torrent and will take a while, you could also use a [seedbox](https://www.howtogeek.com/764504/what-is-a-seedbox-and-why-would-you-want-one/) to download the torrents for you. Just a warning, **seedboxes are really costly.**

Torrent:

* [qBittorrent](https://www.qbittorrent.org/)
* [Transmission](https://transmissionbt.com/)

Download Managers:

* [pyLoad](https://pyload.net/)
* [jdownloader2](https://jdownloader.org/jdownloader2)
* [persepolis](https://github.com/persepolisdm/persepolis)

### Processing

At the stage of processing, the main goal is to "clean" the data and get it into a format that you can use to integrate it into the infrastructure later.&#x20;

#### Security

Before getting into the cleanup, I like to run `clamscan` from [ClamAV](https://docs.clamav.net/Introduction.html) for an initial scan to make sure the files are not malicious. If I am still not comfortable with the security of the file, I then also use VirusTotal as well. I believe they do have a limit of 100-200 MB limit for file upload for the free tier. If you have a breach file bigger than that, consider using the SHA-256 hash value of the file to see if that shows up on VT. If you are still worried about security, consider using a sandbox like [Cuckoo](https://cuckoosandbox.org/) or [DRAKVUF](https://github.com/CERT-Polska/drakvuf-sandbox/).

#### Duplication / Snapshot

Since we currently have one copy of the breach data, we want to replicate the data so we have a clean copy we can come back to in case we delete a file or unintentionally modify it in some way. On Linux you can use the default `cp` or `rsync`. While doing research for this blog, I did come across [https://github.com/Svetlitski/fcp](https://github.com/Svetlitski/fcp), which claims to work faster than `cp`. For Windows, you could use the click on the folder, hit CTRL+C, and then paste it elsewhere with CTRL+V. If you are using a virtual machine and have limited space, you can create a snapshot after the data has been downloaded and before you made any modifications to it. This will allow you to come back to a previous snapshot, if you wanted an untouched version.

**Cleanup**

There are some breaches that have data that does not have beneficial use for an Analyst. For example, if a breach has a line number at the beginning of each line, that is not necessarily beneficial information, and is data that can be removed. I really prefer Linux for cleaning data due to its native command line programs like `sed`, `awk`, `grep`, etc. They really make it easy to format the data how we want. Based on my research, [it seems you can do the same with PowerShell on Windows as well](https://stackoverflow.com/questions/127318/is-there-any-sed-like-utility-for-cmd-exe), but it does have a learning curve to it, which I consider greater than the Linux programs.

#### Formatting

If you choose to index your breach data, you will have to format it in a way that will be readable by the search engine in the back-end. I have seen the CSV format be readable by most search engines, however your mileage may vary. For the [ELK Stack](https://www.elastic.co/elastic-stack/), you have Logstash which transforms the data for Elasticsearch to use. For Logstash, you have to use the specified formats (which includes CSV). Each index software is different.&#x20;

### Integration

Integration really breaks down to whether or not your data is organized or indexed in any way or not. For example, if you have all of your breach data in an alphabetical order, then you can separate your data into those A-Z folders and integrate the data to your system that way. Maybe you want to separate your data by breach type and name, such as the following:

```
Breach Data/
├── Breach/
│   └── ParkMobile/
├── Ransomware/
│   ├── Rhysida/
│   └── Clop/
├── Leak/
│   └── COVID-19 immunity data/
└── Stealer Logs/
    ├── Raccoon/
    ├── Redline/
    └── Vidar/
```

It does depend on what you deem is more convenient for the Analyst(s) working with this data. My recommendation would be similar to what I have shown in the diagram above. This way, you can drill down to a single dataset and focus on that much easily.&#x20;

Indexing takes a bit more work for integration. You have to learn the formats that the indexing solution is able to ingest: CSV, JSON, etc. After this, you have to create the configuration for the pipeline based on the format decided on. Let's look at an example from Elasticsearch. We use Logstash to format the data for it to be readable and index-able by Elasticsearch. Logstash needs a configuration file to understand the format data you are inputting into it. This would look like the following:

```json
// breach.conf
input {
    file {
        path => "<full path to breach csv file>"
        start_position => beginning
    }
}
filter {
    csv {
        columns => [
                "Email",
                "Password"
        ]
        separator => ","
        }
}
output {
    stdout
    {
        codec => rubydebug
    }
     elasticsearch {
        action => "index"
        hosts => ["127.0.0.1:9200"]
        index => "breach"
    }
}
```

With the above configuration, we tell Logstash the following: location of breach file, columns in the breach data, and the location of the Elasticsearch instance. Each search engine solution has its own "Logstash", which means each pipeline will be different. Each breach is also different, meaning that you will have a different configuration for each breach.

### Analysis

After the data is integrated into the environment, analysis of the data would be considered the easier part. If you have indexed your data, then you can use the search engine GUI or API for querying the data. If you do end up using the API, I would recommend checking out [Postman](https://www.postman.com/), as it makes the work a bit easier and looks much more formal than a terminal. An example Postman query for breach data can look like the following:

<figure><img src="../../.gitbook/assets/image (751).png" alt=""><figcaption></figcaption></figure>

For data that is not indexed, there are programs on Linux that work great for finding a certain string (email, password, hash, etc.) you are looking for. The standard program I use and have heard about a lot is `grep`, but I wanted to point some programs out that have worked for me and/or seem to be better alternatives for grep:

* [ripgrep](https://github.com/BurntSushi/ripgrep)
* [ripgrep-all](https://github.com/phiresky/ripgrep-all)
* [ack](https://beyondgrep.com/)
* [ag](https://github.com/ggreer/the\_silver\_searcher)

#### Data Visualization

For an OSINT Analyst, the aforementioned tools should be sufficient to conduct their investigations. However, I do understand the need to have an interesting visual to support the data for the higher-ups and/or the stakeholders. I have only used [Kibana](https://www.elastic.co/kibana/), which comes in the ELK Stack. However, I like to provide multiple options so, here are some that I had found while researching this topic:

* [Apache Superset](https://superset.apache.org/)
* [Grafana OSS](https://grafana.com/oss/grafana/)
* [ECharts](https://echarts.apache.org/en/index.html)

## Conclusion

Ever since I started my OSINT journey, I was fascinated by breach data. It seemed too good to be true at first. As I dived more and more into my OSINT journey, breach data did have really interesting and important information. There are a lot of OSINT and Cyber Threat Intelligence-minded people that discuss the importance of breach data. However, I have not seen a lot of people talk about building an environment sufficient to handle breach data. This was the goal of this blog.&#x20;

## Sources

* [https://www.csnp.org/post/a-beginners-guide-to-osint](https://www.csnp.org/post/a-beginners-guide-to-osint)
* [https://inteltechniques.com/blog/2022/07/06/new-breach-data-lesson-ii-stealer-logs/](https://inteltechniques.com/blog/2022/07/06/new-breach-data-lesson-ii-stealer-logs/)
