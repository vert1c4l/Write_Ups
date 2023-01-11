# Challenge
From: https://twitter.com/OSINTDojo/status/1556630457852121089/photo/1
For this week's #OSINT challenge, see if you can answer the following questions about this unusual User Agent:

![image](https://user-images.githubusercontent.com/101227395/183458501-da030824-e36e-46f1-ac15-b2dc47d8b2e7.png)

1. What Web Browser did the user use?
2. What Operating System and OS version did they use?
3. What is notable about this OS?

**Note: Things may not be as they appear **😉

# Step 1 - Key Info
Before jumping into doing research right away with a search engine of choice, I take a moment to identify what key pieces of information have been provided.  Especially with the cryptic note from the Dojo, this won't be as simple as it may appear.

![OSINTDOJO-20220808-Pivots](https://user-images.githubusercontent.com/101227395/183461320-30abc066-8724-44c8-8784-1a7abf196b04.jpeg)

These are the unique pieces of information listed in the picture:
- Fedora Linux
- Naenara 3.5b4
- Gecko layout engine
- 20130508 -> 08 May 2013, the layout engine version
- X11 Window System
- i686 hardware

# Step 2 - Research
Starting with the Naenara pivot, since the first question is about the Web Browser, I use Google to query _Naenara 3.5b4_.

The first result returns a hit to a blog from White Hat Security that talks about the Naenara Web Browser, which is North Korea's verison of Firefox.  https://www.whitehatsec.com/blog/north-koreas-naenara-web-browser-its-weirder-than-we-thought/

This blog gives me a LOT of juicy information, like some additional info its quirks, its configuration, and of course, the fact that it is used in the Red Star OS.

Going further down the list of results, I find some amplifying information to flesh out the 2nd question asked.  I know that the OS is Red Star, but which version is it?  I get my initial answer from here: https://robert.hawdon.net/2016/06/25/red-star-os-3-0-north-koreas-custom-linux-distribution/, which lists the Red Star version as 3.0.

HOWEVER, I still need to verify some infomration.  Is it actually Red Star 3.0 or could it be from another version?  

Another result from my initial query returns a hit from a github post: https://thadafinser.github.io/UserAgentParserComparison/v5/user-agent-detail/67/18/67188569-9293-4a76-927b-4162b66345c9.html, which matches a lot of the details from the initial image provided.  i686, Fedora, Gecko/20130508, and NaenaraBrowser/3.5b4

# Step 3 - Notable Noteriety
So oother than being an Operating System used by **Most Best Korea**, is there anything else that stands out about it?  Of course there is.  Without going too deep into the details, let's list just a couple of highlights.
- It watermarks different files in order to track the distrobution of documents and media files via USB Stick (https://fahrplan.events.ccc.de/congress/2015/Fahrplan/events/7174.html)
- Users cannot make changes to the OS.  Instead, they get an error or the system reboots (https://www.theguardian.com/world/2015/dec/27/north-koreas-computer-operating-system-revealed-by-researchers)
- For some reason, there is a subreddit dedicated to the Red Star OS.  Actually, this is the internet, so of course there is (https://www.reddit.com/r/RedStarOS/)
- Its User Interface is remarkably similar to that of the Mac OS X UI (https://robert.hawdon.net/blog/wp-content/uploads/2016/06/VirtualBox_Red_Star_3_25_06_2016_15_20_53.png)
- It can probably run Steam (https://gamelust.com/news/investigation-steam-account-north-korea/, https://nerfwire.com/gamer-girl-win-kim-yo-jong-to-be-first-north-korean-woman-with-a-steam-account/)

And the list could go on and on.

# Final Answers
1. **What Web Browser did the user use?**  Naenara 3.5b4, a North Korean version of the Firefox 3.5b4 browser
2. **What Operating System and OS version did they use?**  Red Star 3.0, a Linux distro based on Red Hat's Fedora
3. **What is notable about this OS?**  So, so, so many things.

![image](https://user-images.githubusercontent.com/101227395/183468402-4540fc85-6d9b-41b8-bdba-5e87a7cf7ab6.png)
