# Crack the hash

Cracking hashes challenges

[Crack the hash](https://tryhackme.com/room/crackthehash)

## Topic's

* Brute Forcing

## Level 1

Can you complete the level 1 tasks by cracking the hashes?

1. 48bb6e862e54f2a795ffc4e541caed4d

```
hashid -m 48bb6e862e54f2a795ffc4e541caed4d
```

```
Analyzing '48bb6e862e54f2a795ffc4e541caed4d'
[+] MD2 
[+] MD5 [Hashcat Mode: 0]
[+] MD4 [Hashcat Mode: 900]
[+] Double MD5 [Hashcat Mode: 2600]
[+] LM [Hashcat Mode: 3000]
[+] RIPEMD-128 
[+] Haval-128 
[+] Tiger-128 
[+] Skein-256(128) 
[+] Skein-512(128) 
[+] Lotus Notes/Domino 5 [Hashcat Mode: 8600]
[+] Skype [Hashcat Mode: 23]
[+] Snefru-128 
[+] NTLM [Hashcat Mode: 1000]
[+] Domain Cached Credentials [Hashcat Mode: 1100]
[+] Domain Cached Credentials 2 [Hashcat Mode: 2100]
[+] DNSSEC(NSEC3) [Hashcat Mode: 8300]
[+] RAdmin v2.x [Hashcat Mode: 9900]
```

```
hashcat -m 0 -a 3 48bb6e862e54f2a795ffc4e541caed4d -O --force
```

> 48bb6e862e54f2a795ffc4e541caed4d:`easy`

2. CBFDAC6008F9CAB4083784CBD1874F76618D2A97

```
hashid -m CBFDAC6008F9CAB4083784CBD1874F76618D2A97
```

```
Analyzing 'CBFDAC6008F9CAB4083784CBD1874F76618D2A97'
[+] SHA-1 [Hashcat Mode: 100]
[+] Double SHA-1 [Hashcat Mode: 4500]
[+] RIPEMD-160 [Hashcat Mode: 6000]
[+] Haval-160 
[+] Tiger-160 
[+] HAS-160 
[+] LinkedIn [Hashcat Mode: 190]
[+] Skein-256(160) 
[+] Skein-512(160)
```

```
hashcat -m 100 CBFDAC6008F9CAB4083784CBD1874F76618D2A97 /usr/share/wordlists/rockyou.txt --force
```

> cbfdac6008f9cab4083784cbd1874f76618d2a97:`password123`

3. 1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032

```
hashid -m 1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032
```

```
Analyzing '1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032'
[+] Snefru-256 
[+] SHA-256 [Hashcat Mode: 1400]
[+] RIPEMD-256 
[+] Haval-256 
[+] GOST R 34.11-94 [Hashcat Mode: 6900]
[+] GOST CryptoPro S-Box 
[+] SHA3-256 [Hashcat Mode: 5000]
[+] Skein-256 
[+] Skein-512(256)
```

```
hashcat -m 1400 1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032 /usr/share/wordlists/rockyou.txt -O --force
```

> 1c8bfe8f801d79745c4631d09fff36c82aa37fc4cce4fc946683d7b336b63032:`letmein`

4. $2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom

* [Hash Analyzer](https://www.tunnelsup.com/hash-analyzer/)

```
Hash: $2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom
Salt: Not Found
Hash type: bcrypt
Bit length:	 
Character length:	 
Character type: $2x$x$ followed by base64
Hash: eUznr71EeNkJkUlypTsgbX1H68wsRom
Salt: Dwt1BZj6pcyc3Dy1FWZ5ie
```

```
```

> 

5. 279412f945939ba78ce0758d3fd83daa

## Level 2 

This task increases the difficulty. All of the answers will be in the classic [rock you](https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt) password list.

You might have to start using hashcat here and not online tools. It might also be handy to look at some example hashes on [hashcats page](https://hashcat.net/wiki/doku.php?id=example_hashes).

1. Hash: `F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85`

2. Hash: `1DFECA0C002AE40B8619ECF94819CC1B`

3. Hash: `$6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02`.; Salt: `aReallyHardSalt`; Rounds: `5`

4. Hash: `e5d8870e5bdd26602cab8dbe07a942c8669e56d6`; Salt: `tryhackme`