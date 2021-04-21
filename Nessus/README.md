# Nessus

Learn how to set up and use Nessus, a popular vulnerability scanner.

[Nessus](https://tryhackme.com/room/rpnessus)

## Topic's

* Web Application Analysis
* Nesus Fundamentals

## [Task 1] Deploy!

Deploy the vulnerable machine! This one, well, it has problems.

1. Deploy the virtual machine!

`No answer needed`

## [Task 2] Installation

Install Nessus on a system of your system of choice! For the sake of this guide, I'll be using Ubuntu. I highly recommend installing this on a dedicated VM just for Nessus scanning. Here's a link to the Nessus documentation online: [https://docs.tenable.com/nessus/Content/GettingStarted.htm](https://docs.tenable.com/nessus/Content/GettingStarted.htm)

1. First, create a basic Ubuntu box (or any other system of your choice). Minimum 4 2GHz cores, 4 GB RAM (8 Recommended) and 30 GB of disk space.

`No answer needed`

2. Next, go ahead and register for a Nessus Home license. This can be used to scan up to 16 IP addresses at a time. Be sure to keep this license information safe, you'll need it for any manual work. Here's the registration link: [https://www.tenable.com/products/nessus-home](https://www.tenable.com/products/nessus-home)

`No answer needed`

3. Follow the installation instructions on Tenable's website, once Nessus is set up connect the machine that it lives on to the network using your VPN file.

`No answer needed`

## [Task 3] Nessus Quiz

A short quiz on the features and functions of Nessus, this includes the Nessus 7 manual as well for any clarification. 

1. As we log into Nessus, we are greeted with a button to launch a scan, what is the name of this button?

`New Scan`

2. Nessus allows us to create custom templates that can be used during the scan selection as additional scan types, what is the name of the menu where we can set these?

`Policies`

3. Nessus also allows us to change plugin properties such as hiding them or changing their severity, what menu allows us to change this?

`Plugin Rules`

4. Nessus can also be run through multiple 'Scanners' where multiple installations can work together to complete scans or run scans on remote networks, what menu allows us to see all of these installations?

`Scanners`

5. Let's move onto the scan types, what scan allows us to see simply what hosts are 'alive'?

`Host Discovery`

6. One of the most useful scan types, which is considered to be 'suitable for any host'?

`Basic Network Scan`

7. Following a few basic scans, it's often useful to run a scan wherein the scanner can authenticate to systems and evaluate their patching level. What scan allows you to do this?

`Credentialed Patch Audit`

8. When performing Web App tests it's often useful to run which scan? This can be incredibly useful when also using nitko, zap, and burp to gain a full picture of an application.

`Web Application Tests`

## [Task 4] Scanning! 

Run a basic network scan and learn to read through the results!

1. Deploy the machine and connect to the network

`No answer needed`

2. Create a new 'Basic Network Scan' targeting the deployed VM. What option can we set under 'BASIC' to set a time for this scan to run? This can be very useful when network congestion is an issue.

`Schedule`

3. Under discovery set the scan to cover ports 1-65535. What is this type called?

`Port scan (all ports)`

4. As we are connected to the network via a VPN, it may be to our benefit to 'tone down' the scan a bit. What scan type can we change to under 'ADVANCED' for this lower bandwidth connection?

`Scan low bandwidth links`

5. With these options set (other than the time to run) save and launch the scan.

`No answer needed`

6. After the scan completes, which 'Vulnerability' can we view the details of to see the open ports on this host?

`Nessus SYN scanner`

7. There seems to be a chat server running on this machine, what port is it on?

`6667`

8. Looks like we have a medium level vulnerability relating to SSH, what is this vulnerability named?

`SSH Weak Algorithms Supports`

9. What web server type and version is reported by Nessus?

`Apache/2.4.99`

## [Task 5] Wait, there's mail? 

Add SMTP functionality into your Nessus install!

1. An optional but awesome additional step, link your Nessus box up to an SMTP server via the Settings panel. Google provides this for free if you already have a Gmail account. Adding 2-factor authentication on your account and create an app password, then link Nessus to the Gmail SMTP server via these following settings: https://www.siteground.com/kb/google_free_smtp_server/

`No answer needed`

## [Task 6] So you're telling me that's how you set up a web app... 

Run a Web App scan against a very secure web application that has absolutely no problems!

1. Run a web application scan against this new box.

`No answer needed`

2. What is the plugin id of the plugin that determines the HTTP server type and version?

`10107`

3. What authentication page is discovered by the scanner that transmits credentials in cleartext?

`login.php`

4. What is the file extension of the config backup?

`.bak`

5. Which directory contains example documents? (This will be in a php directory)

`/external/phpids/0.6/docs/examples/`

6. What vulnerability is this application susceptible to that is associated with X-Frame-Options?

`Clickjacking`

7. What version of php is the server using?

`5.5.9-1ubuntu4.26`

