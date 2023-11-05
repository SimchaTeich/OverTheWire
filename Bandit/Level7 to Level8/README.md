# Level 7 → Level 8

## Level Goal
The password for the next level is stored in the file **data.txt** next to the word **millionth**

## Commands you may need to solve this level
[man](https://man7.org/linux/man-pages/man1/man.1.html), grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

## Solution
```
ssh bandit7@bandit.labs.overthewire.org -p 2220
```
```
z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S
```
```
cat data.txt | grep millionth
```

## Password for the next level
```
TESKZC0XvTetK0S9xNwm25STk5iWrBvP
```
