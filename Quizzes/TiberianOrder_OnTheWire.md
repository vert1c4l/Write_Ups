This is my first write up for a contract from [Tiberian Order](https://tiberianorder.com/), a site that publishes challenges that cover all areas of Cybersecurity.  Each challenge results in you, the Agent, finding a Contract Card.  You'll have to use a wide array of skills to solve the different challenges published by your secretive order of Agents.

# [On The Wire Challenge](https://tiberianorder.com/contracts/on-the-wire/)
![Bruce](https://tiberianorder.com/wp-content/uploads/2022/09/On-the-Wire-758x426.jpg)

**Published:** 7 September 2022

**Difficulty:** Medium

**Task:** Given the following [PCAP](https://tiberianorder.com/wp-content/contracts/items/on-the-wire.zip), find the unusual way that a coorupt politician has been communicating with their handlers.

**Initial Observations:**  Given the name of the contract and the image of a shark (whom I've lovingly named Bruce in honor of the movie *Jaws*), it is highly likely that this will be a network forensics challenge.  The packet capture (or pcap, for short) file provided by The Order confirms this.

---

# Step 1 - Wading the Waters

After unzipping the file provided with the challenge, I open the pcap file with [Wireshark](https://www.wireshark.org/).  As stated on their site, *Wireshark is a network protocol analyzer. It lets you capture and interactively browse the traffic running on a computer network.*

The on-the-wire.pcapng file has captured 18943 packets, so this will be like trying to find a needle in a stack of nails.  But every investigation has to start somewhere, so I start by looking at the Hyper Text Transfer Protocol (HTTP) traffic, as this is usually in plain text (not encrypted).

Using the filter bar, I enter `http` and reduce the amount of packets down to 13.  If I'm lucky, the answer will be in one of these packets.

![image](https://user-images.githubusercontent.com/101227395/188992776-7758d04b-2060-4bc5-a3a8-a308f15d37c0.png)

But Lady Luck is fickle and I find nothing of use except an excellent time waster in the form of [The Useless Web](https://theuselessweb.com), where I stare at a [Pug In A Rug](https://puginarug.com/) for about 5 minutes and become a Priest of the Pug Rug.  **Worth it.**

# Step 2 - Diving Deeper

Without any hits from looking at the HTTP traffic, I check the 'Export Objects' menu option to see what files may have been captured during this network activity. 

![image](https://user-images.githubusercontent.com/101227395/188993740-ac1ed9ec-a677-45ce-a059-879bc03bfb7f.png)

In this menu, you can select DICOM, HTTP, SMB, TFTP, and IMF.  Personally, I don't know what each is, but I know they can sometimes be used to find images or videos or program files that were downloaded.  Unfortunately, this doesn't give me anything more than another reference to [The Useless Web](https://theuselessweb.com), where I spend some time listening to smooth jams of [The Binary Piano](https://binarypiano.com/). 

# Step 3 - Holy Diver, You've Been Down Too Long In The Midnight Sea

After taking a look at all the traffic in the packet capture again, I eventually realize that even if there was any other HTTP traffic, it's all going to be HTTP Secure (HTTPS).  This is confirmed by skimming through the all the traffic that uses the Transmission Control Protocol (TCP) and following the TCP Streams.

![image](https://user-images.githubusercontent.com/101227395/188994880-5fa8f24f-d6cf-4922-88c9-b1849246ec9f.png)

So I try a different approach by inspecting the Domain Name System (DNS) traffic.  I remember from some of my classes that bad dudes on the internet will sometimes hide stuff in DNS traffic, as seen in the [Exfiltration Over Alternative Protocol](https://attack.mitre.org/techniques/T1048/) technique.  I filter down to just DNS traffic and start following the User Datagram Protocol (UDP) streams.

![image](https://user-images.githubusercontent.com/101227395/188995368-27ae3145-2824-485e-a03c-21671d0e85ba.png)

# Step 4 - The Deep

Eventually, I find a Uniform Resource Locator (URL) that looks unusual: https://pastebin.com/raw/GAwEkrN0.  Pastebin has an interesting [history](https://www.zdnet.com/article/pastebin-to-hunt-for-hacker-pastes-anonymous-cries-censorship/) and is a common location for people to "dump" sensitive information, like emails or credit cards.  This paste only gives me some text, which is a shortened URL: https://bit.ly/3RpvvCc.  Following this, I find Bruce with his mouth wide open and the challenge is complete!
![FullShark](https://user-images.githubusercontent.com/101227395/189102657-6c92ec3f-be6a-4512-a0f3-992b6d7b43c0.png)


I wonder how the other Agents are faring in their assigned tasks...
 
---

**Time Taken:** 1 hour 13 minutes
