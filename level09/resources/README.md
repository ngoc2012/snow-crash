# level09
## Introduction

...

## Observations

level09@SnowCrash:~$ ls
level09  token
level09@SnowCrash:~$ ltrace ./level09 
__libc_start_main(0x80487ce, 1, 0xbffff6e4, 0x8048aa0, 0x8048b10 <unfinished ...>
ptrace(0, 0, 1, 0, 0xb7e2fe38)               = -1
puts("You should not reverse this"You should not reverse this
)          = 28
+++ exited (status 1) +++
level09@SnowCrash:~$ ltrace ./level09 token
__libc_start_main(0x80487ce, 2, 0xbffff6d4, 0x8048aa0, 0x8048b10 <unfinished ...>
ptrace(0, 0, 1, 0, 0xb7e2fe38)               = -1
puts("You should not reverse this"You should not reverse this
)          = 28
+++ exited (status 1) +++

level09@SnowCrash:~$ ls -la
total 24
dr-x------ 1 level09 level09  140 Mar  5  2016 .
d--x--x--x 1 root    users    340 Aug 30  2015 ..
-r-x------ 1 level09 level09  220 Apr  3  2012 .bash_logout
-r-x------ 1 level09 level09 3518 Aug 30  2015 .bashrc
-r-x------ 1 level09 level09  675 Apr  3  2012 .profile
-rwsr-sr-x 1 flag09  level09 7640 Mar  5  2016 level09
----r--r-- 1 flag09  level09   26 Mar  5  2016 token
level09@SnowCrash:~$ cat token 
f4kmm6p|=�p�n��DB�Du{��
level09@SnowCrash:~$ 




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




