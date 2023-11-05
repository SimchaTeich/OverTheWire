# Level 6 → Level 7

## Level Goal
The password for the next level is stored **somewhere on the server** and has all of the following properties:

* owned by user bandit7
* owned by group bandit6
* 33 bytes in size

## Commands you may need to solve this level
ls , cd , cat , file , du , find , grep

## Solution
```
ssh bandit6@bandit.labs.overthewire.org -p 2220
```
```
P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU
```
```
find / -group bandit6 -user bandit7 -size 33c -exec cat {} + 2>/dev/null # try it without the last part...
```

## Password for the next level
```
z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S
```
