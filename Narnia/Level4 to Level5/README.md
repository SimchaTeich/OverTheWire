# Level 4 → Level 5

## Solution
```
ssh narnia4@narnia.labs.overthewire.org -p 2226
```
```
aKNxxrpDc1
```
```
cd /narnia ; ls -al
```
```
./narnia4
```

![](0.png)

Let's look at the code:

```
cat narnia4.c
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

#include <string.h>
#include <stdlib.h>
#include <stdio.h>
#include <ctype.h>

extern char **environ;

int main(int argc,char **argv){
    int i;
    char buffer[256];

    for(i = 0; environ[i] != NULL; i++)
        memset(environ[i], '\0', strlen(environ[i]));

    if(argc>1)
        strcpy(buffer,argv[1]);

    return 0;
}
```

In challenge `2->3`, after I finished I saw a solution to the challenge in a YouTube video (I attached the link to the video under the title **Appendix**). In the video there was a way to solve the challenge using environment variables. In the current challenge, it seems that the file deletes all environment variables.

But since my solution did not include environment variables - so it also fits here (with necessary changes such as addresses)

Let's get started:



## Password for the next level:
```

```
