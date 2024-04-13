# Backups

Backups are a necessity in case of failure. Whether it is disk/drive failure or software failure, you want to have, well, backups in place to make sure you are able to get back on track. One of the most popular personal backup strategies is the 3-2-1 backup method. This snippet from Seagate says it well:

> 3 Copies of Data – Maintain three copies of data—the original, and at least two copies.
>
> 2 Different Media – Use two different media types for storage. This can help reduce any impact that may be attributable to one specific storage media type. It’s your decision as to which storage medium will contain the original data and which will contain any of the additional copies.
>
> 1 Copy Offsite – Keep one copy offsite to prevent the possibility of data loss due to a site-specific failure.

Using this format will help you get back up and running much faster than starting off from scratch. One thing to note with backups is that they work off of a proactive approach. This means you have to not only do this once, but maintain a healthy backup lifestyle in order to prevent data loss as much as you can. You can also have your own "3-2-1-#-#-#rule", where you add your own customization to the basic strategy. You do have to take the cost of the storage devices into mind, whether it is cloud storage providers or even for a hard drive purchase.&#x20;

There are multiple ways of following the rule. Here are some of the other location suggestions:

* Backups on disk (DAS, SAN, NAS and appliances)
* Backups on tape
* Backups on removable storage
* Storage snapshots
* External storage stored at a friends/relatives house
* External storage stored in a bank vault

There are a lot of complications to consider for backups as well:

* Weather
* Potential Fire
* Electric Static (while moving the device(s))
* Home Invasion
* Police Confescation
* Drive/Disk Failure
* etc.

Using the 3-2-1 is a way of being proactive and making your data be redundant in case of any of the aforementioned complications.

## File System Formats

For data backups, and even for data storage in general, it is a good idea to take a look at the properties that a file system format gives you. Keeping these in mind can help you choose what format is better for your situation.

<table><thead><tr><th width="168.33333333333331"></th><th width="127">NTFS</th><th>APFS </th><th>HFS+ </th><th>exFAT</th><th>FAT32</th></tr></thead><tbody><tr><td><strong>OS Readable</strong></td><td>Windows (Native), macOS (Read-Only), Linux</td><td>macOS (Native)</td><td>macOS (Native)</td><td>Windows, macOS, Linux</td><td>Windows, macOS, Linux</td></tr><tr><td><strong>Journaling</strong></td><td>X</td><td>X</td><td></td><td></td><td></td></tr><tr><td><strong>Permissions Tracking</strong></td><td>X</td><td>X</td><td>X</td><td></td><td></td></tr><tr><td><strong>Max File Size</strong></td><td>16TB Default (4KB Cluster Size), 2MB Cluster Size 8 PB</td><td>8 EB</td><td>slightly less than 8 EB</td><td>16 EB</td><td>4GB</td></tr><tr><td><strong>Max Volume Size</strong></td><td>16TB Default (4KB Cluster Size), 2MB Cluster Size 8 PB</td><td></td><td>slightly less than 8 EB</td><td>64 ZB</td><td>512 MB to 16 TB</td></tr><tr><td><strong>Legend</strong></td><td>X = yes</td><td></td><td></td><td></td><td></td></tr></tbody></table>

## Encryption

From the CIA triad (Confidentiality, Integrity, and Availability), encryption falls under the Confidentiality principle. This means that only authorized users are allowed to access/modify the data. In order to apply this principle to our data, we need to encrypt our data, so we can only see it. Although there are brute force attempts possible to find out a password, I will not go deep into that here. Here are the tools I use for encryption:

* VeraCrypt (for Full Disk Encryption)
* Cryptomator (for when I have to store files in the cloud)

There are more software located at: https://www.privacyguides.org/encryption/ if you are interested. For encryption passwords, make sure you use a password manager and create long passwords or create long passphrases. Here is an xkcd comic that might help clarify whether to choose password vs passphrase (if you have a password manager, then this might not matter too much):

<figure><img src="../.gitbook/assets/2b1be6fa029c4b8fba8a15df5b3b5fc3.cleaned.png" alt=""><figcaption></figcaption></figure>

## Backup Lifestyle

After your data is encrypted, and your file system of choice is chosen, the last step is to actually backup the data. The first time you are backing up data, it usually is a full backup. "The primary advantage to performing a full backup during every operation is that a complete copy of all data is available with a single set of media" . This takes the longest time due to the amount of data being transferred. After the full backup is completed, the nest two backup operations become available to you: incremental backups and differential backups. Incremental backups will copy only the data that has changed since the last backup operation of any type . Incremental backups keep on adding files that are new or modified from the last backup. Incremental backups do not take into account if you previously did a full backup or an incremental backup. Differential backups on the other hand, do take that into account. A differential is similar to incremental in the first time it is run. "However, each time it is run afterwards, it will continue to copy all data changed since the previous full backup. Thus, it will store more backed up data than an incremental on subsequent operations, although typically far less than a full backup. Moreover, differential backups require more space and time to complete than incremental backups, although less than full backups". With that being said, here are some tools I use for backups:

