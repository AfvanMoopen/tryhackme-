# Networking

Part of the Blue Primer series, learn the basics of networking

[Networking](https://tryhackme.com/room/bpnetworking)

## Topic's

* Networking

## Kinda like a street address, just cooler.

In a manner similar to streets and homes, computers and their respective communication networks must have a way to address their 'mail'. In the following set of questions, we'll investigate the various types of IPv4 addresses. Here's a reference guide for that various types of IP addresses:

IP Address Classes

![](https://i.imgur.com/K60atvR.png)

Private Address Space

![](https://i.imgur.com/3jBu61m.png)

Room icon made by Freepik

Enjoy the room! For future rooms and write-ups, follow @darkstar7471 on Twitter.

1. How many categories of IPv4 addresses are there?

`5`

2. Which type is for research? *Looking for a letter rather than a number here

`E`

3. How many private address ranges are there?

`3`

4. Which private range is typically used by businesses?

`A`

5. There are two common default private ranges for home routers, what is the first one?

`192.168.0.0`

6. How about the second common private home range?

`192.168.1.0`

7. How many addresses make up a typical class C range? Specifically a /24

`256`

8. Of these addresses two are reserved, what is the first addresses typically reserved as?

`Network`

9.  The very last address in a range is typically reserved as what address type?

`Broadcast`

10. A third predominant address type is typically reserved for the router, what is the name of this address type?

`Gateway`

11. Which address is reserved for testing on individual computers?

`127.0.0.1`

12. A particularly unique address is reserved for unroutable packets, what is that address? This can also refer to all IPv4 addresses on the local machine.

`0.0.0.0`

## Binary to Decimal

Binary conversion is essential to understand in order to properly manage computer networks. An IPv4 address consists of 32 bits split up into four sections of eight bits. For example, the address 192.168.1.12 translates to this:

**11000000 10101000 00000001 00001100**

When considering individual bit values, we can break down each octet further. For example, let's break down the second octet valuing 168:

![](https://i.imgur.com/lGhAEtT.png)

*This table provided above is useful to recreate when solving for decimal values. A blank variant has been provided below for copying:*

![](https://i.imgur.com/zbyG80a.png)

Convert the following binary values into decimal. I suggest doing this by hand with a sheet of paper as it's essential to practice and retain properly. **These have been split into two sections of four for readability, however, treat them as octets when solving for decimal values.**

1. 1001 0010

`146`

2. 0111 0111

`119`

3. 1111 1111

`255`

4. 1100 0101

`197`

5. 1111 0110

`246`

6. 0001 0011

`19`

7. 1000 0001

`129`

8. 0011 0001

`49`

9.  0111 1000

`120`

10. 1111 0000

`240`

11. 0011 1011

`59`

12. 0000 0111

`7`

## Decimal to Binary

Using the table provided within the previous task convert the following values to binary. **For the sake of preserving the full octet, pad the front of each answer with the appropriate amount of zeros.**


1. 238

`11101110`

2. 34

`00100010`

3. 123

`01111011`

4. 50

`00110010`

5. 255

`11111111`

6. 200

`11001000`

7. 10

`00001010`

8. 138

`10001010`

9.  1

`00000001`

10. 13

`00001101`

11. 250

`11111010`

12. 114

`01110010`

## Address Class Identification

Using the table provided in the first task, identify which class each of the following addresses belongs to.

1. 10.240.1.1

`A`

2. 150.10.15.0

`B`

3. 192.14.2.0

`C`

4. 148.17.9.1

`B`

5. 193.42.1.1

`C`

6. 126.8.156.0

`A`

7. 220.200.23.1

`C`

8. 230.230.45.58

`D`

9.  177.100.18.4

`B`

10. 119.18.45.0

`A`

11. 117.89.56.45

`A`

12. 215.45.45.0

`C`
