# Splunk

Part of the Blue Primer series, learn how to use Splunk to search through massive amounts of information

[Splunk](https://tryhackme.com/room/bpsplunk)

## Topic's

* Splunk Fundamentals
* System Forensic

## Task 1 Deploy!

Deploy the Splunk virtual machine. **This can take up to five to ten minutes to launch. If the webpage does not load for you after ten minutes, terminate and relaunch the machine.**

* **Username**: splunkUser
* **Password**: SplunkUser#321

The first section of this room consists of a quiz over Splunk. I recommend attempting the quiz while the machine loads as it can take some time. If the VM fails to load, a direct link to the OVA file (Splunk) can be found [here](https://darkstar7471.com/resources.html). You can also build this manually using the data and instructions found at this [link](https://github.com/splunk/botsv1).

Deploy the virtual machine and connect to the website found at BOX_IP:8000

`No answer needed`

Move on to the quiz while Splunk loads

`No answer needed`

## Task 2 Can you dig it?

A short quiz over the base search commands that are useful for Splunk. All you'll need for this is the attached quick reference guide and possibly the magic of Google. Include all parts of the search query unless otherwise instructed.

----------------------------------------------------

![](https://i.imgur.com/iabqAoB.png)

----------------------------------------------------

Enjoy the room! For future rooms and write-ups, follow [@darkstar7471](https://twitter.com/darkstar7471) on Twitter.

Splunk queries always begin with this command implicitly unless otherwise specified. What command is this? When performing additional queries to refine received data this command must be added at the start. This is a prime example of a slight trick question.

`search command is used to look up for things you actually wanna look up in the whole data.`

`search`

When searching for values, it's fairly typical within security to look for uncommon events. What command can we include within our search to find these?

`rare command gives you the top 5 or 10 least common result while the top command gives you the top most`

`rare`

What about the inverse? What if we want the most common security event?

`top`

When we import data into splunk, what is it stored under?

`it’s like a repository of the data.`

`index`

We can create 'views' that allow us to consistently pull up the same search over and over again; what are these called?

`dashboard`

Importing data doesn't always go as planned and we can sometimes end up with multiple copies of the same data, what command do we include in our search to remove these copies?

`deduplicates data.`

`dedup`

Splunk can be used for more than just a SIEM and it's commonly used in marketing to track things such as how long a shopping trip on a website lasts from start to finish. What command can we include in our search to track how long these event pairs take?

`transaction`

In a manner similar to Linux, we can 'pipe' search results into further commands, what character do we use for this?

`pipe`

`|`

In performing data analytics with Splunk (ironically what the tool is at it's core) it's useful to track occurrences of events over time, what command do we include to plot this?

`timechart`

What about if we want to gather general statistical information about a search?

`stats`

Data imported into Splunk is categorized into columns called what?

`fields`

When we import data into Splunk we can view it's point of origination, what is this called? I'm looking for the machine aspect of this here.

`host`

When we import data into Splunk we can view its point of origination from within a system, what is this called?

`source`

We can classify these points of origination and group them all together, viewing them as their specific type. What is this called? Use the syntax found within the search query rather than the proper name for this.

`sourcetype`

When performing functions on data we are searching through we use a specific command prior to the evaluation itself, what is this command?

`eval`

Love it or hate it regular expression is a massive component to Splunk, what command do we use to specific regex within a search?

`rex`

It's fairly common to create subsets and specific views for less technical Splunk users, what are these called?

`pivot tables`

What is the proper name of the time date field in Splunk

`_time`

How do I specifically include only the first few values found within my search?

`head`

More useful than you would otherwise imagine, how do I flip the order that results are returned in?

`reverse`

When viewing search results, it's often useful to rename fields using user-provided tables of values. What command do we include within a search to do this?

`lookup`

We can collect events into specific time frames to be used in further processing. What command do we include within a search to do just that?

`bucket`

We can also define data into specific sections of time to be used within chart commands, what command do we use to set these lengths of time? This is different from the previous question as we are no longer collecting for further processing.

`span`

When producing statistics regarding a search it's common to number the occurrences of an event, what command do we include to do this?

`count`

Last but not least, what is the website where you can find the Splunk apps at?

`splunkbase.splunk.com`

We can also add new features into Splunk, what are these called?

`apps`

What does SOC stand for?

`security operations center`

What does SIEM stand for?

`security information and event management`

How about BOTS?

`you can just see this given in Task 3 description.`

`boss of the soc`

And CIM?

`Splunk CMI`

`common information model`

what is the website where you can find the Splunk forums at?

`answers.splunk.com`

## Task 3 BOTS!

Following our delightful introduction to Splunk with our command quiz, we'll be visiting some of the training material that Splunk has directly generated as part of their 'Boss of the SOC' Security Operations Center competition!

![](https://i.imgur.com/ImiZwDX.png)

If you're interested in this, check out this [link](https://www.splunk.com/blog/2017/09/06/what-you-need-to-know-about-boss-of-the-soc.html)

Check out BOTS!

`No answer needed`

## Task 4 Halp, I'm drowning in logs!

Navigate to the webpage found at 10.10.235.162:8000 on the machine you've deployed for this room. The credentials for logging into Splunk are as follows:

* **Username**: splunkUser
* **Password**: SplunkUser#321

![](https://i.imgur.com/HHmY6QZ.png)

Navigate to the 'Investigating with Splunk Workshop' App on the left side of the screen following login. You'll be greeted with the following images:

**Website Defacement**

![](https://i.imgur.com/dHWoa18.png)

**An Example Cerber Ransomware Infection, an Unfortunately Common Real Ransomware**

![](https://i.imgur.com/Zj1umcF.png)

These two images are the two scenarios we will be working through throughout this room. In addition to this, we will be using Lockheed Martin's Kill Chain to break down each attack and report it accordingly.

![](https://i.imgur.com/dEuPao3.png)

Start reading through the Overview and navigate through it until you are ready to begin the APT!

Read the overview!

`No answer needed`

## Task 5 Advanced Persistent Threat

Work your way through the first scenario in order to track down P01s0n1vy! Don't hesitate to use the material provided to give you a nudge!

What IP is scanning our web server?

`40.80.148.42`

What web scanner scanned the server?

`Acunetix`

What is the IP address of our web server?

`192.168.250.70`

What content management system is imreallynotbatman.com using?

`Joomla`

What address is performing the brute-forcing attack against our website?

`23.22.63.114`

What was the first password attempted in the attack?

`12345678`

One of the passwords in the brute force attack is James Brodsky's favorite Coldplay song. Which six character song is it?

`yellow`

What was the correct password for admin access to the content management system running imreallynotbatman.com?

`batman`

What was the average password length used in the password brute forcing attempt rounded to closest whole integer?

`6`

How many seconds elapsed between the time the brute force password scan identified the correct password and the compromised login rounded to 2 decimal places?

`92.17`

How many unique passwords were attempted in the brute force attempt?

`412`

What is the name of the executable uploaded by P01s0n1vy?

`3791.exe`

What is the MD5 hash of the executable uploaded?

`AAE3F5A29935E6ABCC2C2754D12A9AF0`

What is the name of the file that defaced the imreallynotbatman.com website?

`poisonivy-is-coming-for-you-batman.jpeg`

This attack used dynamic DNS to resolve to the malicious IP. What fully qualified domain name (FQDN) is associated with this attack?

`prankglassinebracket.jumpingcrab.com`

What IP address has P01s0n1vy tied to domains that are pre-staged to attack Wayne Enterprises?

`23.22.63.114`

Based on the data gathered from this attack and common open source intelligence sources for domain names, what is the email address that is most likely associated with P01s0n1vy APT group?

`lillian.rose@po1s0n1vy.com`

GCPD reported that common TTPs (Tactics, Techniques, Procedures) for the P01s0n1vy APT group if initial compromise fails is to send a spear phishing email with custom malware attached to their intended target. This malware is usually connected to P01s0n1vy’s initial attack infrastructure. Using research techniques, provide the SHA256 hash of this malware.

`9709473ab351387aab9e816eff3910b9f28a7a70202e250ed46dba8f820f34a8`

What special hex code is associated with the customized malware discussed in the previous question?

`53 74 65 76 65 20 42 72 61 6e 74 27 73 20 42 65 61 72 64 20 69 73 20 61 20 70 6f 77 65 72 66 75 6c 20 74 68 69 6e 67 2e 20 46 69 6e 64 20 74 68 69 73 20 6d 65 73 73 61 67 65 20 61 6e 64 20 61 73 6b 20 68 69 6d 20 74 6f 20 62 75 79 20 79 6f 75 20 61 20 62 65 65 72 21 21 21`

What does this hex code decode to?

`Steve Brant’s Beard is a powerful thing. Find this message and ask him to buy you a beer!!!`

## Task 6 Ransomware

One of your users at Wayne Enterprises has managed to get their machine infected, work your way through this second scenario to discover how it happened! Don't hesitate to use the material provided to give you a nudge!

What was the most likely IP address of we8105desk on 24AUG2016?

`192.168.250.100`

What is the name of the USB key inserted by Bob Smith?

`MIRANDA_PRI`

After the USB insertion, a file execution occurs that is the initial Cerber infection. This file execution creates two additional processes. What is the name of the file?

`Miranda_Tate_unveiled.dotm`

During the initial Cerber infection a VB script is run. The entire script from this execution, pre-pended by the name of the launching .exe, can be found in a field in Splunk. What is the length in characters of this field?

`4490`

Bob Smith's workstation (we8105desk) was connected to a file server during the ransomware outbreak. What is the IP address of the file server?

`192.168.250.20`

What was the first suspicious domain visited by we8105desk on 24AUG2016?

`solidaritedeproximite.org`

The malware downloads a file that contains the Cerber ransomware cryptor code. What is the name of that file?

`mhtr.jpg`

What is the parent process ID of 121214.tmp?

`3968`

Amongst the Suricata signatures that detected the Cerber malware, which signature ID alerted the fewest number of times?

`2816763`

The Cerber ransomware encrypts files located in Bob Smith's Windows profile. How many .txt files does it encrypt?

`406`

How many distinct PDFs did the ransomware encrypt on the remote file server?

`257`

What fully qualified domain name (FQDN) does the Cerber ransomware attempt to direct the user to at the end of its encryption phase?

`cerberhhyed5frqa.xmfir0.win`
