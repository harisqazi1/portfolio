# Linux Setup

This is just a backup of my setup I have on my Linux OS, in case I ever have to start a Linux OS from scratch.

### Software

| Name                     | Type                                          |
| ------------------------ | --------------------------------------------- |
| DejaDup                  | Backup Creation                               |
| Firefox                  | Browser                                       |
| Discord                  | Communication                                 |
| FreeTube                 | YouTube Privacy-centric Alternative           |
| Htop                     | Process List Viewer                           |
| NVTOP                    | Task Monitor for NVIDIA GPUs                  |
| KeePassXC                | Password Manager (locally hosted)             |
| Bitwarden                | Password Manager (cloud hosted)               |
| Lutris / Heroic Launcher | Run games from Epic Games                     |
| Thunderbird Mail         | Email Client                                  |
| Mullvad VPN / ProtonVPN  | VPN Client                                    |
| LibreOffice              | Microsoft Office Alternative                  |
| OpenRGB                  | Logitech G HUB / Razer Chroma RGB Alternative |
| Piper                    | DPI software for the Mouse                    |
| qBittorrent              | Torrent software                              |
| Steam                    | Gaming Client                                 |
| Variety                  | Wallpaper software                            |
| VeraCrypt                | Encryption software                           |
| Visual Studio Code       | Coding IDE                                    |
| VLC                      | Video Player                                  |
| Agenda                   | To-Do List                                    |
| Calibre                  | E-book management                             |
| Flameshot                | Screen-shot tool                              |
| MComix                   | Comic book viewer                             |
| Visual Studio Code       | Coding IDE                                    |
| Zettlr                   | Open-Source Markdown Editor                   |
| Obsidian                 | Markdown Editor                               |
| Wallch                   | Wallpaper Changer                             |
| Tor                      | Secure and Private web browser                |

### Firefox

* General
  * Uncheck **Restore previous session**
    * If you use a VPN and your connection dies, then when you use Firefox again, it will not spawn the same website, which would give away your data to the website
  * In **Network Settings**, enable **DNS over HTTPS** and choose **Cloudflare**
  * In **Browsing**, uncheck **Recommend extensions as you browse** and **Recommend features as you browse**
* Home
  * Set **Homepage and new windows** to **Blank Page**
  * Set **New tabs** to **Blank Page**
    * These will prevent Firefox from spawning their own sites when you need a new page/tab
  * Uncheck everything under **Firefox Home Content** (Shortcuts, Recommended by Pocket, Recent activity, Snippets)
    * This will prevent Firefox from loading their own content
* Search
  * Change **Default Search Engine** to **DuckDuckGo**
  * Uncheck **Provide Suggestions**
    * Prevents the queries from going to the Google API
* Privacy & Security
  * Under **Cookies and Site Data**, check **Delete cookies and site data when Firefox is closed**
  * Under **Logins and Passwords**, uncheck the boxes (with the inside list first):
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
    * Also uncheck **Suggestions from the web** and **Suggestions from sponsors**
  * Under **Permissions**, click **Settings**... next to **Location, Camera, Microphone,** and **Notifications**. In the popup, check the **"Block new requests.."** box on the bottom.
  * Under **Firefox Data Collection and Use**, uncheck everything
  * Under **Security**, uncheck everything
  * Under **HTTPS-Only Mode**, best practice would be to use **Enable HTTPS-Only Mode in all windows**. However, I use **Enable HTTPS-Only in private windows only**

These are the settings I use when you type in **about:config** in the search bar:

* `geo.enabled` -> false
  * Disables Firefox from sharing your location
* `browser.safebrowsing.malware.enabled` -> false
  * Disables Google's ability to monitor your web traffic for malware
* `dom.battery.enabled` -> false
  * Blocks sending battery level information
* `media.autoplay.default`-> 5
  * Disables audio and video from playing automatically
* `dom.webnotifications.enabled` -> false
  * Disables embedded notifications
* `privacy.resistFingerprinting` -> true
  * Disables some fingerprinting
* `webgl.disabled` -> true
  * Disables some fingerprinting
  * Breaks graphical games and websites
* `network.http.sendRefererHeader` -> 0
  * Disables referring website notifications (**BREAKS SOME SITES)**
  * 1 = Send the Referer header when clicking on a link, and set `document.referrer` for the following page.
  * 2 = Send the Referer header when clicking on a link or loading an image, and set `document.referrer` for the following page. (Default)
  * I use 1 just to be safe
* `identity.fxaccounts.enabled` -> false
  * Disables any embedded Firefox accounts
* `beacon.enabled` -> false
  * Disables data being sent to servers when leaving pages
* `browser.cache.disk.enable` -> false
  * Disk cache is not used by Firefox
* `browser.cache.disk_cache_ssl` -> false
  * Firefox will not cache https website contents.
* `geo.provider.network.url` -> 127.0.0.1
  * The data provider used to power Firefox's geolocation feature
* `network.cookie.lifetimePolicy` -> 2
  * Determines whether Firefox will accept so-called session cookies (removed when browser exits) automatically.
    * 0 = The cookie's lifetime is supplied by the server. (Default)
    * 1 = The user is prompted for the cookie's lifetime.
    * 2 = The cookie expires at the end of the session (when the browser closes).
    * 3 =  The cookie lasts for the number of days specified by network.cookie.lifetime.days.
