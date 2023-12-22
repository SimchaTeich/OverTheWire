# Level 1 → Level 2

## Solution
```
ssh leviathan1@leviathan.labs.overthewire.org -p 2223
```
```
PPIfmI1qsA
```
```
ls -all
```
```
./check
```

![](0.png)

```
strings check
```

![](1.png)

![](2.png)

I know that systrings displays strings that are 4 or more characters long. Is the password less than that?

```
cat check
```

![](3.png)

![](4.png)
* I's sorry about this word...

```
bash
```
```
cat /etc/leviathan_pass/leviathan2
```

![](5.png)

## Password for the next level:
```
mEh5PNl10e
```
