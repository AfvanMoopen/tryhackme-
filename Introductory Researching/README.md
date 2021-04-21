# Introductory Researching

A brief introduction to research skills for pentesting.

[Introductory Researching](https://tryhackme.com/room/introtoresearch)

## Topic's

* OSINT
* Vulnerability Analysis
* Linux Fundamentals

## Introduction

Without a doubt, the ability to research effectively is the most important quality for a hacker to have. By its very nature, hacking requires a vast knowledge base -- because how are you supposed to break into something if you don't know how it works? The thing is: no one knows everything. Everyone (professional or amateur, experienced or totally new to the subject) will encounter problems which they don't automatically know how to solve. This is where research comes in, as, in the real world, you can't ever expect to simply be handed the answers to your questions.

As your experience level increases, you will find that the things you're researching scale in their difficulty accordingly; however, in the field of information security, there will never come a point where you don't need to look things up.

This room will serve as a brief overview of some of the most important resources available to you, and will hopefully aid you in the process of building a research methodology that works for you.

We will be looking at the following topics:

* An example of a research question
* Vulnerability Searching tools
* Linux Manual Pages

Let's begin.

1. Read the Introduction

`No answer needed`

## Example Research Question

We'll begin by looking at a typical research question: the kind that you're likely to find when working through a CTF on TryHackMe.

Let's say you've downloaded a JPEG image from a remote server. You suspect that there's something hidden inside it, but how can you get it out?
How about we start by searching for “hiding things inside images” in Google:

![](https://muirlandoracle.co.uk/uploads/2020/03/GoogleSearch1.png)

Notice that the second link down gives us the title of a technique: “Steganography”. You can then click that link and read the document, which will teach you how files are hidden inside images.

Ok, so we know how it's done, let's try searching for a way to extract files using steganography:

![](https://muirlandoracle.co.uk/uploads/2020/03/GoogleSearch2.png)

Already virtually every link is pointing to something useful. The first link contains a collection of useful tools, the second is more instructions on how to perform steganography in the first place. Realistically any of these links could prove useful, but let's take a look at that first one ([https://0xrick.github.io/lists/stego/](https://0xrick.github.io/lists/stego/)):

![](https://muirlandoracle.co.uk/uploads/2020/03/0xRick.png)

The very first tool there looks to be useful. It can be used to extract embedded data from JPEG files -- exactly what we need it to do! This page also tells you that steghide can be installed using something called “apt”.
Let's search that up next!

![](https://muirlandoracle.co.uk/uploads/2020/03/AptSearch.png)

Great -- so apt is a package manager that lets us install tools on Linux distributions like Ubuntu (or Kali!).
How can we install packages using apt? Let's search it!

![](https://muirlandoracle.co.uk/uploads/2020/03/AptSyntax.png)

Perfect -- right at the top of the page we're given instructions. We know that our package is called steghide, so we can go ahead and install that:

![](https://muirlandoracle.co.uk/uploads/2020/03/AptInstallation.png)

Now, let's switch back to that collection of steganography tools we were looking at before. Did you notice that there were instructions on how to use steghide right there?

![](https://muirlandoracle.co.uk/uploads/2020/03/SteghideSyntax.png)

There we go! That's how we can extract an image from a file. Our research has paid off and we can now go and complete the task.

Notice the methodology here. We started with nothing, but gradually built up a picture of what we needed to do. We had a question (How can I extract data from this image). We searched for an answer to that question, then continued to query each of the answers we were given until we had a full understanding of the topic. This is a really good way to conduct research: Start with a question; get an initial understanding of the topic; then look into more advanced aspects as needed.

Now it's your turn. See if you can answer the following questions using your research skills. The first three questions have appropriate search queries in the hints:

1. In the Burp Suite Program that ships with Kali Linux, what mode would you use to manually send a request (often repeating a captured request numerous times)?

`Repeater`

2. What hash format are modern Windows login passwords stored in?

`NTLM`

3. What are automated tasks called in Linux?

`Cron Jobs`

4. What number base could you use as a shorthand for base 2 (binary)?

`Base 16`

5. If a password hash starts with $6$, what format is it (Unix variant)?

`sha512crypt`

## Vulnerability Searching

Often in hacking you'll come across software that might be open to exploitation. For example, Content Management Systems (such as Wordpress, FuelCMS, Ghost, etc) are frequently used to make setting up a website easier, and many of these are vulnerable to various attacks. So where would we look if we wanted to exploit specific software?

The answer to that question lies in websites such as:

* ExploitDB
* NVD
* CVE Mitre

NVD keeps track of CVEs (**C**ommon **V**ulnerabilities and **E**xposures) -- whether or not there is an exploit publicly available -- so it's a really good place to look if you're researching vulnerabilities in a specific piece of software. CVEs take the form: CVE-YEAR-IDNUMBER
*(Hint Hint: It's going to be really useful in the questions!)*

[ExploitDB](https://www.exploit-db.com/) tends to be very useful for hackers, as it often actually contains exploits that can be downloaded and used straight out of the box. It tends to be one of the first stops when you encounter software in a CTF or pentest.

If you're inclined towards the CLI on Linux, Kali comes pre-installed with a tool called "searchsploit" which allows you to search ExploitDB from your own machine. This is offline, and works using a downloaded version of the database, meaning that you already have all of the exploits already on your Kali Linux!

Let's take an example. Say we're playing a CTF and we come across a website:

![](https://i.imgur.com/8fhG8ZO.png)

Well, this is quite obviously FuelCMS. Usually it won't be this obvious, but hey, we'll work with what we've got!

We know the software, so let's search for it in ExploitDB.
*(Note: I'm going to use the CLI tool in Kali, as I find that it's quicker -- however, you are welcome to use the website)*

I'm using the command `searchsploit fuelcms` to search for exploits:

![](https://i.imgur.com/pu2kdrm.png)

If you prefer doing things in the website, here are the results from there:

![](https://i.imgur.com/8MksLin.png)

Success! We've got an exploit that we can now use against the website!

Actually using the exploit is outwith the scope of this room, but you can see the process. 

If you click on the title you'll be given a bit more of an explanation about the exploit:

![](https://i.imgur.com/7DMp9h1.png)

Pay particular attention to the CVE numbers; you'll need them for the questions!
The format will be like so: `CVE-YEAR-NUMBER`

1. What is the CVE for the 2020 Cross-Site Scripting (XSS) vulnerability found in WPForms?

`CVE-2020-10385`

2. There was a Local Privilege Escalation vulnerability found in the Debian version of Apache Tomcat, back in 2016. What's the CVE for this vulnerability?

`CVE-2007-0017`

3. What is the very first CVE found in the VLC media player?

`CVE-2019-18634`

4. If I wanted to exploit a 2020 buffer overflow in the sudo program, which CVE would I use?

`CVE-2019-18634`

## Manual Pages

If you haven't already worked in Linux, I would highly recommend taking a look at the [Learn Linux](https://tryhackme.com/room/zthlinux) room. Linux (usually Kali Linux) is without a doubt the most ubiquitous operating system used in hacking, so it pays to be familiar with it!

One of the many useful features of Linux is the inbuilt `man` command, which gives you access to the manual pages for most tools directly inside your terminal. Occasionally you'll find a tool that doesn't have a manual entry; however, this is rare. Generally speaking, when you don't know how to use a tool, `man` should be your first port of call.

Let's give this a shot!

Say we want to connect to a remote computer using SSH, but we don't know the syntax. We can try `man ssh` to get the manual page for SSH:

![](https://i.imgur.com/WgjMwFZ.png)

Awesome!

We can see in the description that the syntax for using SSH is <user>@<host>:

![](https://i.imgur.com/uFricVE.png)

We can also use the man pages to look for special switches in programs that make the program do other things. An example of this would be that (from our very first example) steghide can be used to both extract and embed files inside an image, based on the switches that you give it. 

For example, if you wanted to display the version number for SSH, you would scroll down in the `man` page until you found an appropriate switch:

![](https://i.imgur.com/0yvbE8H.png)

Then use it:

![](https://i.imgur.com/oxwiNxv.png)

Another way to find that switch would have been to search the `man` page for the correct switch using grep:

![](https://i.imgur.com/cpFNlQu.png)

Now your turn! Answer the following questions using the man command:

1. SCP is a tool used to copy files from one computer to another. *What switch would you use to copy an entire directory?*

`-r`

2. fdisk is a command used to view and alter the partitioning scheme used on your hard drive. *What switch would you use to list the current partitions?*

`-l`

3. nano is an easy-to-use text editor for Linux. There are arguably better editors (Vim, being the obvious choice); however, nano is a great one to start with.  *What switch would you use to make a backup when opening a file with nano?*

`-B`

4. Netcat is a basic tool used to manually send and receive network requests. *What command would you use to start netcat in listen mode, using port 12345?*

`nc -l -p 12345`

# Final Thoughts

You may have been told in school that there are good sources and bad sources of information. That may be true when it comes to essays and referencing information; however, it's my pleasure to state that it does not apply here. Any information can potentially be useful -- so feel free to use blogs, wikipedia, or anything else that contains what you're looking for! Blogs especially can often be very valuable for learning when it comes to information security, as many security researchers keep a blog.

Having completed this room, you hopefully now have established the basis of a methodology to tackle research questions that you come across by yourself. The vast majority of rooms on TryHackMe can be solved purely using knowledge found on Google, so please take the opportunity to improve your skills by Googling any problems you come across!

As a follow-up to this room, I highly recommend CMNatic's [Google Dorking](https://tryhackme.com/room/googledorking) room to learn some advanced Google tricks!

1. Research Complete!

`No answer needed`
