# Level 17 → Level 18

## Level Goal
There are 2 files in the homedirectory: **passwords.old and passwords.new**. The password for the next level is in **passwords.new** and is the only line that has been changed between **passwords.old and passwords.new**

**NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19**

## Commands you may need to solve this level
cat, grep, ls, diff

## Solution
```
ssh bandit17@bandit.labs.overthewire.org -p 2220
```
```
VwOSWtCA7lRKkTfbr2IDh6awj9RNZM5e
```
```
diff passwords.old passwords.new
```

## Password for the next level
```
hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg
```
