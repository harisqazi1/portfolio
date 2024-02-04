# Dark Waters

## Background

Prior to working on Kase Scenarios' Dark Water (located at: [https://courses.kasescenarios.com/courses/dark-waters](https://courses.kasescenarios.com/courses/dark-waters)), I have only done a couple TryHackMe OSINT challenges and participated in a couple Trace Labs events. Since I had access to this due to being chosen for the beta, I will be only going over the questions and how I get to the answers. This way, I am not spoiling it for others and give them chance to solve it themselves while following my steps. Plus Rae and Zewen have worked hard on the animations and visuals, so it would feel like a robbery if I took a snippet of everything and posted all of it here. **No answers are in this blog. Only steps I took to get to the answer.**

### Time Taken to Complete: \~18 hours

## Review

I honestly really enjoyed the challenge. I was looking for a treasure hunt type of CTF before, and this checked that box. There were many things I learned during this CTF. I can't list them here otherwise it would spoil some answers and techniques. There were some questions that I was just really stuck on. I'm glad that Zewen and Rae were there to nudge in the right direction, otherwise I would have been stuck forever.

#### Pros

* I got to experience various OSINT tools and techniques
* I learned about new topics in OSINT that I had never heard about before
* The questions were challenging to actually make my brain work
* I learned how to use some tools in a different manner
* Visuals were clean, and it looked professionally done

#### Cons

* Some hints were just bruuutal. I overthink things and I was burned out. One question I had solved over the span of 3 days since I wanted to get a fresh perspective each time.
* I felt like in order to make some of the questions harder, there was a layer of abstraction between the actual plot and the question. This could have been me spending too much time doing the questions. Every time I came back to the plot after the questions, I was like "oh yea, this was happening"
* I couldn't work over a VPN. Not really a Kase issue, but more of a Thinkific issue.

## Glen Rock

The scenario starts off by introducing the case, the main character Alec Wolfe, and his handler George. He gets a strange letter in the mail, and his handler recommends he check out the location on the envelope. Alec is then "seen" driving into town. It was dash-cam footage of Alec driving to town. I viewed the video and paused it where I was able to view the name of a local building.

<figure><img src="../../.gitbook/assets/image (21) (3) (1).png" alt=""><figcaption></figcaption></figure>

I then popped that name into Google Maps as `Peoples Rock Glen Rock`. Before pressing enter, Google Maps had already given me a suggestion to the name of the location. After dropping the "Google Maps yellow human" on the street in front of the building, I was where the video had ended.

<figure><img src="../../.gitbook/assets/image (5) (5).png" alt=""><figcaption></figcaption></figure>

Google Maps Street View:

<figure><img src="../../.gitbook/assets/image (6) (5).png" alt=""><figcaption></figcaption></figure>

Now I knew I was in the right place. Alec then mentions he is headed to a bar. My task now was to see the closest bar to the location I had found on Google Maps. I had seemed to find one:

<figure><img src="../../.gitbook/assets/image (13) (5).png" alt=""><figcaption></figcaption></figure>

The bartender offers Alec a room on the floor above the bar. Alec mentions that he will take the offer. Alec notices a woman across the street, and in discussion with her she explains that she was threatened by the Glenn Rock Paper Company to stop digging into Glen Rock and its corruption. She mentions she was being threatened on social media. This gave me a hint that I might have to look on various social media websites to find information about who this person could be. I looked a head and saw the first question:

### Question #1

<figure><img src="../../.gitbook/assets/image (9) (1) (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

I ended up finding a website, while searching `glen rock 1837 history land` on Google, which had shown me the answer.

### Question #2

<figure><img src="../../.gitbook/assets/image (3) (5) (1) (1).png" alt=""><figcaption></figcaption></figure>

This one was easier since I had Google Maps from before to refer to. Well, I was looking around and it turned out to be a bust. I ended up "Googling" `glen rock pa railroad` and found the answer.

### Question #3

<figure><img src="../../.gitbook/assets/image (4) (1) (3) (1).png" alt=""><figcaption></figcaption></figure>

