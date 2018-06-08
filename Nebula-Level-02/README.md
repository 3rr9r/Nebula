# Nebula Level 02

## Info

Login: 		level02 / level02

Topic: 		Arbitrary file execution

## Description

The Code below allows an arbitrary file execution.

```c
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/types.h>
#include <stdio.h>

int main(int argc, char **argv, char **envp)
{
  char *buffer;

  gid_t gid;
  uid_t uid;

  gid = getegid();
  uid = geteuid();

  setresgid(gid, gid, gid);
  setresuid(uid, uid, uid);

  buffer = NULL;

  asprintf(&buffer, "/bin/echo %s is cool", getenv("USER"));
  printf("about to call system(\"%s\")\n", buffer);
  
  system(buffer);
}

```

## Ideas

The program is handling the Environment variable `USER`. Maybe i can change that from level02 to flag02. I need a shell running somehow. 

### Analyzation 

1. The function `asprintf()` is writing the string `"/bin/echo level02 is cool" ` into `buffer` . Which points to the very first adress of buffer.
2. `printf()` is getting its input form the string`"about to call ...."` and `buffer` . 
3. And `system(buffer)` is doing what exactly? Its getting its input from buffer.  Buffer is filled with `/bin/echo level02 is cool` .

So maybe it's working like in the first nebula exercise? We need to get `system()` to run the code we want!



I'm playing around with the environment varibale `USER`. I'm setting it to flag02 and /bin/bash... then export it. `System()`is executing`/bin/echo` from the buffer and getting `USER` as parameter... So if i change `USER` to `/bin/bash` that's not helping. Because it would just echo the string. If i could somehow bend what's executed... Or maybe something how `printf()`parses variables... Or maybe move the pointer from buffer. So that it is not pointing to the first character but to one want. If that would work `system()` would get the buffer from a point i want. It could get something like `/bin/echo /bin/bash` start with the second comand instead of the first...

1. I should find out more about `asprintf()`
   1. get to know how its parsing variables etc
2. get to know how syscalls via `system()` are working
3. have a break and  breakfast.



## Solution

My ideas where right to change the `USER`-variable. Change it to `;/bin/sh;` and run the program. Then you're in.

### How is that working?

The `system()` function calls system commands. By setting the `USER` to `;/bin/sh;`  we are telling it to open a shell. This is working because the semicolon `;` tells `system()` that a new statement is beginning. 

That's it.