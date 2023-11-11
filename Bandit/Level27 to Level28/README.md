# Level 27 → Level 28

## Level Goal
There is a git repository at **ssh://bandit27-git@localhost/home/bandit27-git/repo** via the port **2220**. The password for the user **bandit27-git** is the same as for the user **bandit27**.

Clone the repository and find the password for the next level.

## Commands you may need to solve this level
git

## Solution
```
ssh bandit27@bandit.labs.overthewire.org -p 2220
```
```
YnQpBuifNMas1hcUFk70ZmqkhUU2EuaS
```
```
mkdir /tmp/myRepo
```
```
cd /tmp/myRepo
```
```
git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
```
```
YnQpBuifNMas1hcUFk70ZmqkhUU2EuaS
```
```
ls -all
```
```
cd repo
```
```
ls -all
```
```
cat README
```
```
cd ~
```
```
rm -r /tmp/myRepo
```

## Password for the next level
```
AVanL161y9rsbcJIsFHuw35rjaOM19nR
```