For this one, I started with Google with a search of `glen rock pennsylvania library`. This resulted with me finding the following:

<figure><img src="../../.gitbook/assets/image (18) (5) (1).png" alt=""><figcaption></figcaption></figure>

This then led me search this address in Google Maps:

<figure><img src="../../.gitbook/assets/image (11) (1) (3).png" alt=""><figcaption></figcaption></figure>

These trees were in my way. I then looked on Google Earth to see if there a better angle there:

<figure><img src="../../.gitbook/assets/image (27) (1) (2).png" alt=""><figcaption></figcaption></figure>

Nope, this was a dead-end as well. Going back to Google Images, I see the following:

<figure><img src="../../.gitbook/assets/image (20) (3) (1).png" alt=""><figcaption></figcaption></figure>

I have to be in the wrong place, since the images on Google Maps do not look like this. I looked on multiple websites, and I keep getting back the same address:

<figure><img src="../../.gitbook/assets/image (16) (3) (1).png" alt=""><figcaption></figcaption></figure>

Looking at an image on Google, I think I am at the wrong place (yes, I know the picture is really blurry):

<figure><img src="../../.gitbook/assets/image (12) (1) (3).png" alt=""><figcaption></figcaption></figure>

There is a railroad across from the library. I went back to Google Maps, dropped the "yellow guy" next to the railroad, and went on a Google Maps road trip until I ended up here:

<figure><img src="../../.gitbook/assets/image (6) (1) (4).png" alt=""><figcaption></figcaption></figure>

Looks like the right place I think. Also shout-out to "charlie naurath" who uploaded these images. Got me the angle I am currently looking at. Looking from another angle I was able to see what was depicted. From there I was able to guess the answer correctly.

### Question #4

<figure><img src="../../.gitbook/assets/image (17) (5) (1).png" alt=""><figcaption></figcaption></figure>

This should, I assume, be an easy Google search. I ended up finding this website, which had the answer right after I searched on Google.

### Question #5

<figure><img src="../../.gitbook/assets/image (28) (2).png" alt=""><figcaption></figcaption></figure>

Similar to the previous question, this one was also straight forward.

### Question #6

<figure><img src="../../.gitbook/assets/image (26) (2).png" alt=""><figcaption></figcaption></figure>

For this question, I took a screenshot of the image. I then uploaded that to Google and I was able to see the answers in the results.

### Question #7

<figure><img src="../../.gitbook/assets/image (1) (6) (1).png" alt=""><figcaption></figcaption></figure>

As always, started off with Google. I did multiple searches for this one. The following got me the result I was looking for. Query is blurred as it is the answer to a question.

<figure><img src="../../.gitbook/assets/image (19) (3) (2).png" alt=""><figcaption></figcaption></figure>

I didn't find any answers on any websites, but I thought to see maybe if they posted something on Facebook. I scrolled down a long while and then found this:

<figure><img src="../../.gitbook/assets/image (7) (1) (3).png" alt=""><figcaption></figcaption></figure>

I needed to find the title for this event. I then looked at earlier posts:

<figure><img src="../../.gitbook/assets/image (10) (1) (3) (1).png" alt=""><figcaption></figcaption></figure>

After trying multiple answers, the image above helped me get the answer.

### Question #8

<figure><img src="../../.gitbook/assets/image (2) (5) (1) (1).png" alt=""><figcaption></figcaption></figure>

The previous question helped with this.

### Question #9

<figure><img src="../../.gitbook/assets/image (15) (3).png" alt=""><figcaption></figcaption></figure>

Started once again at Google:

<figure><img src="../../.gitbook/assets/image (25) (2) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (24) (1) (2).png" alt=""><figcaption></figcaption></figure>

I did not have the zip code. However, that was one search away:

<figure><img src="../../.gitbook/assets/image (23) (2).png" alt=""><figcaption></figcaption></figure>

