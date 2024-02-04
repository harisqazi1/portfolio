# Digital Books

## E-Reader

Before getting into digital books, there is a consideration that needs to be made on hardware. When people think of "e-reader", they think Amazon Kindle. It has made a name in the industry for having a great selection of books and being energy efficient. However, what people don't realize is that they are "renting" books from Amazon, even if they buy the book (**I would still recommend people to buy Kindles for the price, but purchase books from external parties**). This is due to how licensing agreements work. You might have heard of this more from the TV and Movie realm ([IGN - Sony Pulls Discovery Videos PlayStation Users Already Own, Sparking Concern Over Our Digital Future](https://web.archive.org/web/20240112223030/https://www.ign.com/articles/sony-pulls-discovery-videos-playstation-users-already-own-sparking-concern-over-our-digital-future) or [PCWorld - Amazon Removes E-Books From Kindle Store, Revokes Ownership](https://web.archive.org/web/20240112223221/https://www.pcworld.com/article/524327/kindle\_e\_book.html)). This led me to look into other e-readers and see which ones can prevent that from happening to me. I started off by reading [https://www.reddit.com/r/ereader/wiki/ereaders\_101/](https://www.reddit.com/r/ereader/wiki/ereaders\_101/) (archived link: [https://web.archive.org/web/20240112223842/https://www.reddit.com/r/ereader/wiki/ereaders\_101/](https://web.archive.org/web/20240112223842/https://www.reddit.com/r/ereader/wiki/ereaders\_101/)). From here, PocketBook stood out to me for a couple reasons:

* Cheaper than Onyx Boox devices
* Sleek and simple design
* No region lock-in for books