* `pdfjs.disabled` -> true
  * Disable JavaScript for PDF view

Disable Telemetry Settings:

* `browser.newtabpage.activity-stream.feeds.telemetry` -> false
* `browser.ping-centre.telemetry` -> false
* `browser.tabs.crashReporting.sendReport` -> false
* `devtools.onboarding.telemetry.logged` -> false
* `toolkit.telemetry.enabled` -> false
* `toolkit.telemetry.server` -> \*DELETE URL and leave empty\*
* `toolkit.telemetry.unified` -> false

Disable Pocket:

* `browser.newtabpage.activity-stream.feeds.discoverystreamfeed` -> false
* `browser.newtabpage.activity-stream.feeds.section.topstories` -> false
* `browser.newtabpage.activity-stream.section.highlights.includePocket` -> false
* `browser.newtabpage.activity-stream.showSponsored` -> false
* `extensions.pocket.enabled` -> false

Disable prefetching:

* `network.dns.disablePrefetch`  -> true
* `network.prefetch-next` -> false

Harden SSL preferences:

* `security.ssl3.rsa_des_ede3_sha` -> false (if available)
* `security.ssl.require_safe_negotiation` -> true

WebRTC settings:

* `media.peerconnection.enabled` -> false
* `media.navigator.enabled` -> false
* `media.peerconnection.turn.disable` -> true
* `media.peerconnection.use_document_iceservers` -> false
* `media.peerconnection.video.enabled` -> false

#### Firefox Extensions

* uBlock Origin - free and open source ad blocker
* LocalCDN - emulates CDNs to improve online privacy
* Popup Blocker (strict) - asks you for permission before you get redirected to another site
* CleanURLs - removes tracking elements from URLs
* CanvasBlocker - Modifies JavaScript to prevent fingerprinting
* NoScript Security Suite - gives you control of JavaScript on websites
* Dark Reader - modifies sites to have forced dark mode
* SingleFile - save a webpage with one button
* Chameleon - user-agent changer
* Cookie AutoDelete - deleted cookies when the tab/browser closes
* Decentraleyes - prevents content delivery requests from reaching Google and etc.

If you do not want to have the hassle of modifying all of these options, then check out [https://github.com/arkenfox/user.js](https://github.com/arkenfox/user.js). The person who made the repository has made the user profile with all the privacy settings, so all you have to do is download it and upload it as your own profile.

#### Resources

* [https://blog.increasinglyadequate.com/posts/hardening\_firefox.html](https://blog.increasinglyadequate.com/posts/hardening\_firefox.html)
* [https://ebin.city/\~werwolf/posts/firefox-hardening-guide/](https://ebin.city/\~werwolf/posts/firefox-hardening-guide/)
* [https://www.ghacks.net/overview-firefox-aboutconfig-security-privacy-preferences/](https://www.ghacks.net/overview-firefox-aboutconfig-security-privacy-preferences/)
* [http://kb.mozillazine.org/Network.cookie.lifetimePolicy](http://kb.mozillazine.org/Network.cookie.lifetimePolicy)
* [https://chrisx.xyz/blog/yet-another-firefox-hardening-guide/#why-hardening-firefox](https://chrisx.xyz/blog/yet-another-firefox-hardening-guide/#why-hardening-firefox)
* [https://www.ghacks.net/best-firefox-addons/](https://www.ghacks.net/best-firefox-addons/)
* [http://kb.mozillazine.org/Network.http.sendRefererHeader](http://kb.mozillazine.org/Network.http.sendRefererHeader)

### Gnome Tweaks

Gnome extensions can be found [here](https://extensions.gnome.org). I got a couple of recommendations from [Pop!\_OS](https://support.system76.com/articles/customize-gnome).&#x20;

* Dash To Dock - [https://extensions.gnome.org/extension/307/dash-to-dock/](https://extensions.gnome.org/extension/307/dash-to-dock/)
* Window List - [https://extensions.gnome.org/extension/602/window-list/](https://extensions.gnome.org/extension/602/window-list/)
* Sound Output Device Chooser - [https://extensions.gnome.org/extension/906/sound-output-device-chooser/](https://extensions.gnome.org/extension/906/sound-output-device-chooser/)
* User Themes - [https://extensions.gnome.org/extension/19/user-themes/](https://extensions.gnome.org/extension/19/user-themes/)

I learned that you can also apply themes to Linux. In order to do this, I went to this site, and then did the following:

* Download and extract the .tar.xz file.
* Install [User Themes](https://extensions.gnome.org/extension/19/user-themes) extension
* Move the theme folder to ".themes" in your home directory. (Some themes have more than one folder, so youll have to read the instructions)

In order to change the theme you will have to install gnome-tweaks by running `sudo apt install gnome-tweaks`. To change the theme (in the Tweaks app), go to **Appearance**, and then you can change the theme from **Shell**.

I used the following theme(s):

* Flat Remix GNOME - [https://www.gnome-look.org/p/1013030](https://www.gnome-look.org/p/1013030)
