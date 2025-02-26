# level01

```bash
cat /etc/passwd
flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash
```

42hDRfypTqqnw

### decode with john
```bash
/usr/sbin/john pwd --show
?:abcdefg
```

### get the flag
```bash
level01@SnowCrash:~$ su flag01
Password: 
Don't forget to launch getflag !
flag01@SnowCrash:~$ getflag
Check flag.Here is your token : f2av5il02puano7naaf6adaaf
```