Entering that information in got me the answer. But this was in Farenheit and not Celcius as specified by the question. I then looked online to convert it. Luckily DuckDuckGo had a module that easily converted the number. I got the answer by rounding down to the next whole number.

## Party Vibes

<figure><img src="../../.gitbook/assets/image (8) (1) (2).png" alt=""><figcaption></figcaption></figure>

### Question #1

<figure><img src="../../.gitbook/assets/image (14) (3).png" alt=""><figcaption></figcaption></figure>

For this one, I first downloaded the flyer above. Usually for big corporate companies, they usually strip metadata from a file when it gets uploaded online. However, I had a hunch that there is a chance they might have not for this scenario. I uploaded the file to an online metadata reader and got the answer in one of the fields.

### Question #2

<figure><img src="../../.gitbook/assets/image (22) (3).png" alt=""><figcaption></figcaption></figure>

One of the first things shown in the case was a video in the section titled "GRPC: The Company That Cares About Nature". In the video at the end, you see a link:

<figure><img src="../../.gitbook/assets/image (5) (1) (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

Going to this site, and scrolling down a bit, shows the following:

<figure><img src="../../.gitbook/assets/image (43) (2).png" alt=""><figcaption></figcaption></figure>

The answer was in the last line of her bio.

### Question #3

<figure><img src="../../.gitbook/assets/image (41) (2).png" alt=""><figcaption></figcaption></figure>

