# Introduction to OWASP ZAP

Learn how to use OWASP ZAP from the ground up. An alternative to BurpSuite.

[Introduction to OWASP ZAP](https://tryhackme.com/room/learnowaspzap)

## Topic's

* Web Application Analysis
* OWASP Zap Fundamentals

## Intro to ZAP

![](https://i.imgur.com/qi5qt4r.png)

OWASP Zap is a security testing framework much like Burp Suite. It acts as a very robust enumeration tool. It’s used to test web applications.

**Why wouldn’t I use Burp Suite?**

That’s a GOOD question! Most people in the Info-sec community DO just use Burp Suite. But OWASP ZAP has a few benefits and features that the Burp Suite does not and it’s my preferred program of the two. 

**What are the benefits to OWASP ZAP?**

It’s completely open source and free. There is no premium version, no features are locked behind a paywall, and there is no proprietary code.

There’s a couple of feature benefits too with using OWASP ZAP over Burp Suite:

* Automated Web Application Scan: This will automatically passively and actively scan a web application, build a sitemap, and discover vulnerabilities. This is a paid feature in Burp.
* Web Spidering: You can passively build a website map with Spidering. This is a paid feature in Burp.
* Unthrottled Intruder: You can bruteforce login pages within OWASP as fast as your machine and the web-server can handle. This is a paid feature in Burp.
* No need to forward individual requests through Burp: When doing manual attacks, having to change windows to send a request through the browser, and then forward in burp, can be tedious. OWASP handles both and you can just browse the site and OWASP will intercept automatically. This is NOT a feature in Burp.

If you’re already familiar with Burp the keywords translate over like so:

![](https://lh6.googleusercontent.com/KpTw37QyRfu1WTf5rIGz4ZDwEPm7s4CutgFTSrFBsXJT97a8HT8HiIcuUciFCyVXxcfVmCMuX1TbhOAM2gZLBYgkzPQzpF3tcd_DpcudC6JElYRDoZhUcXeP4mtn83UeUa7gKnWG)

This guide will teach you how to do the following in ZAP:

* Automated Scan
* Directory Bruteforce
* Authenticated Scan
* Login Page Bruteforce
* Install ZAP Extensions

This room will be using OWASP Zap against the DVWA machine, feel free to deploy your own instance and follow along.

1. What does ZAP stand for?

* [OWASP ZAP](https://en.wikipedia.org/wiki/OWASP_ZAP)

`Zed Attack Proxy`

2. Connect to the TryHackMe network and deploy the machine. Once deployed, wait a few minutes and visit the web application: http://MACHINE_IP

## Disclaimer

ZAP is a great tool that’s totally slept on, and I personally prefer it over Burp, but the documentation and support for the tool is microscopic compared to the titan that is Burp.

Burp has some extensions and features that ZAP does not have, as an example ZAP is unable to perform Login timing attacks. Burp can. If you wish to learn more about login timing attacks you can check out the TryHackMe room Hackernote.

ZAP can be used as your go-to tool to start Web Application testing but it should not be your only tool. ZAP is just one of many tools to put under your hacker utility belt.

1. I've read the task.

## Installation

![](https://i.imgur.com/1pRazZt.png)

OWASP ZAP has a handy installer for Windows, Mac OS, and Linux systems.

Download and install it from the official website: [https://www.zaproxy.org/download/](https://www.zaproxy.org/download/)

1. Install ZAP on an operating system of your choice!
2. Open OWASP ZAP, ready to follow along with this room.

## How to perform an automated scan

Lets perform an automated scan. Click the big Automated Scan button and input your target.

![](https://lh4.googleusercontent.com/1NsFAThXFHd7PdAR9C1puIflm8QYQJrxZ0hEC1me4aZKxlfuagRPJHOYdo0RLzmLBMXfWmgUvSOwBLHZkFR_rAANQEb3AG9HkdYyEBsjCr9rDPxGPtQ73SPszwO2BhtrWOR6l2Wa)

The automated scan performs both passive and automated scans to build a sitemap and detect vulnerabilities.

On the next page you may see the options to select either to use “traditional spider” or “Ajax spider”.

A traditional spider scan is a passive scan that enumerates links and directories of the website. It builds a website index without brute-forcing. This is much quieter than a brute-force attack and can still net a login page or other juicy details, but is not as comprehensive as a bruteforce.

The Ajax Spider is an add-on that integrates in ZAP a crawler of AJAX rich sites called Crawljax. You can use it in conjunction with the traditional spider for better results. It uses your web browser and proxy.

The easiest way to use the Ajax Spider is with HTMLUnit.

To install HTML Unit use the command

`sudo apt install libjenkins-htmlunit-core-js-java`

And then select HtmlUnity from the Ajax Spider Dropdown.

Both utilities can further be configured in the options menu (`Ctrl+Alt+O`)

Example Automated Scan Output:

![](https://lh5.googleusercontent.com/rmYUE3qmjalywZ1gDsYhXtmOlJ5T8ei4OXXV2_0KhK7VPODDLtAoT-c9uBG-CC24Ivf1JkEYsF2irOJClEBqBPxzsdOX2r_ZUnf4i2ed5UZ5Qpl872IENhxcWRVhivehiJb4mV-B)

With very minimal setup we were able to do an automated scan that gave us a sitemap and a handful of vulnerabilities.

1. Set up Ajax Spider

## Manual Scanning

Lets perform a manual scan against the DVWA machine.

Like Burp, you should set-up your proxy between OWASP ZAP and your Browser. We’ll be using Firefox. 

**OWASP Proxy Setup:**

![](https://lh3.googleusercontent.com/BH2dFPdblAa8hB6pAFlWFog8zqwcrG5OcDrJuheExNR9acufgMU5a8GYlagfTCUS4elqznDldO1OBLCPd0ty4AiSkuetyouugh5ak1-N_BTzHdU79IZbHhjPUM6HUUe-pQQpL-9T)

Open Options

![](https://lh6.googleusercontent.com/AVtxM1-JnYBlppNSVYArxS7kv58cpKdDkvu1XH9Jm2KtUrPiNFscqG-YpqDPOvYFoEiNiQYnjnV3TwbyHyn6nO5iULoPG1xWdO8_l66bpWVUGtJPqdumLhGPlMIKnH7GxLLhRrai)

Change Local Proxy settings to the above.

**Add ZAP Certificates:**

Without importing ZAP Certificates, ZAP is unable to handle simultaneous Web request forwarding and intercepting. Do not skip this step.

![](https://lh3.googleusercontent.com/4gzHAeodZOBlXVPjtaDgU5v14E2hVzhRK_suUqvUagGI4isvVALgYLVmc4z6PaRCrwLfIo-aIjbzXQcbTs0tMpYFQAtIA5n4fjMaA6REuLmDtuienaG2XTkzD1gBcblw0CiTnwtp)

In the same options menu, navigate to Dynamic SSL Certificates and save the certificate somewhere you’ll remember and not delete.

![](https://lh4.googleusercontent.com/j66KrFbvPyCtd7rpMuZeUj04nWWHcW-f5_kKfhjXHkm19BF8fCw3-A_uRrCNcc0bWJnkKhmSN-TUKUAHS4yHL21DIjcWavnihzSg2MgWptEPb41O6Ltgt1PH7tBdvZJEUtNdt6Y-)

Then, open Firefox, navigate to your preferences, and search for certificates and click “View Certificates” 

![](https://lh4.googleusercontent.com/qzOOB_XWTvZfRfuDaPz5_xmyzV-CGjXKP9G-uk-p3wV3z0DkRInfLgfo4hgoAm9ioo6h2zcI4DT6bBHrQYdCzqM2boUqBDdtpwPXUsGICC8qdCSGyX_OPD9VfUJC18-I6_mVVPtT)

Then click “Import” and then navigate to the earlier downloaded certificate and open it. 

![](https://lh5.googleusercontent.com/4b179PhoDOWf6W-TI7WXr3kCIQS0_lh_7P1iiVmB_c4Lo5buapPUNAkLegHNqaF4NKcvHcaE090HngEW_PjR8bPCZ3UTqGRnkMZZJCAzuwBRHZsi_46p8S42h4muoUptq-qrlGRX)

Select both and then hit OK.

**Firefox Proxy Setup:**

![](https://lh4.googleusercontent.com/JAoKQSMJ7N6RzquRfKerRYaO-KlqEFOY78cBBEFQM1oSoyNuCtaAOZx7CFB8gbeoCj_CJlEfIzO9pJT75EA2FhfMVg5jvIBkMIwvM_hF4Okcabu6S0sQt0KTxUMckN4T-9UWbiyR)

Go back to your Firefox preferences and search for “proxy”. Click Settings.

![](https://lh4.googleusercontent.com/kv9khldkNj55yzZIVtmZEoaaDCmFUFNFjiT3AsFTSfFNy2fAjYm-KKxdAxJaU3Nfeiddjd3yLR7EBznqhw3lAkkg_Uu4z6z7r_Yt-ERacL2ZYOdxkO7Zi0ia_amkcT6s5aFxUsHS)

Adjust your Manual Proxy Configuration to match and then click OK. 

Now you’re set-up! Time to get into the fun stuff :)

1. What IP do we use for the proxy?

`127.0.0.1`

## Scanning an Authenticated Web Application

Without your Zap application being authenticated, it can't scan pages that are only accessible when you've logged in. Lets set up the OWASP ZAP application to scan these pages, using your logged in session.

Lets go to the DVWA machine (http://MACHINE_IP), and login using the following credentials:

Username: **admin**
Password: **password**

![](https://lh3.googleusercontent.com/vQqr9MiQKsRBoxeOtmi46APqHfX2AQIPl89jWc0CFMAJQ0PIpeMx6ViAwVmxW_O1iecnZ5JPPma4OlpQq-PYOMfx_zBnNN443uMfeHgV2Q_UkhSt-fOnlHAUAFJpQv4ZK0g6YZJT)

After logging in you should see this.

![](https://lh6.googleusercontent.com/DnffEIkiBTAB6EOO99JDaQho9iqCAvPpV-4fsT4Aep4xLof3RToyKx_Z-hW4s2NONOJ6FSwzaO-cEkkEITp2gbeWT9cTJmoauiQwca3XtMvGVJNHVhEeZmhkzfgQ_u5MRgB8X1hG)

For the purpose of this exercise, once you've logged in, navigate to the DVWA Security tab and set the Security level to Low and then hit submit.

We're going to pass our authentication token into ZAP so that we can use the tool to scan authenticated webpages.

![](https://lh3.googleusercontent.com/we59cHeEzTth8qgVZUyhDR7NVZbGAvA7IWbX46mlTEBUGerLwjn0qzhWX2WZa0DLjxXU19_4PdzXuM64Nk8hw0H2R9ZLD7OaMG394kzwyj2S_iSyFoM-8OMP4rGEmb4mqYmCltPF)

In ZAP open the HTTP Sessions tab with the new tab button, and set the authenticated session as active.

Now re-scan the application. You’ll see it’s able to pick up a lot more. This is because its able to see all of the sections of DVWA that was previously behind the login page. 

1. Try scanning the DVWA web application as an authenticated user.

`No answer needed`

## Brute-force Directories

If the passive scans are not enough, you can use a wordlist attack and directory bruteforce through ZAP just as you would with gobuster. This would pick up pages that are not indexed.

![](https://lh3.googleusercontent.com/z8ApJRUY4nVhUrZn4Z4A4ABe02lw2YXFmr1uolH59FrXMoWMQHtLbgQmQBAWXy7stu6duEbhnZrElLAVJ6lOhy4uvMppyQ4bLjqiMSNCLzwvZREI0_fweeCGPIAdpshw02uGrLwm)

First. Go into your ZAP Options (at the bottom navigation panel, with the screen plus button), navigate to Forced Browse, and add the Custom Wordlist. You can also add more threads and turn off recursive brute-forcing.

![](https://lh6.googleusercontent.com/T5o__aO4ykIWUQuHp_Mr6dFqFpFHfp6wmnAoVcPMrGZPfF5LCR4gBFA6aVdiX9t-XHA_LZIKP6Bjdcqkt_Fo41JtUlA5mOq1voGcU5TuqBabBIy0fHRfLjJyrFPYaW5v0_1j8DCa)

Then, right click the site->attack->forced browse site

![](https://i.imgur.com/QNrZngC.png)

Select your imported wordlist from the list menu, and then hit the play button! We recommend using this wordlist for this exercise.

ZAP will now bruteforce the entire website with your wordlist.

1. Try brute-forcing the DVWA web application.

``

## Bruteforce Web Login

Lets brute-force a form to get credentials. Although we already know the credentials, lets see if we can use Zap to obtain credentials through a Brute-Force attack.

If you wanted to do this with BurpSuite, you'd need to intercept the request, and then pass it to Hydra. However, this process is much easier with ZAP!

![](https://lh3.googleusercontent.com/cr3N3j8wZlT3e-EzfiPNB-cws2oN7qE6Yuh432SGhweGjRiWUx62-1SmqDIuszJ7jXvBukmiPxnHUEy424IlNTcqjSX5nAOv67R-XtZgckc8Z_fwzCipmcFNAYRZnSuaL-cD9B9T)

Navigate to the Brute Force page on DVWA and attempt login as “**admin**” with the password “**test123**”

![](https://lh4.googleusercontent.com/1MKBaSdI9McUcXCxpNtsOar01CXLtXaD136vofBnA3c86uBWFSMEZNCjT-xjLwvG6URuy12i3yLkDeB1wCtHXyuL6L7lyqDbm3mR1XC5w78YWTLYSPmZPs6HgVTVFUJWU0oWh23D)

Then, find the GET request and open the Fuzz menu.

![](https://lh5.googleusercontent.com/n6XwcP_q0bQX6tilUqGayuiGyolg1bPAsEvpBsYlJfvatXeVrt-zkbmxnw7ftLfA_SD8TJcyxnaa4l8Zixw0TIWImh2PyfH5MlRKVX2fZtkAJeWHVy77EXO1c8rIXrpnV7s2NOzo)

Then highlight the password you attempted and add a wordlist. This selects the area of the request you wish to replace with other data. 

![](https://lh5.googleusercontent.com/hbB-70zZq30Xhl7ZcAkbMKg8w3lZQ-bKOjxa9pi6-rZ1wYmi33H-S-Z0g10M2GNF1LyOPLILovrl09E91jwi-oWClS9WRhMLyKVmI5aUMb5rsVvjZu74FCV2bCJabzmjjXmCj7-O)

For speed we can use fasttrack.txt which is located in your `/usr/share/wordlists` if you’re using Kali Linux. 

![](https://lh3.googleusercontent.com/_QpKqdAtZLMGhhc9PaaziYGm9ANooS6b_ePkXSx13uUDXU1h1-auC06UC8R9WJKwDv0ar1oSi8S5Bxd70CvRY0GI5PsxxI2XwCdalq0EnEb4wTEvTs43dPMZVrREdMrYojhK660m)

After running the fuzzer, sort the state tab to show Reflected results first. Sometimes you will get false-positives, but you can ignore the passwords that are less than 8 characters in length.

1. Use ZAP to bruteforce the DVWA 'brute-force' page. What's the password?

`password`

## ZAP Extensions

Want to further enhance ZAPs capabilities? Look at some of it’s downloadable extensions!


* [https://github.com/zaproxy/zap-extensions](https://github.com/zaproxy/zap-extensions)
* [https://github.com/bugcrowd/HUNT](https://github.com/bugcrowd/HUNT)

Let’s install the bugcrowd HUNT extensions for OWASP ZAP. This will passively scan for known vulnerabilities in web applications.

First navigate in your terminal somewhere you’d like to store the scripts

`git clone https://github.com/bugcrowd/HUNT`

![](https://lh4.googleusercontent.com/zBrHAyWTsLGlJS4Nj4r_AeATc61_FTe9CUH_uxAKjH_GQEVSfnxuK2JpJlwdIlGVl0abgv8G_rTZEf-SXOUMtll7k300KjW0Shi-O5i-f9AC6w4Pvgthfi7_gF2wJUFMBpiJoAQx)

Then in ZAP click the “Manage Add-Ons” icon

![](https://lh6.googleusercontent.com/ykeiHWbfj4cKBb20d_qN3-avWIFovtmBZwkP3g4Q247PdMlAIRDqNC-dozxfnWVoO60Nr7T0kwas3mfLgHKAA94a-qxLvlaEm8d8ibqWm_gdDILuKgK5VIMD5-IVtAPHH01Q7IRV)

From the Marketplace install “Python Scripting” and “Community Scripts”

![](https://lh6.googleusercontent.com/xK8a02iEsNTJxCt_YnhZC1KCOH5fCtEIas4hrxlEwPnQrSSwwgg2xz-oeYG9-YA67oNr3PmxwdC3WGuunvHgoDq9do6-BhqVdZDIjNn2MGABXifz_55Ef61M-xLSAShOBR6oo2nR)

In ZAP Options, under Passive Scanner, make sure “Only scan messages in scope” is enabled. Then hit OK.

![](https://i.imgur.com/DG6E3CN.png)

In ZAP open the Scripts tab.

![](https://lh5.googleusercontent.com/yqVWaq3JqV_a3J6IPSLnUfEGvOgnn3uQAUNUkWIX0vHnyxKwzwihq_AMWpkOH7sIEDXGCoLY41qn56M6cxow-yl1XHjFq-xWGX89-qFyKvLjCEceHtSVeq0KuwrTEPsUvMhdHmA-)

And under Passive Rules, find and enable the HUNT.py script

Now when you browse sites and HUNT will passively scan for SQLi, LFI, RFI, SSRF, and others. Exciting!

1. Set up HUNT on your Zap application to automatically perform passive scans on sites you visit!

## Further Reading

Wow! You reached the end! Good job! Try your new ZAP skills on some Web Application CTFs. TryHackMe has quite the variety. My personal favorite is HackPark.

* Desktop eManuel: [https://www.zaproxy.org/docs/desktop/ui/](https://www.zaproxy.org/docs/desktop/ui/)
* OWASP ZAP Forums: [https://groups.google.com/forum/#!forum/zaproxy-users](https://groups.google.com/forum/#!forum/zaproxy-users)
* ZAP in Ten: [https://www.alldaydevops.com/zap-in-ten](https://www.alldaydevops.com/zap-in-ten)

Yeah that's pretty much all there is. I wasn't kidding when I said "microscopic" in comparison to Burp suite.

That's the one major con of ZAP is the pitiful amount of documentation there is. The project is still active and contributed to though. Just no one's really writing guides. 

1. Check out the additional reading material.

