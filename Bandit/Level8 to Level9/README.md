# Level 8 → Level 9

## Level Goal
The password for the next level is stored in the file **data.txt** and is the only line of text that occurs only once

## Commands you may need to solve this level
grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

## Helpful Reading Material
[Piping and Redirection](https://ryanstutorials.net/linuxtutorial/piping.php)<br />


## Solution
```
ssh bandit8@bandit.labs.overthewire.org -p 2220
```
```
TESKZC0XvTetK0S9xNwm25STk5iWrBvP
```
```
cat data.txt | sort -r | uniq -c | grep " 1 "
```

## Password for the next level
```
EN632PlfYiZbn3PhVK3XOGSlNInNE00t
```
