# Mobile

This is my setup for my current mobile OS: GrapheneOS.

## Installation

I used the web installer, which is located here: [https://grapheneos.org/install/web](https://grapheneos.org/install/web). After 2 tries, and 10 minutes, I was up and running.

## Repositories

The first thing I had downloaded was [F-Droid](https://f-droid.org/). I then added the following repos (which I learned about from [Mental Outlaw's video](https://www.youtube.com/watch?v=MnNm-o0yfZw))(link to doc: [https://forum.f-droid.org/t/known-repositories/721](https://forum.f-droid.org/t/known-repositories/721)):

| Name             | Repo                                                                                                                               |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| Bitwarden        | https://mobileapp.bitwarden.com/fdroid/repo?fingerprint=BC54EA6FD1CD5175BCCCC47C561C5726E1C3ED7E686B6DB4B18BAC843A3EFE6C           |
| Bromite          | https://fdroid.bromite.org/fdroid/repo?fingerprint=E1EE5CD076D7B0DC84CB2B45FB78B86DF2EB39A3B6C56BA3DC292A5E0C3B9504                |
| Collabora Office | https://www.collaboraoffice.com/downloads/fdroid/repo?fingerprint=573258C84E149B5F4D9299E7434B2B69A8410372921D4AE586BA91EC767892CC |
| IzzyOnDroid      | https://apt.izzysoft.de/fdroid/repo?fingerprint=3BF0D6ABFEAE2F401707B6D966BE743BF0EEE49C2561B9BA39073711F628937A                   |
| Cryptomator      | https://static.cryptomator.org/android/fdroid/repo?fingerprint=F7C3EC3B0D588D3CB52983E9EB1A7421C93D4339A286398E71D7B651E8D8ECDD    |
| Molly            | https://molly.im/fdroid/repo/?fingerprint=3B7E93B1FE32C6E35A93D6DDFC5AFBEB1239A7C6EA6AF20FF33ED53CDC38B04A                         |
| NewPipe          | https://archive.newpipe.net/fdroid/repo?fingerprint=E2402C78F9B97C6C89E97DB914A2751FDA1D02FE2039CC0897A462BDB57E7501               |

## Apps

| App                                | Description                        | Source       |
| ---------------------------------- | ---------------------------------- | ------------ |
| Aegis                              | 2FA app                            | F-Droid      |
| Aurora Store                       | Google Playstore Client            | F-Droid      |
| AntennaPod                         | Podcast App                        | F-Droid      |
| Bitwarden                          | Password Manager                   | F-Droid      |
| Bluesky                            | Social Media                       | Aurora Store |
| Clima                              | Weather                            | F-Droid      |
| Clipeus                            | Clipboard Cleaner                  | F-Droid      |
| Compass                            | Compass                            | F-Droid      |
| Cryptomator                        | Encryption for files ($10 license) | F-Droid      |
| Feeder                             | FOSS Feed Reader                   | F-Droid      |
| FlorisBoard                        | Keyboard w/ glide                  | F-Droid      |
| K-9 Mail                           | Email Client                       | F-Droid      |
| KeePassDX                          | Password manager for KeePass files | F-Droid      |
| Kiwix                              | Offline Educational Content        | F-Droid      |
| OsmAnd+                            | Map and Navigation                 | F-Droid      |
| Metro                              | Music Player                       | F-Droid      |
| MuPDF viewer                       | PDF viewer                         | F-Droid      |
| Molly                              | Signal Fork                        | F-Droid      |
| Tubular                            | YouTube Client                     | F-Droid      |
| Orbot                              | Tor Proxy                          | F-Droid      |
| Proton Mail                        | Encrypted Mail                     | F-Droid      |
| Record You                         | Voice Recording                    | F-Droid      |
| Seeneva                            | Comic Book Reader                  | F-Droid      |
| SimpleLogin                        | Email Alias Manager                | F-Droid      |
| Tasks.org                          | To-Do List                         | F-Droid      |
| Cheogram                           | XMPP Client                        | F-Droid      |
| Authy                              | 2FA                                | Aurora Store |
| Gboard (No Network access)         | Google Keyboard                    | Aurora Store |
| Microsoft Lens (No Network access) | PDF Scanner                        | Aurora Store |
| Pay by Privacy.com                 | Virtual Credit Cards               | Aurora Store |
| Signal                             | Encrypted Chat                     | Aurora Store |
| Magic Earth                        | Navigation & Map                   | Aurora Store |
| NFC Tools                          | NFC Management                     | Aurora Store |
| Twilio Authy Authenticator         | 2FA app                            | Aurora Store |

## Settings

### Missing Apps within Aurora Store:

Apps -> Default Apps -> Opening Links -> Aurora Store -> Add link -> choose both links

### Wi-Fi:

Network & Internet -> Internet -> Network Preferences -> Turn off Wi-Fi Automatically -> 1 minute

### DNS:

Network & Internet -> Private DNS -> Private DNS provider hostname (set to DNS Provider)

### **Battery Drain:**

Apps -> \*In each app\* -> Notifications -> \*disable\*

Apps -> \*In each app\* -> Battery -> Restricted

### **Permissions:**

Privacy -> Permission Manager -> \*modify per app basis\*

### **Emergency Alerts:**

Safety & emergency -> Wireless emergency alerts -> \*disable everything\*

### **Sounds:**

Sound & vibration -> Phone ringtone -> None

Sound & vibration -> Default notification sound -> None

Sound & vibration -> \*disable\* Screen locking sound

Sound & vibration -> \*disable\* Charging sounds and vibration

Sound & vibration -> \*disable\* touch sounds

### **Disable every installed app from being displayed on the home screen:**

\*hold onto home screen\* -> Home settings -> \*disable\* Add app icons to home screen

### **Misc:**

Security -> \*disable\* "Enable native code debugging" and "Screen lock camera access"

Settings -> Display -> Lock screen -> \*disable\* Lift to check phone&#x20;

Settings -> Display -> \*disable\* Adaptive brightness
