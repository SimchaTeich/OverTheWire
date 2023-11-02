# Level 1 → Level 2

## Level Goal
The password for the next level is stored in a file called `-` located in the home directory

## Commands you may need to solve this level
ls , cd , cat , file , du , find

## Helpful Reading Material
[Google Search for “dashed filename”](https://www.google.com/search?q=dashed+filename)<br />
[Advanced Bash-scripting Guide - Chapter 3 - Special Characters](http://tldp.org/LDP/abs/html/special-chars.html)

## Solution
```
ssh bandit1@bandit.labs.overthewire.org -p 2220
```
```
NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL
```
```
ls
```
```
cat ./-
```

## Password for the next level
```
rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi
```
