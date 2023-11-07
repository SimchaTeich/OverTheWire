# Level 18 → Level 19

## Level Goal
The password for the next level is stored in a file **readme** in the homedirectory. Unfortunately, someone has modified **.bashrc** to log you out when you log in with SSH.

## Commands you may need to solve this level
ssh, ls, cat

## Solution
```
ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme
```
```
hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg
```

## Password for the next level
```
awhqfNnAbc1naukrpqDYcF95h7HoMTrC
```
