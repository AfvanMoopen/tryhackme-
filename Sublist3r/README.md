# Sublist3r

Part of the Red Primer series, learn how to find subdomains with Sublist3r!

[Sublist3r](https://tryhackme.com/room/rpsublist3r)

## Topic's

- Sublist3r Fundamentals
- OSINT
- Web Enumeration

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Intro

Reconnaissance, the first step of a pentest, is arguably the most important step. Discovering the total attack surface of your target is critical, especially if you're performing phishing and miss a portal that you can use to login. Sublist3r is a fantastic python script that allows us to perform quick and easy recon against our target, discovering various subdomains associated with the websites/domains in scope.

![Sublist3r](https://i.imgur.com/00ySGQu.png)

**Disclaimer!** There's a pretty good chance your ISP isn't going to like recon activities and neither will most search engines. As such, I've provided a text document with the full terminal output from a Sublist3r run as a download for task four. You can still run this program and you're likely not going to get into trouble, however, you'll likely have a temporary CAPTCHA imposed on your Google searches for the next hour if you run this script.

You can also use this site if you don't want to run Sublist3r: [https://dnsdumpster.com/](https://dnsdumpster.com/)

1. You can find Sublist3r [here](https://github.com/aboul3la/Sublist3r)! We'll install this in the next task.

`No answer needed`

## Installation

The GitHub repository for Sublist3r can be found here: [https://github.com/aboul3la/Sublist3r](https://github.com/aboul3la/Sublist3r)

We'll be installing Sublist3r in the opt directory, the standard directory for optional package installation on Linux.

You'll need to have either Python 3.4.x (or higher) or Python 2.7.x installed. We'll be using Python 3 commands for this room.

1. First, let's change to our opt directory: cd /opt

`No answer needed`

2. Next, let's clone the Sublist3r repository into opt: git clone https://github.com/aboul3la/Sublist3r.git

`No answer needed`

3. Now let's move into the Sublist3r directory we've just created: cd /opt/Sublist3r

`No answer needed`

4. Finally, let's install the requirements for running Sublist3r: pip3 install -r requirements.txt

`No answer needed`

## Switchboard

Sublist3r has a number of switches that we can use to do everything from setting our target domain to changing which engine to use for searching. You can access this via running Sublist3r with only the help switch: -h

1. What switch can we use to set our target domain to perform recon on?

`-d`

2. How about setting which engines we'll use for searching? (i.e. google, bing, etc)

`-e`

3. Saving our output is important both so we don't have to run recon again but also so we can return to our returns and review them at a later time. What switch do we use to define an output file?

`-o`

4. Sublist3r can sometimes take some time to run but we can speed through up the use of threads. Which switch allows us to set the number of threads?

`-t`

5. Last but not least, we can also bruteforce the domains for our target. This isn't always the most useful, however, it can sometimes find a key domain that we might have missed. What switch allows us to enable brute forcing?

`-b`

## Scans away!

Time to scan, let's run Sublit3r against a target company domain and learn about some common domains! You can also run this via the recon tool at [https://dnsdumpster.com/](https://dnsdumpster.com/) or you can also just download my results from running sublist3r.

1. Let's run sublist3r now against nbc.com, a fairly large American news company. Run this now with the command: python3 sublist3r.py -d nbc.com -o sub-output-nbc.txt

```
python3 /opt/Sublist3r/sublist3r.py -d nbc.com -o sub-output-nbc.txt


                 ____        _     _ _     _   _____
                / ___| _   _| |__ | (_)___| |_|___ / _ __
                \___ \| | | | '_ \| | / __| __| |_ \| '__|
                 ___) | |_| | |_) | | \__ \ |_ ___) | |
                |____/ \__,_|_.__/|_|_|___/\__|____/|_|

                # Coded By Ahmed Aboul-Ela - @aboul3la

[-] Enumerating subdomains now for nbc.com
[-] Searching now in Baidu..
[-] Searching now in Yahoo..
[-] Searching now in Google..
[-] Searching now in Bing..
[-] Searching now in Ask..
[-] Searching now in Netcraft..
[-] Searching now in DNSdumpster..
[-] Searching now in Virustotal..
[-] Searching now in ThreatCrowd..
[-] Searching now in SSL Certificates..
[-] Searching now in PassiveDNS..
[-] Saving results to file: sub-output-nbc.txt
[-] Total Unique Subdomains Found: 229
msnbc.com
www.nbc.com
30rock.nbc.com
acc-api.nbc.com
acc-img.nbc.com
acc-m.nbc.com
acc-www.nbc.com
adminmx.nbc.com
ads.nbc.com
agtvote.nbc.com
altaec1.nbc.com
altaec2.nbc.com
altany6.nbc.com
altany7.nbc.com
api.nbc.com
app.nbc.com
apt.nbc.com
www.apt.nbc.com
apt2gostgaoa-ltm.nbc.com
aptaoa-ltm.nbc.com
aptnewprod2.nbc.com
aptstgaoa-ltm.nbc.com
blog.nbc.com
blogs.nbc.com
boards.nbc.com
www.boards.nbc.com
stage.boards.nbc.com
www.stage.boards.nbc.com
dev.nbc.com
dev-id.nbc.com
dev-www.nbc.com
dmzmarsapps.nbc.com
dmzmarsapps1.nbc.com
dmzmarsapps2.nbc.com
dmzmarsapps21.nbc.com
dmzmarsapps22.nbc.com
e.nbc.com
eastnet.nbc.com
ecstgnbcessowebapps.nbc.com
www.ecstgnbcessowebapps.nbc.com
edit.nbc.com
o92.em.nbc.com
email.nbc.com
links.email.nbc.com
o1.email.nbc.com
o2.email.nbc.com
o3.email.nbc.com
o4.email.nbc.com
o5.email.nbc.com
o6.email.nbc.com
o104.emails.nbc.com
o105.emails.nbc.com
o106.emails.nbc.com
o668.emails.nbc.com
o95.emails.nbc.com
events.nbc.com
forum.nbc.com
frd.nbc.com
friendship.nbc.com
help.nbc.com
heroes.nbc.com
id.nbc.com
img.nbc.com
ip129.nbc.com
ip130.nbc.com
ip132.nbc.com
ip133.nbc.com
ip134.nbc.com
ip135.nbc.com
ip136.nbc.com
ip137.nbc.com
ip138.nbc.com
ip139.nbc.com
ip140.nbc.com
ip141.nbc.com
ip143.nbc.com
ip144.nbc.com
ip145.nbc.com
ip147.nbc.com
ip149.nbc.com
ip151.nbc.com
ip153.nbc.com
ip154.nbc.com
ip155.nbc.com
ip156.nbc.com
ip157.nbc.com
ip158.nbc.com
links.nbc.com
login.nbc.com
m.nbc.com
mail.nbc.com
click.mail.nbc.com
image.mail.nbc.com
mta.mail.nbc.com
pages.mail.nbc.com
view.mail.nbc.com
mailer1.nbc.com
mailer2.nbc.com
mailer5.nbc.com
mailer6.nbc.com
mobile.nbc.com
mp.nbc.com
map.mp.nbc.com
musiccues.nbc.com
my.nbc.com
www.nbc6.nbc.com
nbcaccess.nbc.com
nbcessowebapps.nbc.com
www.nbcessowebapps.nbc.com
nbcsportsgroup-score-portal.nbc.com
www.nbcsportsgroup-score-portal.nbc.com
olympic.nbc.com
www.olympic.nbc.com
olympicsinvitations2.nbc.com
origin-www.nbc.com
ovation.nbc.com
passions.nbc.com
playtotv.nbc.com
nbc-agt-s13.playtotv.nbc.com
nbc-agt-s13-acceptance.playtotv.nbc.com
nbc-agt-s13-dev.playtotv.nbc.com
nbc-agt-s13-staging.playtotv.nbc.com
nbc-agt-s14.playtotv.nbc.com
nbc-agt-s14-acceptance.playtotv.nbc.com
nbc-agt-s14-dev.playtotv.nbc.com
nbc-agt-s14-staging.playtotv.nbc.com
director.nbc-agt-s15.playtotv.nbc.com
director.nbc-agt-s15-staging.playtotv.nbc.com
director.nbc-thevoice-loadtest.playtotv.nbc.com
nbc-thevoice-s15.playtotv.nbc.com
nbc-thevoice-s15-acceptance.playtotv.nbc.com
nbc-thevoice-s15-dev.playtotv.nbc.com
nbc-thevoice-s15-staging.playtotv.nbc.com
nbc-thevoice-s16.playtotv.nbc.com
nbc-thevoice-s16-acceptance.playtotv.nbc.com
nbc-thevoice-s16-dev.playtotv.nbc.com
nbc-thevoice-s16-staging.playtotv.nbc.com
nbc-thevoice-s17.playtotv.nbc.com
nbc-thevoice-s17-acceptance.playtotv.nbc.com
nbc-thevoice-s17-dev.playtotv.nbc.com
nbc-thevoice-s17-staging.playtotv.nbc.com
director.nbc-thevoice-s18.playtotv.nbc.com
director.nbc-thevoice-s18-staging.playtotv.nbc.com
director.nbc-thevoice-s19-staging.playtotv.nbc.com
backend.nbc-voting-staging.playtotv.nbc.com
director.nbc-voting-staging.playtotv.nbc.com
prod-tsjf-www.nbc.com
prod-www.nbc.com
secure.nbc.com
secure-uat.nbc.com
sportsevents.nbc.com
www.sportsevents.nbc.com
ssologin.nbc.com
www.ssologin.nbc.com
stage.nbc.com
stage-id.nbc.com
stage-img.nbc.com
stage-www.nbc.com
login.stg.nbc.com
ssologin.stg.nbc.com
www.ssologin.stg.nbc.com
studiopass.nbc.com
tbll.nbc.com
apt.telemundo.nbc.com
video.nbc.com
virtual.nbc.com
vote.nbc.com
agt.vote.nbc.com
backend.agt.vote.nbc.com
director.agt.vote.nbc.com
backend.agt-dev.vote.nbc.com
director.agt-dev.vote.nbc.com
backend.agt-staging.vote.nbc.com
director.agt-staging.vote.nbc.com
backend.nbc-voting-dev-client.vote.nbc.com
director.nbc-voting-dev-client.vote.nbc.com
nbc-voting-staging.vote.nbc.com
backend.nbc-voting-staging.vote.nbc.com
backend-origin.nbc-voting-staging.vote.nbc.com
director.nbc-voting-staging.vote.nbc.com
director-origin.nbc-voting-staging.vote.nbc.com
nbc-voting-staging-director.vote.nbc.com
nbc-voting-waftest.vote.nbc.com
backend.nbc-voting-waftest.vote.nbc.com
backend-origin.nbc-voting-waftest.vote.nbc.com
director.nbc-voting-waftest.vote.nbc.com
director-origin.nbc-voting-waftest.vote.nbc.com
nbc-voting-waftest-director.vote.nbc.com
voice.vote.nbc.com
backend.voice.vote.nbc.com
backend-origin.voice.vote.nbc.com
director.voice.vote.nbc.com
director-origin.voice.vote.nbc.com
backend.voice-dev.vote.nbc.com
director.voice-dev.vote.nbc.com
voice-director.vote.nbc.com
backend.voice-staging.vote.nbc.com
director.voice-staging.vote.nbc.com
agtappvote.votenow.nbc.com
agtappvote-dev.votenow.nbc.com
agtappvote-test.votenow.nbc.com
agtsave.votenow.nbc.com
agtsave-dev.votenow.nbc.com
agtsave-test.votenow.nbc.com
agtstbvote.votenow.nbc.com
agtstbvote-dev.votenow.nbc.com
agtstbvote-test.votenow.nbc.com
agtvote.votenow.nbc.com
agtvote-dev.votenow.nbc.com
agtvote-test.votenow.nbc.com
btfvote.votenow.nbc.com
ns1.votenow.nbc.com
ns2.votenow.nbc.com
ns3.votenow.nbc.com
ns4.votenow.nbc.com
secure.votenow.nbc.com
voicevote.votenow.nbc.com
webxcn1nbcge.nbc.com
webxcn2nbcge.nbc.com
webxpn1nbcge.nbc.com
webxpn2nbcge.nbc.com
widget.nbc.com
apt.wip.nbc.com
ssologin.wip.nbc.com
ssologin.stg.wip.nbc.com
office-words.www.nbc.com
yourgarage.nbc.com
xn--12-nbc.com
www.xn--12-nbc.com
```

`No answer needed`

2. Once that completes open up your results and take a look through them. Email domains are almost always interesting and typically have an email portal (usually Outlook) located at them. Which subdomain is likely the email portal?

> mail.nbc.com

`mail`

3. Administrative control panels should never be exposed to the internet! Which subdomain is exposed that shouldn't be?

`admin`

1. Company blogs can sometimes reveal information about internal activities, which subdomain has the company blog at it?

`blog`

2. Development sites are often vulnerable to information disclosure or full-blown attacks. Two developer sites are exposed, which one is associated directly with web development?

> dev-www.nbc.com

`dev-www`

3. Customer and employee help desk portals can often reveal internal nomenclature and other potentially sensitive information, which dns record might be a helpdesk portal?

> help.nbc.com

`help`

4. Single sign-on is a feature commonly used in corporate domains, which dns record is directly associated with this feature? Include both parts of this subdomain separated by a period.

> ssologin.stg.nbc.com

`ssologin.stg`

1. One last one for fun. NBC produced a popular sitcom about typical office work environment, which dns record might be associated with this show?

> office-words.www.nbc.com

`office-words`