Now this one kind of threw me off a bit. They only thing I know about her is that she made the flyer and she worked at Glen Rock for 2 years. She also loves `redacted`, which means she loves being indoors? (not too sure about that). My first thought was to hit up Wigle (https://wigle.net/) to see if I can try to find the SSID by using part of her name and knowing she might live in Glen Rock. To do this, I found Glen Rock on the Wigle map:

<figure><img src="../../.gitbook/assets/image (18) (4).png" alt=""><figcaption></figcaption></figure>

Now my job was to try to find the SSID. Searching for `redacted name` in the vicinity of Glen Rock, showed me the following:

<figure><img src="../../.gitbook/assets/image (8) (1) (3).png" alt=""><figcaption></figcaption></figure>

None of those worked. I then realized that the SSID is actually the display name of the Access Point. I entered in different variations of the name, based off the results above, but was still rejected. I was lost for a while. It then hit me. Why not use the wildcard feature in Wigle? I tried it with what she likes doing:

<figure><img src="../../.gitbook/assets/image (130).png" alt=""><figcaption></figcaption></figure>

This location happened to be exactly in Glen Rock! I was then able to submit the SSID and get it correct.

### Question #4

<figure><img src="../../.gitbook/assets/image (13) (1) (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

This is where I can use the hexadecimal string from the previous answer.

### Question #5

<figure><img src="../../.gitbook/assets/image (1) (1) (3) (1).png" alt=""><figcaption></figcaption></figure>

Looking at the data in Wigle, I did see a date. That was the answer.

### Question #6

<figure><img src="../../.gitbook/assets/image (3) (1) (3).png" alt=""><figcaption></figcaption></figure>

We are still working off of the data from Wigle here as well. If I am not mistaken, the first 6 characters in a MAC Address or even a BSSID can tell you what is the vendor. The last 6 are specific to the device itself. I believe this information can be found with an online OUI lookup tool. Searching for that on https://ouilookup.com/ gave me the answer.

### Question #7

<figure><img src="../../.gitbook/assets/image (20) (1) (3).png" alt=""><figcaption></figcaption></figure>

I searched on Google for the company name and the parent company was shown as well.

## The Campaign

Alec goes back to the bar, and has a conversation with some individuals. This was about to break into a fight, but Lisa comes through and prevents it from happening. The next morning, Lisa talks to Alec and states that the room he had slept in last night (above the bar), was actually her brother Peter's room. Peter's daughter had died due to pneumonia. Peter believed that this was due to the Glen Rock Paper Plant. He was eventually fired from the plant, and then he believed he was being followed. He sent a voicemail to Lisa, and after a couple of days they had found him dead in his car. Alec shows Lisa the envelope he brought with him, which might have been from Peter. Lisa says her whole family works at the plant, so she cannot help him with this. However, she does say she left some documents downstairs that was from the detective who had worked on the case of her brother. This was meant to assist Alec in a way on his own investigation.

We then hear the voicemail from Peter. Using [https://www.veed.io/](https://www.veed.io/), I was able to turn it into text (for your viewing pleasure):

<figure><img src="../../.gitbook/assets/image (23) (1) (1).png" alt=""><figcaption></figcaption></figure>

From hearing this, it seemed that there was something maybe hidden in the bar. Alec then mentions that he plans on going to the Glen Rock Paper Centennial Party to check out the place.

Alec is waiting in line to get into the party, and he overhears some guards talking about Cryptocurrencies. After looking at Alec, they tell him that he is not invited into the party.

While doing recon, Alec sees the Glen Rock Paper Company's CEO's son (yes, a mouthful) shaking hands with a local politician, Alexander Ross. He also spots an owner of a local paper as well there.

George then texts his handler, George Hammond, that the guards did not let him in.

### Question #1

<figure><img src="../../.gitbook/assets/image (14) (1) (2).png" alt=""><figcaption></figcaption></figure>

I did see some "money talk" in a document on the Kase Portal website:

<figure><img src="../../.gitbook/assets/image (17) (4).png" alt=""><figcaption></figcaption></figure>

Going to the link, I went to this page:

<figure><img src="../../.gitbook/assets/image (42) (2).png" alt=""><figcaption></figcaption></figure>

I was basically going back and forth on different places, and realized the answer was on the main page:

<figure><img src="../../.gitbook/assets/image (15) (1) (3) (1).png" alt=""><figcaption></figcaption></figure>

### Question #2

<figure><img src="../../.gitbook/assets/image (40) (2).png" alt=""><figcaption></figcaption></figure>

I was able to find this at the bottom of the same page:

<figure><img src="../../.gitbook/assets/image (2) (1) (4) (1).png" alt=""><figcaption></figcaption></figure>

### Question #3

<figure><img src="../../.gitbook/assets/image (3) (2) (3) (1).png" alt=""><figcaption></figcaption></figure>

We go to the document located at:

<figure><img src="../../.gitbook/assets/image (4) (2) (2).png" alt=""><figcaption></figcaption></figure>

I got nothing searching for `PR`. But searching for `Public` got me the following:

<figure><img src="../../.gitbook/assets/image (5) (2) (2).png" alt=""><figcaption></figcaption></figure>

With that, another section done.

## Murky Waters

Alec calls the lab to see if they have any results for the sample of water he had sent over. The lab tells him that he had sent them an email to cancel the test. Alec tells the lab that he did not do this, and to run the test.

This is the email that was sent to the lab:

<figure><img src="../../.gitbook/assets/image (133).png" alt=""><figcaption></figcaption></figure>

Alec wants to find out who is behind the email.

### Question #1

<figure><img src="../../.gitbook/assets/image (136).png" alt=""><figcaption></figcaption></figure>

I got nowhere just searching for the address. Then I realized this person might be using this username somewhere else as well. I used [https://whatsmyname.app/](https://whatsmyname.app/) to get me the following information:

<figure><img src="../../.gitbook/assets/image (131).png" alt=""><figcaption></figcaption></figure>

There is a Reddit and olx account with the same username. I went to Reddit and saw this:

<figure><img src="../../.gitbook/assets/image (23) (1).png" alt=""><figcaption></figcaption></figure>

Going to Steam, I saw the following:

<figure><img src="../../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

Now we have more usernames to enumerate. I was trying to enumerate, and ended up not getting too much to work off of. I then used [https://www.steamidfinder.com/](https://www.steamidfinder.com/), and saw the following information:

<figure><img src="../../.gitbook/assets/image (25) (3).png" alt=""><figcaption></figcaption></figure>

We at least have a first name to go off of. I logged into my own account and saw the same information:

<figure><img src="../../.gitbook/assets/image (19) (3).png" alt=""><figcaption></figcaption></figure>

After a looooong time, and a nudge from zewen, I realized I had to look on some social media to find this person. I was eventually able to find them on Facebook:

<figure><img src="../../.gitbook/assets/image (126).png" alt=""><figcaption></figcaption></figure>

### Question #2

<figure><img src="../../.gitbook/assets/image (127).png" alt=""><figcaption></figcaption></figure>

Looking at the profile, I was able to find this information:

<figure><img src="../../.gitbook/assets/image (13) (6).png" alt=""><figcaption></figcaption></figure>

### Question #3

<figure><img src="../../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>

Same situation for this as the previous question:

<figure><img src="../../.gitbook/assets/image (135).png" alt=""><figcaption></figcaption></figure>

With this, I had completed this section.

## Missed Calls

Alec finds out that the worker `redacted name` is related to the lab technician he had talked to on the phone.

We are then given an audio clip, and its transcription, where George Hammond (Alec's handler) mentions that Glen Rock Paper Company had reached out to him to prevent George from publishing this information. George also directs him in the direction of a Reddit post to help Alec on his investigation. I ended up finding the sub-reddit first, and it seemed to only have two posts:

<figure><img src="../../.gitbook/assets/image (12) (4).png" alt=""><figcaption></figcaption></figure>

The post George was referring to was the first one. The imgur link leads to the following:

<figure><img src="../../.gitbook/assets/image (132).png" alt=""><figcaption></figcaption></figure>

The quality of the picture was kind of blurry, in my opinion. I used a tool I had learned about recently called Upscayl (https://upscayl.github.io/). You can see the difference it makes (left is without, and right is with):

<figure><img src="../../.gitbook/assets/image (28) (3).png" alt=""><figcaption></figcaption></figure>

I then needed to find out which language or encoding this was. I was looking around for a long time. I plugged it into Google in different formats to see if I would have any luck. I ended up finding the font out using this method. This translated to a location.

Alec then talks to himself and realizes that cache could mean that people have a secret way of communicating with each other.

### Question #1

<figure><img src="../../.gitbook/assets/image (22) (4) (1).png" alt=""><figcaption></figcaption></figure>

I thought the name of the cache was the name of the font. Apparently that was incorrect. Even with the hint, I was still lost. I think the phrasing of this one is throwing me off more than the question itself. After looking around, I ended up finding a website where you can actually search for geocaching devices:

<figure><img src="../../.gitbook/assets/image (117).png" alt=""><figcaption></figcaption></figure>

This was the name of the cache. In addition, the username was similar to another username we had seen before.

### Question #2

<figure><img src="../../.gitbook/assets/image (114).png" alt=""><figcaption></figcaption></figure>

This was one easier as we already had it from before. Clinking on the link to the cache, I was able to get the right format with the degree symbol.

<figure><img src="../../.gitbook/assets/image (11) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Question #3

<figure><img src="../../.gitbook/assets/image (124).png" alt=""><figcaption></figcaption></figure>

We see this "encoding" or cipher here:

<figure><img src="../../.gitbook/assets/image (18) (5).png" alt=""><figcaption></figcaption></figure>

This seems to be as ROT13, or rotation 13 where each letter is replaced with its value 13 characters apart in the alphabet. But entering `ROT13` was wrong. Maybe `ceasar cipher`?

Sooo.. I misunderstood this question. I thought this was for the "Additional Hints" but the question was being asked about what we solved above in the cryptic message. The answer was the font as we had found earlier.

With this, another section done.

## Coordinated Excursion

Alec decides to chase down the cache. In the cache, Alec finds the following string:

<figure><img src="../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

Using [https://www.dcode.fr/cipher-identifier](https://www.dcode.fr/cipher-identifier), I was able to find out this was potentially ROT13. I then decoded it using [https://www.dcode.fr/rot-13-cipher](https://www.dcode.fr/rot-13-cipher):

<figure><img src="../../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>

### Question #1

<figure><img src="../../.gitbook/assets/image (125).png" alt=""><figcaption></figcaption></figure>

We just solved this question above.

### Question #2

<figure><img src="../../.gitbook/assets/image (27) (1).png" alt=""><figcaption></figcaption></figure>

Searching this online led to a government organization's site, which had the answer.

### Question #3

<figure><img src="../../.gitbook/assets/image (20) (3).png" alt=""><figcaption></figcaption></figure>

This supposed answer was on the same site as well:

<figure><img src="../../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

Apparently not. I then looked around for a while online trying to figure out what it could be. I even read some documentation on the vehicle. This did not help me too much either. After a while I finally got it. The type was actually the "Model" of the vehicle.

With that question done, another section done.

## Data Analysis

Alec reaches out to his handler George to ask him to grab flight data for the plane on Jan 8, 2023. This was provided to us (the player) in the same section. My assumption is that most "tracking" services charge for histories of different types of crafts. So shout-out to the Kase Staff for coming through with this info. The file was of `.kml` type. I needed to find an online reader for this. I ended up finding [https://www.gpsvisualizer.com/map?output\_home](https://www.gpsvisualizer.com/map?output\_home), uploaded the file to it, and saw this:

<figure><img src="../../.gitbook/assets/image (134).png" alt=""><figcaption></figcaption></figure>

From the looks of it, it looks like a joy trip, like someone is trying to get to see around the town and its main locations.

### Question #1

<figure><img src="../../.gitbook/assets/image (10) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

The file names were not cleaned when exported from FlightAware.

<figure><img src="../../.gitbook/assets/image (15) (1) (3).png" alt=""><figcaption></figcaption></figure>

So my guess what I can brute force this in a way, but using the airport "code" to find out which one it was. I was able to search for it on FlightAware: [https://flightaware.com/live/airport/KFDK](https://flightaware.com/live/airport/KFDK).

<figure><img src="../../.gitbook/assets/image (116).png" alt=""><figcaption></figcaption></figure>

....this did not work. Jokes on me for the quick trick. I then went to the website where I had uploaded the file above ([https://www.gpsvisualizer.com/map?output\_home](https://www.gpsvisualizer.com/map?output\_home)) and was able to zoom in and see it:

<figure><img src="../../.gitbook/assets/image (29) (2) (1).png" alt=""><figcaption></figcaption></figure>

This worked for me.

### Question #2

<figure><img src="../../.gitbook/assets/image (21) (1) (3).png" alt=""><figcaption></figcaption></figure>

I believe the same website where I uploaded the .kml file can help us here as well.

<figure><img src="../../.gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

With this, another section done.

## Phony Threats

Alec gets a threatening phone call to tell him to stop investigating. He thinks about investigating the number further.

### Question #1

<figure><img src="../../.gitbook/assets/image (118).png" alt=""><figcaption></figcaption></figure>

Searching on [https://calleridtest.com](https://calleridtest.com) I see the following:

<figure><img src="../../.gitbook/assets/image (115) (2).png" alt=""><figcaption></figcaption></figure>

I got some more information from another site: [https://numpi.com](https://numpi.com).

<figure><img src="../../.gitbook/assets/image (14) (4).png" alt=""><figcaption></figcaption></figure>

I also saw this on [https://www.phonevalidator.com](https://www.phonevalidator.com):

<figure><img src="../../.gitbook/assets/image (112).png" alt=""><figcaption></figcaption></figure>

Maybe this is a Google Voice number. I was lost. It then hit me, why not look on social media. I checked on Reddit. Nothing. Instagram. Nothing. I then found this on Facebook:

<figure><img src="../../.gitbook/assets/image (17) (5).png" alt=""><figcaption></figcaption></figure>

Based on this information, you can get to the answer.

With that, another section done.

## The Devil Is In The Pixels

Alec gets the lab results back and notices high levels in the results. Alec also has a voicemail left from Lisa, who sends a picture and the back of the image. The back of the image seems to have a username attached to it and a drawing of a ship:

<figure><img src="../../.gitbook/assets/image (119).png" alt=""><figcaption></figcaption></figure>

The image itself is the following. I am showing it for context (trimmed):

<figure><img src="../../.gitbook/assets/image (121).png" alt=""><figcaption></figcaption></figure>

In the back, I see numbers upside down. Right one is a `4`. Left one is a `6`. The middle one seems to be a `0`. This could be a year or could be the sailor's group number. I do take the username and go hunting online but do not end up finding anything useful in my first search. I then check out what could be the boat or ship. Looking online for `ship 406` I see the following:

<figure><img src="../../.gitbook/assets/image (129).png" alt=""><figcaption></figcaption></figure>

Looking at the guy in the picture, my estimate is that he looks like he is German. I think this has something to do with a German sub. Hear me out. The username is `Hoagie215`. Hoagie rolls, if I am not mistaken, used for submarine sandwiches as seen here:

<figure><img src="../../.gitbook/assets/image (123).png" alt=""><figcaption></figcaption></figure>

Bringing all the clues together. I think there is a German submarine that had sank (due to the drawing of the ship seeming to be "broken" on one side) and the number of the ship is 215. This could just be a big rabbit hole I had gone down.

### Question #1

<figure><img src="../../.gitbook/assets/image (122).png" alt=""><figcaption></figcaption></figure>

This question kind of threw me back. I had not seen this anywhere in my searching for the boat. I continued to do more research. I came across this:

<figure><img src="../../.gitbook/assets/image (24) (1).png" alt=""><figcaption></figcaption></figure>

Maybe I am onto something.

Nope. I guess I was not. I went back and looked at the hint `Some prefer the road, some prefer the forest. Others like the open sea.` I took the term "open sea" to mean like the actual ocean, but then I realized that there is a crypto website called OpenSea. It has NFT's for sale. Crypto addresses do start with a "0x". I searched there for a user called `Hoagie215`, and I found one, which led me to the answer.

### Question #2

<figure><img src="../../.gitbook/assets/image (2) (5) (1).png" alt=""><figcaption></figcaption></figure>

and the hint:

<figure><img src="../../.gitbook/assets/image (5) (1) (3) (1).png" alt=""><figcaption></figcaption></figure>

I keep looking closer and for some reason I am missing the solution. I even inverted the image to see if maybe it was hidden somewhere:

<figure><img src="../../.gitbook/assets/image (4) (1) (3).png" alt=""><figcaption></figcaption></figure>

It did not seem to be there either. I believe it has something to do with the eyes. But I keep going down rabbit holes. I think I spent like 4 hours on this question alone. I was just goofing on Pixlr X and I ended up finding what was written in the image:

<figure><img src="../../.gitbook/assets/image (9) (1) (3) (1).png" alt=""><figcaption></figcaption></figure>

That ended up being the flag.

### Question #3

<figure><img src="../../.gitbook/assets/image (2) (1) (4) (2).png" alt=""><figcaption></figcaption></figure>

This is about to be another menace of a question. I then went to the second image to see if I can get the result the same way.

<figure><img src="../../.gitbook/assets/image (3) (1) (1) (3) (1).png" alt=""><figcaption></figcaption></figure>

Well, this one took 1 minute to do.

### Question #4

<figure><img src="../../.gitbook/assets/image (1) (1) (3).png" alt=""><figcaption></figcaption></figure>

Maybe I can get lucky again? This one I had to play around with the Hue and Tint:

<figure><img src="../../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

And with that, the section was complete.

## End Of The Puzzle

Alec concludes that the Glen Rock Paper company is getting shipments at the Boston port. The ships name seems to be the `redacted`. We did get "information" which I believe might be the port of Boston?

<figure><img src="../../.gitbook/assets/image (7) (1) (1).png" alt=""><figcaption></figcaption></figure>

Nope, this seems to be in New York. This could be the main building of where the "bad dealings" are going on. After this, Alec wraps up the case, and this information gets made into articles about Glen Rock Paper Company, where the company gets exposed.
