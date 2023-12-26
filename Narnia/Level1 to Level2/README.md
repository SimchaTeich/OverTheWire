# Level 1 → Level 2

## Solution
```
ssh narnia1@narnia.labs.overthewire.org -p 2226
```
```
eaa6AjYMBB
```
```
cd /narnia ; ls -al
```
```
cat narnia1.c
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

int main(){
    int (*ret)();

    if(getenv("EGG")==NULL){
        printf("Give me something to execute at the env-variable EGG\n");
        exit(1);
    }

    printf("Trying to execute EGG!\n");
    ret = getenv("EGG");
    ret();

    return 0;
}
```

I already know this form of `int (*ret)();`, it's a **pointer** to function. Because the `getenv` function returns an address (to the contents of `EGG`, actually to some string), we'll want to make `EGG` a type of memory pointer that prints the password.


## Password for the next level:
```

```