If you visit [https://pocketbookstore.com/](https://pocketbookstore.com/), you will notice that there are a lot of options to choose from. I ended up with Pocketbook InkPad 4 because of one line: "The new e-reader also stands out with a screen size as close as possible to a standard printed book". This line (located on [https://pocketbookstore.com/products/pocketbook-inkpad-4-e-book-reader](https://pocketbookstore.com/products/pocketbook-inkpad-4-e-book-reader)), is what sold this version for me. I have made a small table below to compare 2 of the PocketBook readers that stood out to me (details as of 1/12/2024):

<table><thead><tr><th width="194">Name</th><th>Pocketbook InkPad 4</th><th>Pocketbook InkPad Lite</th></tr></thead><tbody><tr><td>Screen Size</td><td>7.8 inch</td><td>9.7 inch</td></tr><tr><td>Resolution</td><td>1404 × 1872 and 300 PPI</td><td>1200x825 with 150 PPI</td></tr><tr><td>Storage Size</td><td>32 GB</td><td>8 GB + 128GB</td></tr><tr><td>Wi-Fi?</td><td>Yes</td><td>Yes</td></tr><tr><td>Book/Graphic Support</td><td>21 book and 4 graphic formats (types not listed)</td><td>ACSM, CBR, CBZ, CHM, DJVU, DOC, DOCX, EPUB, EPUB(DRM), FB2, FB2.ZIP, HTM, HTML, MOBI, PDF, PDF (DRM), PRC, RTF and TXT</td></tr><tr><td>Price</td><td>$259.99</td><td>$209.99 </td></tr></tbody></table>

### Configuration

* Connect to Wi-Fi  on first boot
* Update Firmware from Settings
* Remove Wi-Fi Connection
* Connect E-Reader to PC (works on Linux)
* Click "PC Link" on E-Reader
* Add Books under "English" folder at root directory
* Backup the whole PocketBook using rclone/rsync/FreeFileSync
* Download the Manual, Firmware Package, and Release Notes from [https://pocketbook.ch/en-ch/support](https://pocketbook.ch/en-ch/support)
* Connect to Wi-Fi occasionally for updates

I have not used any of these, but more configurations can be found at [https://www.mobileread.com/forums/showthread.php?t=69185](https://www.mobileread.com/forums/showthread.php?t=69185) and [https://www.mobileread.com/forums/showthread.php?t=292165](https://www.mobileread.com/forums/showthread.php?t=292165). In addition, I did try to install **KOReader** ([https://github.com/koreader/koreader/wiki/Installation-on-PocketBook-devices](https://github.com/koreader/koreader/wiki/Installation-on-PocketBook-devices)) on the InkPad 4, but it did not end up working.&#x20;

## E-books

Based on my research, these sites were well-known for buying e-books from:

* [https://www.ebooks.com/en-us/](https://www.ebooks.com/en-us/drm-free/)
* [https://storybundle.com/](https://storybundle.com/)
* [https://angryrobotbooks.com](https://angryrobotbooks.com)
* [https://www.kobo.com/us/en/ebooks](https://www.kobo.com/us/en/ebooks)
* [https://www.humblebundle.com/books](https://www.humblebundle.com/books)
* [https://standardebooks.org/](https://standardebooks.org/)

You can see more here [https://www.defectivebydesign.org/guide/ebooks](https://www.defectivebydesign.org/guide/ebooks).&#x20;

When purchasing e-books, you have to be cognizant of the format of the e-book you are purchasing (DRM-free or not). I recently purchased a book from [https://www.ebooks.com/en-us/](https://www.ebooks.com/en-us/). The second I buy the book, I see the following:&#x20;

<figure><img src="../.gitbook/assets/image (779).png" alt=""><figcaption></figcaption></figure>

I have to download Adobe Digital Editions in order to read a book that I paid for in full. There would be nothing wrong with this....if I was not locked in to reading my book only using Adobe Digital Editions and in .epub (encrypted) format. This is DRM. It is meant to prevent piracy and Intellectual Property, but ends up hurting those who actually pay for the material. This is where I had to learn to break the Adobe DRM from the ebook I had purchased.

{% hint style="danger" %}
According to Abbey House Media v. Apple Inc ([https://www.eff.org/document/abbey-house-media-v-apple-inc](https://www.eff.org/document/abbey-house-media-v-apple-inc)), it is legal to tell people how to remove DRM from books they have purchased. Read the full story here: [https://www.eff.org/deeplinks/2014/12/pointing-users-drm-stripping-software-isnt-copyright-infringement-judge-rules](https://www.eff.org/deeplinks/2014/12/pointing-users-drm-stripping-software-isnt-copyright-infringement-judge-rules). I assume that you have purchased a book, like myself, and are then wanting to use it freely across your own devices.  **I am not writing this for pirated content, nor will I be held liable if you do use it for pirated content or for breaking DRM to distribute books.**
{% endhint %}

### Adobe DRM Removal (Linux)

{% hint style="warning" %}
I would recommend using burner emails and cards for Adobe and for wherever you are purchasing your books from in case you get banned. I will not be responsible if this happens to you.
{% endhint %}

This took me a couple of days to figure out. I will try to break the steps as basic as I can, as I plan to refer to this myself. This is a mix of various people's tutorials. This worked for me on Pop!\_OS 22.04 as of 1/12/2024.&#x20;

* Download Calibre on Linux. I used the Flathub version.&#x20;
  * `sudo apt install flatpak`
  * `flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo`
  * (Optional) You might have to restart to use flatpak
  * `flatpak install com.calibre_ebook.calibre`
    * You could download for user or system
* Download and use the ACSM Calibre Plugin
  * Download the latest release here: [https://github.com/Leseratte10/acsm-calibre-plugin/releases](https://github.com/Leseratte10/acsm-calibre-plugin/releases)
    * It helps to download all files going forward to one location, to make locating these files easier
  * Open Calibre
    * I kept defaults for all choices on first application open
    * On the top right, you will see 3 "bubbles". Click on them
      *

          <figure><img src="../.gitbook/assets/image (780).png" alt=""><figcaption></figcaption></figure>
    * Click Preferences > Plugins (under Advanced) > Load plugin from file (bottom left choice) > choose the zip you just downloaded
      * This will prompt you to restart Calibre, do so
    * Click on the same 3 "bubbles" > Plugins (under Advanced) > click the arrow next to File Type > double click on "DeACSM (0.0.XX) by Leseratte10"
      * &#x20;Click "Link to ADE account" (I chose this option, but you see information on the other options here: [https://github.com/Leseratte10/acsm-calibre-plugin](https://github.com/Leseratte10/acsm-calibre-plugin))
      * For AdobeID provider, the default is "Adobe ID". Keep it like that.
      * Enter email address and then password
      * Choose ADE 2.0.1 (This worked for me, feel free to choose 4.X.X if you want)
    * It should now look like this
      *

          <figure><img src="../.gitbook/assets/image (781).png" alt=""><figcaption></figcaption></figure>
    * Export the encryption key and activation data to a safe place (we will need these later)
      * We need the encryption key to break DRM
      * It is good to have the activation data just in case
* Install wine and winetricks
  * `sudo wget -O /etc/apt/keyrings/winehq-archive.key https://dl.winehq.org/wine-builds/winehq.key`
  * `sudo wget -NP /etc/apt/sources.list.d/ https://dl.winehq.org/wine-builds/ubuntu/dists/jammy/winehq-jammy.sources`
  * `sudo apt update`
  * `sudo apt install --install-recommends winehq-stable winetricks winbind`
  * `winetricks dotnet40`
    * This will prompt you to download `mono` and `.NET`
* Download the .acsm file from the book store and store it in a safe place. For ebooks\[.]com, the file was "URLLink.acsm".&#x20;
* Install and set up Adobe Digital Editions
  * Download the Windows version from [https://www.adobe.com/solutions/ebook/digital-editions/download.html](https://www.adobe.com/solutions/ebook/digital-editions/download.html).
  * `wine FOLDER/ADE_4.5_Installer.exe` (FOLDER = where you saved the installer)
    * You could just `cd` to the directory, and run `wine ADE_4.5_Installer.exe`
  * Accept terms
  * (Optional) Uncheck `Start Menu Shortcuts`, `Desktop Shortcut`, and `Quick Launch Shortcut`
  * Install in default location
  * Say no to Norton install&#x20;
  * This will automatically open Adobe Digital Editions after install
    * If you accidentally clicked off of it, you can run find it in the following directory: `~/.wine/drive_c/Program Files (86)/Adobe/Adobe Digital Editions 4.5/`
    * `cd` there and run `wine DigitalEditions.exe`
  * Help > Authorize Computer
  * Download a copy of your book (if on the cloud) or add the .ascm file to Adobe Digital Editions (can be dragged and dropped)
* Install and set up Adobe Digital Editions (Alternate Version)
  * `winetricks adobe_diged`
    * If Adobe Digital Editions does not open up automatically run: `wine ~/.wine/drive_c/Program\ Files\ (x86)/Adobe/Adobe\Digital\ Editions/digitaleditions.exe`
  * Drag and drop the .acsm file to Adobe Digital Editions
  * Clicking the arrow on the left of the book and choosing "Item Info" will tell you where the file is saved on Windows (wine):
    *

        <figure><img src="../.gitbook/assets/image (782).png" alt=""><figcaption></figcaption></figure>
  * **However,** there should be a folder in your Documents folder called "My Digital Editions" created
    *

        <figure><img src="../.gitbook/assets/image (783).png" alt=""><figcaption></figcaption></figure>
* DRM Removal
  * Download the DeDRM tool from [https://github.com/apprenticeharper/DeDRM\_tools/releases](https://github.com/apprenticeharper/DeDRM\_tools/releases) (I used 7.2.1 as the version by NoDRM did not end up working for me)
  * Unzip the .zip and then unzip the "DeDRM\_plugin.zip" file inside of it
    * We need the ineptepub.py file from here
  * `python3 ineptepub.py adobe_uuidxxxxxxxxxxxxxxx.der 1984.epub test1984.epub`
    * `adobe_uuidxxxxxxxxxxxxxxx.der` is one of the files we output from the DeASCM plugin earlier
    * `1984.epub` was the file in "My Digital Editions" (\~/Documents/My\ Digital\ Editions)
    * `test1984.epub` was the output file name I chose
*

    <figure><img src="../.gitbook/assets/image (784).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you export to PDF, it should be in /home/(your username here)/Calibri\ Library/(Author Name)/(Book Name)/
{% endhint %}

#### Sources

* [https://github.com/Leseratte10/acsm-calibre-plugin](https://github.com/Leseratte10/acsm-calibre-plugin)
* [https://superuser.com/questions/1027608/how-to-read-an-acsm-file-on-linux](https://superuser.com/questions/1027608/how-to-read-an-acsm-file-on-linux)
