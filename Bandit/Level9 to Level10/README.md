# Level 9 → Level 10

## Level Goal
The password for the next level is stored in the file **data.txt** in one of the few human-readable strings, preceded by several ‘=’ characters.

## Commands you may need to solve this level
grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

## Solution
```
ssh bandit9@bandit.labs.overthewire.org -p 2220
```
```
EN632PlfYiZbn3PhVK3XOGSlNInNE00t
```
```
strings data.txt | grep '=='
```

## Password for the next level
```
G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s
```