Tools:

* Déjà Dup Backups (for backing up Linux home directory files)
* FreeFileSync (for syncing files from one folder to another)

After the backups are complete, you have to setup a cadence at which you will keep backing up on. For some it's daily, for others, its monthly. It is different for everybody.

## Large Volume Backups

There are times where the volume or storage size of your data is big enough where it will be really costly to put it in the cloud. An example of this if you were a data archivist and were downloading TB's of data. Putting that in the cloud can be costly. See the following information (grabbed from [https://www.backblaze.com/cloud-storage/pricing#calculator](https://www.backblaze.com/cloud-storage/pricing#calculator) so it might be biased, as they are a cloud provider themselves):

|                     | Backblaze B2                                               | AWS S3 | Azure | Google Cloud |
| ------------------- | ---------------------------------------------------------- | ------ | ----- | ------------ |
| Storage (TB/month)  | $6                                                         | $26    | $20   | $23          |
| Egress ($/GB)       | Free (up to 3x average monthly data stored, then $0.01/GB) | $0.09  | $0.08 | $0.11        |
| 2TB Storage / year  | $144                                                       | $624   | $499  | $552         |
| 10TB Storage / year | $720                                                       | $3120  | $2496 | $2760        |

I put the 2 TB and 10 TB on the bottom to make people aware of how much the cost can increase exponentially. Data archival can get expensive really quickly. In addition, if you want to follow the 3-2-1 protocol, then that is even more storage to consider. As such, I think it is cost-efficient to then think locally for solutions for this. If you are comfortable paying for cloud with TBs of data (or have a NAS setup), then, by all means go ahead. The following is just to have backups in the Terabytes, while also not spending too much money. If you have money to allocate for this, then cloud and/or NAS solutions would work great. If not then the following includes buying hard drives (internal and external) and also other devices to make backups. Based on my experience here are the following things you need:

