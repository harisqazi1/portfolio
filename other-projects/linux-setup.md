# Linux Setup

This is just a backup of my setup I have on my Linux OS, in case I ever have to start a Linux OS from scratch.

### Software

| Name | Type |
| :--- | :--- |
| DejaDup | Backup Creation |
| Firefox | Browser |
| Discord | Communication |
| FreeTube | YouTube Privacy-centric Alternative |
| Htop | Process List Viewer |
| KeePassXC | Password Manager \(locally hosted\) |
| Bitwarden | Password Manager \(cloud hosted\) |
| Lutris | Run games from Epic Games |
| Thunderbird Mail | Email Client |
| Mullvad VPN / ProtonVPN | VPN Client |
| LibreOffice | Microsoft Office Alternative |
| OpenRGB | Logitech G HUB / Razer Chroma RGB Alternative |
| Piper | DPI software for the Mouse |
| qBittorrent | Torrent software |
| Steam | Gaming Client |
| Variety | Wallpaper software |
| VeraCrypt | Encryption software |
| Visual Studio Code | Coding IDE |
| VLC | Video Player |

### Firefox

* General
  * Uncheck **Restore previous session**
    * If you use a VPN and your connection dies, then when you use Firefox again, it will not spawn the same website, which would give away your data to the website
  * In **Network Settings**, enable **DNS over HTTPS** and choose **Cloudflare**
* Home
  * Set **Homepage and new windows** to **Blank Page**
  * Set **New tabs** to **Blank Page**
    * These will prevent Firefox from spawning their own sites when you need a new page/tab
  * Uncheck everything under **Firefox Home Content** \(Shortcuts, Recommended by Pocket, Recent activity, Snippets\)
    * This will prevent Firefox from loading their own content
* Search
  * Change **Default Search Engine** to **DuckDuckGo**
  * Uncheck **Provide Suggestions**
    * Prevents the queries from going to the Google API
* Privacy & Security
  * Under **Cookies and Site Data**, check **Delete cookies and site data when Firefox is closed**
  * Under **Logins and Passwords**, uncheck the boxes \(with the inside list first\):
    * **Show alerts about passwords for breached websites**
    * **Suggest and generate strong passwords**
    * **Autofill logins and passwords**
    * and then, **Ask to save logins and passwords for websites**
  * Under **History,** in the dropdown choose Firefox will **Use custom settings for history**.
    * Also uncheck, **Remember search and form history**, and **Remember browsing and download history**
    * Check **Clear history when Firefox closes**
    * **DO NOT** check the box next to **Always use private browsing mode**
      * It will break Firefox containers
  * Under **Address Bar,** only have **Bookmarks** and **Open tabs** showing up
  * Under **Permissions**, click **Settings**... next to **Location, Camera, Microphone,** and **Notifications**. In the popup, check the **"Block new requests.."** box on the bottom.
  * Under **Firefox Data Collection and Use**, uncheck everything
  * Under **Security**, uncheck everything
  * Under **HTTPS-Only Mode**, best practice would be to use **Enable HTTPS-Only Mode in all windows**. However, I use **Enable HTTPS-Only in private windows only**

These are the settings I use when you type in **about:config** in the search bar:

* **geo.enabled** -&gt; false
  * Disables Firefox from sharing your location
* **browser.safebrowsing.malware.enabled** -&gt; false
  * Disables Google's ability to monitor your web traffic for malware
* **dom.battery.enabled** -&gt; false
  * Blocks sending battery level information
* **extensions.pocket.enabled** -&gt; false
  * Disables the Pocket feature
* **browser.newtabpage.activity-stream.section.highlights.includePocket** -&gt; false
* **browser.newtabpage.activity-stream.feeds.telemetry** -&gt; false
* **toolkit.telemetry.server** -&gt; \*DELETE URL\*
  * Removes telemetry
* **toolkit.telemetry.unified** -&gt; false
  * Removes telemetry
* **media.autoplay.default** -&gt; 5
  * Disables audio and video from playing automatically
* **dom.webnotifications.enabled** -&gt; false
  * Disables embedded notifications
* **privacy.resistFingerprinting** -&gt; true
  * Disables some fingerprinting
* **webgl.disabled** -&gt; true
  * Disables some fingerprinting
* **network.http.sendRefererHeader** -&gt; 0
  * Disables referring website notifications \(**BREAKS SOME SITES\)**
* **identity.fxaccounts.enabled** -&gt; false
  * Disables any embedded Firefox accounts
* **beacon.enabled** -&gt; false
  * Disables data being sent to servers when leaving pages
* **browser.cache.disk.enable** -&gt; false
  * Disk cache is not used by Firefox
*  **browser.cache.disk\_cache\_ssl** -&gt; false
  * Firefox will not cache https website contents.
* **geo.provider.network.url** -&gt; 127.0.0.1
  * The data provider used to power Firefox's geolocation feature
* **network.cookie.lifetimePolicy** -&gt; 2
  * Determines whether Firefox will accept so-called session cookies \(removed when browser exits\) automatically.
    * 0 = The cookie's lifetime is supplied by the server. \(Default\)
    * 1 = The user is prompted for the cookie's lifetime.
    * 2 = The cookie expires at the end of the session \(when the browser closes\).
    * 3 =  The cookie lasts for the number of days specified by network.cookie.lifetime.days.
* **pdfjs.disabled** -&gt; true
  * Disable JavaScript for PDF view

WebRTC settings:

* **media.peerconnection.enabled** -&gt; false
* **media.peerconnection.turn.disable** -&gt; true
* **media.peerconnection.use\_document\_iceservers** -&gt; false
* **media.peerconnection.video.enabled** -&gt; false

#### Firefox Extensions

* uBlock Origin - free and open source ad blocker
* LocalCDN - emulates CDNs to improve online privacy
* Popup Blocker \(strict\) - asks you for permission before you get redirected to another site
* CleanURLs - removes tracking elements from URLs
* CanvasBlocker - Modifies JavaScript to prevent fingerprinting

If you do not want to have the hassle of modifying all of these options, then check out [https://github.com/arkenfox/user.js](https://github.com/arkenfox/user.js). The person who made the repository has made the user profile with all the privacy settings, so all you have to do is download it and upload it as your own profile.

