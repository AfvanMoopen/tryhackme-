# Scripting

Learn basic scripting by solving some challenges!

[Scripting](https://tryhackme.com/room/scripting)

## Topic's

- Coding Python
- Python Scripting

## Appendix archive

Password: `1 kn0w 1 5h0uldn'7!`

## Task 1 [Easy] Base64

This file has been base64 encoded 50 times - write a script to retrieve the flag. Here is the general process to do this:

1. read input from the file
2. use function to decode the file
3. do process in a loop

Try do this in both Bash and Python!

1. What is the final string?

```py
import base64

#Open file
with open('b64.txt') as f:
    msg = f.read()

#Decode 50 times
for _ in range(50):
    msg = base64.b64decode(msg)

print(f"The flag is: {msg.decode('utf8')}")
```

```
kali@kali:~/CTFs/tryhackme/Scripting$ python3 task_1_1.py
The flag is: HackBack2019=
```

`HackBack2019=`

## Task 2 [Medium] Gotta Catch em All

You need to write a script that connects to this webserver on the correct port, do an operation on a number and then move onto the next port. Start your original number at 0.

The format is: operation, number, next port.

For example the website might display, **add 900 3212** which would be: add 900 and move onto port 3212.

Then if it was **minus 212 3499**, you'd minus 212 (from the previous number which was 900) and move onto the next port 3499

Do this until you the page response is STOP (or you hit port 9765).

Each port is also only live for 4 seconds. After that it goes to the next port. You might have to wait until port 1337 becomes live again...

Go to: http://<machines_ip>:3010 to start...

General Approach(it's best to do this using the sockets library in Python):

1. Create a socket in Python using the [sockets](https://docs.python.org/3/howto/sockets.html) library
2. Connect to the port
3. Send an operation
4. View response and continue

---

1. Once you have done all operations, what number do you get (rounded to 2 decimal places at the end of your calculation)?

[http://10.10.62.16:3010/](http://10.10.62.16:3010/)

```py
import socket
import time
import re
import sys

def Main():
    serverIP = sys.argv[1] #Get ip from user input
    serverPort = 1337
    oldNum = 0 #Start at 0 as per instruction

    while serverPort != 9765:
        try: #try until port 1337 available
            if serverPort == 1337:
                print(f"Connecting to {serverIP} waiting for port {serverPort} to become available...")

            #Create socket and connect to server
            s = socket.socket()
            s.connect((serverIP,serverPort))

            #Send get request to server
            gRequest = f"GET / HTTP/1.0\r\nHost: {serverIP}:{serverPort}\r\n\r\n"
            s.send(gRequest.encode('utf8'))

            #Retrieve data from get request
            while True:
                response = s.recv(1024)
                if (len(response) < 1):
                    break
                data = response.decode("utf8")

            #Format and assign the data into usable variables
            op, newNum, nextPort = assignData(data)
            #Perform given calculations
            oldNum = doMath(op, oldNum, newNum)
            #Display output and move on
            print(f"Current number is {oldNum}, moving onto port {nextPort}")
            serverPort = nextPort

            s.close()

        except:
            s.close()
            time.sleep(3) #Ports update every 4 sec
            pass

    print(f"The final answer is {round(oldNum,2)}")

def doMath(op, oldNum, newNum):
    if op == 'add':
        return oldNum + newNum
    elif op == 'minus':
        return oldNum - newNum
    elif op == 'divide':
        return oldNum / newNum
    elif op == 'multiply':
        return oldNum * newNum
    else:
        return None

def assignData(data):
    dataArr = re.split(' |\*|\n', data) #Split data with multi delim
    dataArr = list(filter(None, dataArr)) #Filter null strings
    #Assign the last 3 values of the data
    op = dataArr[-3]
    newNum = float(dataArr[-2])
    nextPort = int(dataArr[-1])

    return op, newNum, nextPort

if __name__ == '__main__':
    Main()
```

```
kali@kali:~/CTFs/tryhackme/Scripting$ python3 task_2_1.py 10.10.75.15
Connecting to 10.10.75.15 waiting for port 1337 to become available...
Connecting to 10.10.75.15 waiting for port 1337 to become available...
Connecting to 10.10.75.15 waiting for port 1337 to become available...
Current number is 900.0, moving onto port 23456
Current number is 898.0, moving onto port 8888
Current number is 3592.0, moving onto port 9823
Current number is 1796.0, moving onto port 9887
Current number is 2252.0, moving onto port 7823
Current number is 2209.0, moving onto port 10456
Current number is 4418.0, moving onto port 10457
Current number is 5295.0, moving onto port 40000
Current number is 5304.0, moving onto port 40200
Current number is 180336.0, moving onto port 8743
Current number is 180331.0, moving onto port 63890
Current number is 180331.0, moving onto port 38721
Current number is 180374.0, moving onto port 6632
Current number is 180352.0, moving onto port 29932
Current number is 60117.333333333336, moving onto port 29132
Current number is 541056.0, moving onto port 8773
Current number is 539856.0, moving onto port 1338
Current number is 539868.0, moving onto port 1876
Current number is 539868.0, moving onto port 34232
Current number is 539868.0, moving onto port 6783
Current number is 539768.0, moving onto port 4040
Current number is -1079536.0, moving onto port 5050
Current number is -1079236.0, moving onto port 9898
Current number is -107923.6, moving onto port 3232
Current number is -107933.6, moving onto port 10321
Current number is -107803.6, moving onto port 7709
Current number is -431214.4, moving onto port 9872
Current number is 5174572.800000001, moving onto port 32424
Current number is 1724857.6000000003, moving onto port 65513
Current number is 1723857.6000000003, moving onto port 3459
Current number is 1723880.6000000003, moving onto port 7832
Current number is 344776.12000000005, moving onto port 1111
Current number is 344768.12000000005, moving onto port 2222
Current number is 344769.12000000005, moving onto port 9765
The final answer is 344769.12
```

`344769.12`

## Task 3 [Hard] Encrypted Server Chit Chat

The VM you have to connect to has a UDP server running on port 4000. Once connected to this UDP server, send a UDP message with the payload "hello" to receive more information. You will find some sort of encryption(using the AES-GCM cipher). Using the information from the server, write a script to retrieve the flag. Here are some useful thingsto keep in mind:

1. sending and receiving data over a network is done in bytes
2. the PyCA encryption library and functions takes its inputs as bytes
3. AES GCM sends both encrypted plaintext and tag, and the server sends these values sequentially in the form of the encrypted plaintext followed by the tag

This machine may take up to 5 minutes to configure once deployed. Please be patient.

Use this general approach(use Python3 here as well):

- Use the Python sockets library to create a UDP socket and send the aforementioned packets to the server
- use the PyCA encyption library and follow the instructions from the server

1. What is the flag?

```py
import socket
import hashlib
import sys
from cryptography.hazmat.backends import default_backend
from cryptography.hazmat.primitives.ciphers import (
    Cipher, algorithms, modes
)

def Main():
    host = sys.argv[1] #Get ip from user input
    port = 4000
    server = (host, port)
    iv = b'secureivl337' #Hardcoded for ease
    key = b'thisisaverysecretkeyl337'

    #Create socket *No need to connect as using UDP
    s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

    #Get initial message
    s.sendto(b"hello", server)
    print(recv(s))
    #Get the rest of the information
    s.sendto(b"ready", server)
    data = recv(s)
    print(data)
    checksum = data[104:136].hex() #Convert to hex to make comparison easier

    #Loop flags until checksums match
    while True:
        #Get the cipher text
        s.sendto(b"final", server)
        cText = bytes(recv(s))
        #Get the tag
        s.sendto(b"final", server)
        tag = bytes(recv(s))
        #Decrypt
        pText = decrypt(key, iv, cText, tag)
        #Compare
        if hashlib.sha256(pText).hexdigest() != checksum:
            continue
        else:
            print(f"The flag is: {pText}")
            break

def recv(s):
    try:
        data = s.recv(1024)
        return data
    except Exception as e:
        print(str(e))

def decrypt(key, iv, cText, tag):
    #Create AES GCM decryptor object
    decryptor = Cipher(algorithms.AES(key), modes.GCM(iv, tag),
    backend = default_backend()).decryptor()
    #Return decrypted text
    return decryptor.update(cText) + decryptor.finalize()

if __name__ == '__main__':
    Main()
```

```
kali@kali:~/CTFs/tryhackme/Scripting$ python3 task_3.py 10.10.54.108
b"You've connected to the super secret server, send a packet with the payload ready to receive more information"
b"key:thisisaverysecretkeyl337 iv:secureivl337 to decrypt and find the flag that has a SHA256 checksum of ]w\xf0\x18\xd2\xbfwx`T\x86U\xd8Ms\x82\xdc'\xd6\xce\x81n\xdeh\xf6]rb\x14c\xd9\xda send final in the next payload to receive all the encrypted flags"
The flag is: b'THM{eW-sCrIpTiNg-AnD-cRyPtO}'
```

`THM{eW-sCrIpTiNg-AnD-cRyPtO}`