* Multiple Hard Drives (get the size of data you plan to store)
  * Try to buy from between the top brands to make sure there is variety in your drive manufacturers
    * Seagate and Western Digital are the top brands at the moment, but keep an eye on [https://www.backblaze.com/cloud-storage/resources/hard-drive-test-data](https://www.backblaze.com/cloud-storage/resources/hard-drive-test-data) for hard drive stats
      * Western Digital bough HGST, otherwise HGST is pretty good as well
  * Recommended Readings
    * Read: [https://www.backblaze.com/blog/how-long-do-disk-drives-last/](https://www.backblaze.com/blog/how-long-do-disk-drives-last/)
    * Understand Claimed vs Real capacity: [https://platinumdatarecovery.com/hard-drive-capacity-calculator](https://platinumdatarecovery.com/hard-drive-capacity-calculator)
    * Understand CMR vs SMR: [https://www.wundertech.net/cmr-vs-smr-what-is-the-best-hard-drive/](https://www.wundertech.net/cmr-vs-smr-what-is-the-best-hard-drive/)
    * Read Seagate comparison: [https://www.broadbandbuyer.com/advice/3755-seagate-hard-disk-drive-comparison-chart/](https://www.broadbandbuyer.com/advice/3755-seagate-hard-disk-drive-comparison-chart/)
* Hard Drive Docking Station
  * Minimum 2 bay would be recommended, so you can transfer between 2 drives if you needed at the minimum
  * Try to get UASP Protocol Support as it increases the read/write speed (see more:  [https://www.startech.com/en-us/blog/all-you-need-to-know-about-uasp](https://www.startech.com/en-us/blog/all-you-need-to-know-about-uasp))
  * A HDD enclosure is meant to keep a drive there for most of its life, and as that is not our use case, we don't need it here (see more: [https://www.reddit.com/r/DataHoarder/comments/hftmsb/hdd\_enclosure\_vs\_docking\_station/](https://www.reddit.com/r/DataHoarder/comments/hftmsb/hdd\_enclosure\_vs\_docking\_station/))
  * NOTE: A docking station could potentially bottleneck read/write speed as it is a middle-man. This could be of significant effect when applying encryption via a PC or laptop to a hard drive the dock
* Hard Drive Storage Carrying Case (optional)
  * If you have an internal drive, I would recommend having a case for it to protect it from the elements and from mishaps
* Fire and water-proof safe or container (optional)
  * Recommended to just add a layer of protection in case fire or water damage effects your drive

### Stress Testing

After purchasing a new (or even used) hard drive, it is important to test the drive to make sure it functions properly under stress before fully utilizing the drive in operation. There are 2 main things I consider here: S.M.A.R.T. (Self-Monitoring, Analysis, and Reporting Technology a.k.a SMART) data and stress testing outputs. S.M.A.R.T. detects and reports drive information which can potentially report hardware failures to you. It is kind of like a health monitor for a drive in a way. On Linux a great tool for this is `smartctl`, and on Windows there is [CrytalDiskMark](https://crystalmark.info/en/software/crystaldiskmark/).&#x20;

As stated previously, S.M.A.R.T. is the first main part to consider here, and stress testing is the other. I use `fio` for stress testing. I run fio on Linux, but this can be used on Windows as well: [https://github.com/axboe/fio/releases](https://github.com/axboe/fio/releases). The stress testing allows you to push your drive to see it's read and write capacity under load for you to understand if it is performing as expected or not.

The following is my procedure (current to April 2024):

#### Stress Testing Procedure

{% hint style="warning" %}
/dev/sdc is the location of **my drive**. Yours might be different. Triple check to make sure you choose the correct drive to make these changes on. On Linux, use the Disks app to make sure or `lsblk` command to do so.
{% endhint %}

```bash
# 1 - Perform a Long S.M.A.R.T test with smartctl
# MAKE SURE TO RUN THE SECOND COMMAND WHILE THE FIRST IS RUNNING
# Second command makes sure to keep the device awake, otherwise your device will go to sleep. 
# I recommend terminator on Linux to create side-by-side windows
sudo smartctl -t long -C /dev/sdc # Long test; Takes 656 minutes (~11 hours) to complete 
sudo watch -d -n 60 smartctl -a /dev/sdc # This runs smartctl -a /dev/sdc every minute to check your drive; This keeps your device awake as well.
# Run the fio stress test
# Source for version of command: https://www.reddit.com/r/DataHoarder/comments/7wh4a6/comment/du2elge/
# Following runs for 7200 seconds (~2 hours)
sudo fio --filename=/dev/sdc --name=randwrite --ioengine=sync --iodepth=1 --rw=randrw --rwmixread=50 --rwmixwrite=50 --bs=4k --direct=0 --numjobs=8 --size=300G --runtime=7200 --group_reporting
# A final small S.M.A.R.T test to make sure drive is healthy
sudo smartctl -t short -C /dev/sdc
```

### Encryption

After stress testing, you can use a software to encrypt either files in batches (ex. Cryptomator) or full-disk encrypt (ex. VeraCrypt) your whole drive. In my experience, using VeraCrypt on a 6 TB hard drive took **37 hours to complete**, and was at a rate of 42 MiB/s. **Currently, not sure if this took long due to encryption, size of drive, or the hard drive dock I was using.**

## Data Transfer

Here are a couple of tools that help transferring huge files from one location to another:

* rsync ([https://en.wikipedia.org/wiki/Rsync](https://en.wikipedia.org/wiki/Rsync))
* rclone ([https://rclone.org/](https://rclone.org/))
* FreeFileSync ([https://freefilesync.org/](https://freefilesync.org/))

### Storage

After the main data maintenance is completed, storage is the last part. Here is where the optional hard drive case, fire-proof, and water-proof containers/safes come into play. If you have a drive in your house, and it is only used to backup occasionally, you need a place to store it where the elements or other items will not come into contact with it. These containers allow a solution for that. A hard drive case allows to protect from dirt and to protect your drive from drops. A fire and water-proof safe will protect from, well fire and water, but also from the elements like sunlight. Our goal is not to make a Fort Knox in our house for hard drives, but just try to preserve it as much as we can, so we don't have to buy new drives as frequently. If you have more than one backup, then my recommendation would be to give it to a friend, store it in a bank safety deposit box, or in a 24/7 temperature-controlled storage unit. These would be away from your house, allowing a copy of your data to be safe in case an intruder takes everything or if nature takes all of your things away. These options would be more suited financially for you than cloud storage. However, if none of these are feasible for you, then I would recommend looking into cloud.

## Sources:

* https://www.seagate.com/blog/what-is-a-3-2-1-backup-strategy/
* https://www.veeam.com/blog/321-backup-rule.html
* https://www.seagate.com/support/kb/file-system-format-comparisons-006171en/
* https://en.wikipedia.org/wiki/Comparison\_of\_file\_systems
* https://www.csoonline.com/article/3519908/the-cia-triad-definition-components-and-examples.html
* https://xkcd.com/936/
* https://youtu.be/S0KZ5iXTkzg
  * https://yewtu.be/watch?v=S0KZ5iXTkzg (Invidious link)
* https://flathub.org/apps/details/org.gnome.DejaDup
* https://rsync.samba.org/
* https://www.techtarget.com/searchdatabackup/feature/Full-incremental-or-differential-How-to-choose-the-correct-backup-type
* [https://www.thomas-krenn.com/en/wiki/SMART\_tests\_with\_smartctl](https://www.thomas-krenn.com/en/wiki/SMART\_tests\_with\_smartctl)
* [https://superuser.com/questions/766943/smart-test-never-finishes-what-can-be-done-to-let-them-complete](https://superuser.com/questions/766943/smart-test-never-finishes-what-can-be-done-to-let-them-complete)
* [https://www.linuxjournal.com/article/6983](https://www.linuxjournal.com/article/6983)
* [https://en.wikipedia.org/wiki/Self-Monitoring,\_Analysis\_and\_Reporting\_Technology](https://en.wikipedia.org/wiki/Self-Monitoring,\_Analysis\_and\_Reporting\_Technology)
