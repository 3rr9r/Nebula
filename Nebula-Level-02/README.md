# Nebula Level 02

## Info

Login: 		level02 / level02

Topic: 		Arbitrary file execution

## Description

Te Code below allows an arbitrary file execution.

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

The program is handling the Environment variable `USER`. Maybe i can change that form level02 to flag02. I need a shell running somehow. 

The function `asprintf()` is writing the string `"/bin/echo level02 is cool" ` into `buffer` . It points to the very first adress of buffer. `printf()` is getting its input form the string`"about to call ...."` and `buffer` . And `system(buffer)` is doing what exactly? Its getting its input from buffer.  Buffer is filled with `/bin/echo level02 is cool` . So maybe it's working like in the first nebula? exercise? We need to get `system()` to run the code we want!



## Solution



