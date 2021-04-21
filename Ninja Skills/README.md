# Ninja Skills

Practise your Linux skills and complete the challenges.

[Ninja Skills](https://tryhackme.com/room/ninjaskills)

## Topic's

* Linux find Command

## Ninja Skills

![](https://i.imgur.com/JbCoSfv.png)

Let's have some fun with Linux. Deploy the machine and get started.

**This machine may take up to 3 minutes to configure.**

(If you prefer to SSH into the machine, use the credentials `new-user` as the username and password)

Answer the questions about the following files:

* 8V2L
* bny0
* c4ZX
* D8B3
* FHl1
* oiMO
* PFbD
* rmfX
* SRSq
* uqyw
* v2Vb
* X1Uy

The aim is to answer the questions as efficiently as possible.

```
find / -type f \( -name 8V2L -o -name bny0 -o -name c4ZX -o -name D8B3 -o -name FHl1 -o -name oiMO -o -name PFbD -o -name rmfX -o -name SRSq -o -name uqyw -o -name v2Vb -o -name X1Uy \) 2>>/dev/null

/mnt/D8B3
/mnt/c4ZX
/var/FHl1
/var/log/uqyw
/opt/PFbD
/opt/oiMO
/media/rmfX
/etc/8V2L
/etc/ssh/SRSq
/home/v2Vb
/X1Uy
```

1. Which of the above files are owned by the best-group group(enter the answer separated by spaces in alphabetical order)

```
find / -type f \( -name 8V2L -o -name bny0 -o -name c4ZX -o -name D8B3 -o -name FHl1 -o -name oiMO -o -name PFbD -o -name rmfX -o -name SRSq -o -name uqyw -o -name v2Vb -o -name X1Uy \) -exec ls -ilrt {} \; 2>>/dev/null

268017 -rw-rw-r-- 1 new-user best-group 13545 Oct 23  2019 /mnt/D8B3
268022 -rw-rw-r-- 1 new-user new-user 13545 Oct 23  2019 /mnt/c4ZX
268016 -rw-rw-r-- 1 new-user new-user 13545 Oct 23  2019 /var/FHl1
268021 -rw-rw-r-- 1 new-user new-user 13545 Oct 23  2019 /var/log/uqyw
268023 -rw-rw-r-- 1 new-user new-user 13545 Oct 23  2019 /opt/PFbD
268024 -rw-rw-r-- 1 new-user new-user 13545 Oct 23  2019 /opt/oiMO
268020 -rw-rw-r-- 1 new-user new-user 13545 Oct 23  2019 /media/rmfX
268019 -rwxrwxr-x 1 new-user new-user 13545 Oct 23  2019 /etc/8V2L
268012 -rw-rw-r-- 1 new-user new-user 13545 Oct 23  2019 /etc/ssh/SRSq
268014 -rw-rw-r-- 1 new-user best-group 13545 Oct 23  2019 /home/v2Vb
268018 -rw-rw-r-- 1 newer-user new-user 13545 Oct 23  2019 /X1Uy
```

`D8B3 v2Vb`

2. Which of these files contain an IP address?

```
find / -type f \( -name 8V2L -o -name bny0 -o -name c4ZX -o -name D8B3 -o -name FHl1 -o -name oiMO -o -name PFbD -o -name rmfX -o -name SRSq -o -name uqyw -o -name v2Vb -o -name X1Uy \) -exec egrep -o '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' * {} \; 2>>/dev/null

/opt/oiMO:1.1.1.1
```

`oiMO`

3. Which file has the SHA1 hash of 9d54da7584015647ba052173b84d45e8007eba94

```
find / -type f \( -name 8V2L -o -name bny0 -o -name c4ZX -o -name D8B3 -o -name FHl1 -o -name oiMO -o -name PFbD -o -name rmfX -o -name SRSq -o -name uqyw -o -name v2Vb -o -name X1Uy \) -exec sha1sum {} \; 2>>/dev/null

2c8de970ff0701c8fd6c55db8a5315e5615a9575  /mnt/D8B3
9d54da7584015647ba052173b84d45e8007eba94  /mnt/c4ZX
d5a35473a856ea30bfec5bf67b8b6e1fe96475b3  /var/FHl1
57226b5f4f1d5ca128f606581d7ca9bd6c45ca13  /var/log/uqyw
256933c34f1b42522298282ce5df3642be9a2dc9  /opt/PFbD
5b34294b3caa59c1006854fa0901352bf6476a8c  /opt/oiMO
4ef4c2df08bc60139c29e222f537b6bea7e4d6fa  /media/rmfX
0323e62f06b29ddbbe18f30a89cc123ae479a346  /etc/8V2L
acbbbce6c56feb7e351f866b806427403b7b103d  /etc/ssh/SRSq
7324353e3cd047b8150e0c95edf12e28be7c55d3  /home/v2Vb
59840c46fb64a4faeabb37da0744a46967d87e57  /X1Uy
```

`c4ZX`

4. Which file contains 230 lines?

```
find / -type f \( -name 8V2L -o -name bny0 -o -name c4ZX -o -name D8B3 -o -name FHl1 -o -name oiMO -o -name PFbD -o -name rmfX -o -name SRSq -o -name uqyw -o -name v2Vb -o -name X1Uy \) -exec grep wc -l 230 {} \; 2>>/dev/null

/mnt/D8B3
/mnt/c4ZX
/var/log/uqyw
/opt/PFbD
/opt/oiMO
/media/rmfX
/etc/8V2L
/etc/ssh/SRSq
/home/v2Vb
/X1Uy
```

`bny0`

5. Which file's owner has an ID of 502?

```
find / -type f \( -name 8V2L -o -name bny0 -o -name c4ZX -o -name D8B3 -o -name FHl1 -o -name oiMO -o -name PFbD -o -name rmfX -o -name SRSq -o -name uqyw -o -name v2Vb -o -name X1Uy \) -exec ls -ln {} \; 2>>/dev/null

-rw-rw-r-- 1 501 502 13545 Oct 23  2019 /mnt/D8B3
-rw-rw-r-- 1 501 501 13545 Oct 23  2019 /mnt/c4ZX
-rw-rw-r-- 1 501 501 13545 Oct 23  2019 /var/FHl1
-rw-rw-r-- 1 501 501 13545 Oct 23  2019 /var/log/uqyw
-rw-rw-r-- 1 501 501 13545 Oct 23  2019 /opt/PFbD
-rw-rw-r-- 1 501 501 13545 Oct 23  2019 /opt/oiMO
-rw-rw-r-- 1 501 501 13545 Oct 23  2019 /media/rmfX
-rwxrwxr-x 1 501 501 13545 Oct 23  2019 /etc/8V2L
-rw-rw-r-- 1 501 501 13545 Oct 23  2019 /etc/ssh/SRSq
-rw-rw-r-- 1 501 502 13545 Oct 23  2019 /home/v2Vb
-rw-rw-r-- 1 502 501 13545 Oct 23  2019 /X1Uy
```

`X1Uy`

6. Which file is executable by everyone?

```
find / -type f \( -name 8V2L -o -name bny0 -o -name c4ZX -o -name D8B3 -o -name FHl1 -o -name oiMO -o -name PFbD -o -name rmfX -o -name SRSq -o -name uqyw -o -name v2Vb -o -name X1Uy \) -exec ls -ln {} \; 2>>/dev/null

-rw-rw-r-- 1 501 502 13545 Oct 23  2019 /mnt/D8B3
-rw-rw-r-- 1 501 501 13545 Oct 23  2019 /mnt/c4ZX
-rw-rw-r-- 1 501 501 13545 Oct 23  2019 /var/FHl1
-rw-rw-r-- 1 501 501 13545 Oct 23  2019 /var/log/uqyw
-rw-rw-r-- 1 501 501 13545 Oct 23  2019 /opt/PFbD
-rw-rw-r-- 1 501 501 13545 Oct 23  2019 /opt/oiMO
-rw-rw-r-- 1 501 501 13545 Oct 23  2019 /media/rmfX
-rwxrwxr-x 1 501 501 13545 Oct 23  2019 /etc/8V2L
-rw-rw-r-- 1 501 501 13545 Oct 23  2019 /etc/ssh/SRSq
-rw-rw-r-- 1 501 502 13545 Oct 23  2019 /home/v2Vb
-rw-rw-r-- 1 502 501 13545 Oct 23  2019 /X1Uy
```

`8V2L`
