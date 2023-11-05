# Level 11 → Level 12

## Level Goal
The password for the next level is stored in the file **data.txt**, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

## Commands you may need to solve this level
grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

## Helpful Reading Material
[Rot13 on Wikipedia](https://en.wikipedia.org/wiki/Rot13)<br />


## Solution
```
ssh bandit11@bandit.labs.overthewire.org -p 2220
```
```
6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM
```
```
cat data.txt | tr "a-zA-Z" "n-za-mN-ZA-M"
```

## Password for the next level
```
JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv
```
