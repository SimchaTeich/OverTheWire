# Level 12 → Level 13

## Level Goal
The password for the next level is stored in the file **data.txt**, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)

## Commands you may need to solve this level
grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd, mkdir, cp, mv, file

## Helpful Reading Material
[Hex dump on Wikipedia](https://en.wikipedia.org/wiki/Hex_dump)<br />


## Solution
```
ssh bandit12@bandit.labs.overthewire.org -p 2220
```
```
JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv
```
```
mkdir /tmp/myFolder/
```
```
xxd -r data.txt > /tmp/myFolder/data.bin
```
```
cd /tmp/myFolder
```
```
mv data.bin data.gz
```
```
gzip -d data.gz
```
```
mv data data2.bz
```
```
bzip2 -d data2.bz
```
```
mv data2 data3.gz
```
```
gzip -d data3.gz
```
```
mv data3 data4.tar
```
```
tar -x -f data4.tar
```
```
mv data5.bin data5.tar
```
```
tar -x -f data5.tar
```
```
mv data6.bin data6.bz
```
```
bzip2 -d data6.bz
```
```
mv data6 data7.tar
```
```
tar -x -f data7.tar
```
```
mv data8.bin data8.gz
```
```
gzip -d data8.gz
```
```
mv data8 data9.txt
```
```
cat data9.txt
```

## Password for the next level
```
wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw
```
