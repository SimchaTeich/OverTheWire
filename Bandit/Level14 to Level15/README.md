# Level 14 → Level 15

## Level Goal
he password for the next level can be retrieved by submitting the password of the current level to **port 30000 on localhost**.

## Commands you may need to solve this level
ssh, telnet, nc, openssl, s_client, nmap

## Helpful Reading Material
[How the Internet works in 5 minutes (YouTube)](https://www.youtube.com/watch?v=7_LPdttKXPc) (Not completely accurate, but good enough for beginners)<br />
[IP Addresses](http://computer.howstuffworks.com/web-server5.htm)<br />
[IP Address on Wikipedia](https://en.wikipedia.org/wiki/IP_address)<br />
[Localhost on Wikipedia](https://en.wikipedia.org/wiki/Localhost)<br />
[Ports](http://computer.howstuffworks.com/web-server8.htm)<br />
[Port (computer networking) on Wikipedia)](https://en.wikipedia.org/wiki/Port_(computer_networking))

## Solution
```
ssh bandit14@bandit.labs.overthewire.org -p 2220
```
```
fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq
```
```
nc localhost 30000
```
```
fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq
```

## Password for the next level
```
jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt
```
