# Tor

A beginner orienteered guide on using the TOR network

* [Tor](https://tryhackme.com/room/torforbeginners)

## Unit 1 - Tor

Tor is a free and open-source software for enabling anonymous communication. Tor directs Internet traffic through a free, worldwide, volunteer overlay network consisting of more than seven thousand relays to conceal a user's location and usage from anyone conducting network surveillance or traffic analysis. Using Tor makes it more difficult to trace Internet activity to the user: this includes "visits to Web sites, online posts, instant messages, and other communication forms". Tor's intended use is to protect the personal privacy of its users, as well as their freedom and ability to conduct confidential communication by keeping their Internet activities unmonitored.

In penetration testing, there might be a need to conduct a full-fledged black-box test. This is a form of testing in which security professionals have to deal with such things as firewalls and other mechanisms of restriction on the customer’s side. In this case, the Tor network can be used in order to constantly change IP and DNS addresses and therefore successfully overcome any restrictions.

In this unit, we are going to install the Tor service and learn basic commands.

1. Run `apt-get install tor` to install/update your Tor packages

`No answer needed`

2. Run `service tor start` to start the Tor service

`No answer needed`

3. Run `service tor status` to check Tor's availability

`No answer needed`

4. Run `service tor stop` to stop the Tor service

`No answer needed`

## Unit 2 - Proxychains

![](https://i.imgur.com/rvVo73x.png)

Proxychains - a tool that forces any TCP connection made by any given application to follow through proxy like TOR or any other SOCKS4, SOCKS5 or HTTP(S) proxy.

Proxychains is widely used by pentesters during the reconnaissance stage (For example with nmap).

1. Let's start with running `apt install proxychains` to install/update proxychains tool

`No answer needed`

2. Now it's time to configure proxychains to work properly. Run `nano /etc/proxychains.conf` to edit the settings. (Note: You can use any text editing tool instead of nano)

`No answer needed`

3. We can now see, that most of the methods are under comment mark. You can read their description and decide on using one of them in the future. For this lesson let's uncomment `dynamic_chain` and comment others (simply put '#' to the left). Additionally, it is useful to uncomment proxy_dns in order to prevent DNS leak. Scroll through the document and see whenever you want to add some additional proxies at the bottom of the page (which is not required at this point).  Apply all the settings.

`No answer needed`

4. Now let's check our settings. Start the TOR service and run `proxychains firefox`. Usually, you are required to put 'proxychains' command before anything in order to force it to transfer data through Tor.

`No answer needed`

5. After the firefox has loaded, check if your IP address has changed with any website that provides such information. Also, try running a test on `dnsleaktest.com` and see if your DNS address changed too. NOTE: All other web browser windows should be closed before opening firefox through proxychains!

`No answer needed`

## Unit 3 - Tor browser

Tor browser, as seen from its name, is a browser that transfers all its traffic through TOR and by using firefox headers makes all Tor users look the same.

On a daily basis, Tor browser is useful for anyone who wants to keep their internet activities out of the hands of advertisers, ISPs, and web sites. That includes people getting around censorship restrictions in their country, police officers looking to hide their IP address or anyone else who doesn't want their browsing habits linked to them.

Install Tor browser on your system (It is not necessarily to do this on your Kali Machine).

* [Windows, Mac OS installation](https://www.torproject.org/)
* [Kali Linux guide](https://hackingpress.com/install-tor-on-kali-linux/#Step_1_Create_a_new_user)

1. Finish the installation

`No answer needed`

2. Launch the Tor Browser and set your privacy settings to Level 2 (Safer)

`No answer needed`

3. Access the website below and capture the flag by copying bitcoin address at the bottom of the page! [http://danielas3rtn54uwmofdo3x2bsdifr47huasnmbgqzfrec5ubupvtpid.onion/](http://danielas3rtn54uwmofdo3x2bsdifr47huasnmbgqzfrec5ubupvtpid.onion/)

`1K918TvvE4PMPzPuZT7zSDAQV4ZNUjHBm5`
