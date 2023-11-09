# Level 26 → Level 27

## Level Goal
Good job getting a shell! Now hurry and grab the password for bandit27!

## Commands you may need to solve this level
ls

## Solution
```
ssh bandit26@bandit.labs.overthewire.org -p 2220
```
Don't forget to minimize the window:
```
c7GvcKlw9mC7aUQaPx7nwFstuAIBw1o1
```
Same actions from the previous challenge to open the shell:
```
v
```
```
:set shell=/bin/bash
```
```
:shell
```
And the following actions already belong to the current challenge:
```
ls
```
```
./bandit27-do
```
```
./bandit27-do id
```
```
./bandit27-do uid=11027 cat /etc/bandit_pass/bandit27
```

## Password for the next level
```
YnQpBuifNMas1hcUFk70ZmqkhUU2EuaS
```
