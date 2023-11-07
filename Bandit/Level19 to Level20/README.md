# Level 19 → Level 20

## Level Goal
To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

## Helpful Reading Material
[setuid on Wikipedia](https://en.wikipedia.org/wiki/Setuid)


## Solution
```
ssh bandit19@bandit.labs.overthewire.org -p 2220
```
```
awhqfNnAbc1naukrpqDYcF95h7HoMTrC
```
```
ls
```
```
./bandit20-do
```
```
./bandit20-do id
```
```
./bandit20-do euid=11020 cat /etc/bandit_pass/bandit20
```

## Password for the next level
```
VxCazJaVykI6W36BkBU0mJTCM8rR95XT
```
