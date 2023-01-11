---
tags: nim
---
<br>
<br>
<br>
<br>
<br>

<br>
<br>
<center><font size="6"><b>Malware Analysis Report</b></font></center>

![[malware-roger-1920-4116333000.jpg]]
<center><font size = "5"><b>Malware.unknown.exe</b></font></center>
<center><font size = "4"><i><b> 2023_01_10  |  Vertica1_ |  Version 1.0</b></i></font></center>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

# Executive Summary
```ad-info
| Type   | Value                                                             |
| ------ | ---------------------------------------------------------------- |
| MD5    | 812a7c7eb9d7a4332b9e166aa09284d7                                 |
| SHA1   | ec0d565afe635c2c7863b2a05df8a49c58b703a3                         |
| SHA256 | 81a10784ae60a58a969e858c9c4a2ae0d4ebe46e9bd6776992461c062f70099d |
```

On 10 January 2023, an unknown program was obtained by the Incident Response team for Big Money Enterprises, Inc.  An analysis of the captured sample displayed obfuscated information that attempted to hide several URLs.  Launching the program initiates an outgoing beacon that waits to establish a connection.  

Due to the name of the URL, hey[.]youup, we are dubbing this program BootyCall.


***
# High-Level Technical Summary
When the unknown malware is launched, it initiates GET request over HTTP to  hey[.]youup[.]local.  If the program cannot establish a connection to hey[.]youup[.]local, the program will continue to send a request every 3 seconds over a new port on the local host in sequential order.

![[BootyCall.png]]

---

# Analysis

### Static
The following methods were performed for basic static analysis:
1. Obtained hash values, Malware Behavior Catalog (MBC) identifiers, ATT&CK Tactics/Techniques from **Capa**.
2. String analysis with **FLOSS**.
3. Search of SHA256 hash value on **VirusTotal**.
4. DLLs and Win API Calls obtained using **PEstudio**.

#### Capa
![[Pasted image 20230110075810.png]]
#### FLOSS
##### Unique Strings
- `00010203040506070809101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899` 
	- (00 -> 99, no spaces)
- `streams.nim`
- `net.nim`
- `BCryptGenRandom`
- `Bcrypt.dll`
- `@hwtwtwpw:w/w/whwewyw.wywowuwuwpw.wlwowcwawlw`
	- String was 
	- hXXp[://]hey.youup[.]local
- `@axuxtxhx.xnxsx.xlxoxcxaxlx`
	- auth[.]ns[.]local
- `@.cBoBsBmBoBsBfBuBrBbBoBoBtBsBeBmBpBoBrBiBuBmB.BlBoBcBaBlB`
	- .cosmosfurbootsemporium[.]local
- `@Desktop\cosmo.jpeg`
- `user-agent: Nim httpclient/1.6.2`
- `MessageBoxA`
- `OPENPGPKEY`

##### Deobfuscated Strings
- `@hwtwtwpw:w/w/whwewyw.wywowuwuwpw.wlwowcwawlw`
	- String was padded with lowercase 'w'.  Deobfuscated returns  hXXp[://]hey.youup[.]local.
- `@axuxtxhx.xnxsx.xlxoxcxaxlx`
	- String was padded with lowercase 'x'.  Deobfuscated returns auth[.]ns[.]local
- `@.cBoBsBmBoBsBfBuBrBbBoBoBtBsBeBmBpBoBrBiBuBmB.BlBoBcBaBlB`
	- String was padded with uppercase 'B'.  Deobfuscated returns value .cosmosfurbootsemporium[.]local.  Possible partial URL as the presence of the first '.' suggests a subdomain.

### VirusTotal 
Search result utilizing the SHA256 hash: https://www.virustotal.com/gui/file/81a10784ae60a58a969e858c9c4a2ae0d4ebe46e9bd6776992461c062f70099d

![[Pasted image 20230110082439.png]]
<center><i>Figure 1 - Screenshot of the search summary using the SHA256 hash</i></center>

### PEstudio
#### Import Address Table
##### DLLs
* KERNEL32.dll
* msvcrt.dll
* USER32.dll

##### Windows API calls
The below list only includes the Windows API Calls that were flagged by PEstudio.  
- VirtualProtect
- GetCurrentThreadId
- GetCurrentProcessId
- TerminateProcess
- getenv

### Dynamic
When program was run on local machine with no outbound connection capabilities, unknown program made no changes to files or registry.  

#### Persistence
No persistence mechanisms were observed by the analyst at current time.

#### C2/Beaconing
Beacon to hXXp[://]hey[.]youup[.]local every 3 seconds.
![[Pasted image 20230110175129.png]]
<center><i>Figure 2 - Wireshark capture showing beaconing</i></center>

Opens ports and then places them in 'Close Wait' status.
![[Pasted image 20230110175848.png]]
<center><i>Figure 3 - TCPView showing TCP connections being opened then changed to "Close Wait" status</i></center>

Program continues to beacon out to hey[.]youup[.]local  (see Figure 2)  until it is able to establish a connection.  This was accomplished by setting up a DNS entry for the loopback address on the analyst machine.
![[Pasted image 20230110190559.png]]
<center><i>Figure 4 - Netcat listener using loopback address, receiving connection from program after launch</i></center>

Attempting issue commands such as `whoami` and other random phrases drops the connection and the program exits.


---
# Appendix
### Indicators of Compromise

#### Network Indicators
-  hey[.]youup[.]local 

#### Host Indicators
- None at this time

### Rule(s)

#### YARA

rule Training_PMAT_BootyCall : BootyCall
{
	meta:
		author = "V"
		description = "Detection for the late night caller "
		date = "2023-01-10"
		version = "1.0"
		hash = "812a7c7eb9d7a4332b9e166aa09284d7"
	
	strings:
		$c2_url = { 68 ?? 74 ?? 74 ?? 70 ?? 3a ?? 2f ?? 2f ?? 68 ?? 65 ?? 79 ?? 2e ?? 79 ?? 6f ?? 75 ?? 75 ?? 70 ?? 2e ?? 6c ?? 6f ?? 63 ?? 61 ?? 6c }
	
	condition:
		uint16(0) == 0x5A4D and 
		$c2_url
}

#### Suricata

alert http $HOME_NET any -> $EXTERNAL_NET 80 (msg:"Beacon for BootyCall"; flow:to_server; http.header; content:"hey.youup.local"; http.user-agent; content:"Nim httpclient")

### MITRE ATT&CK
T1027 - Obfuscated Files or Information
	T1027.001 - Binary Padding
T1082 - System Information Discovery
T1083 - File and Directory Discovery
