# Level 5 → Level 6

## Level Goal
The password for the next level is stored in a file somewhere under the **inhere** directory and has all of the following properties:

* human-readable
* 1033 bytes in size
* not executable

## Commands you may need to solve this level
ls , cd , cat , file , du , find

## Solution
```
ssh bandit5@bandit.labs.overthewire.org -p 2220
```
```
lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR
```
```
find ./inhere -size 1033c -type f ! -executable -exec file {} + | grep "ASCII text"
```
```
cat ./inhere/maybehere07/.file2
```

## Password for the next level
```
P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU
```
