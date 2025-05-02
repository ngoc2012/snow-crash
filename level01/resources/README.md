# level01

The `/etc/passwd` file on Unix-like systems stores essential user account information. Each line represents one user and contains **7 fields**, separated by colons (`:`), in the following order:

```
username:password:UID:GID:GECOS:home_directory:shell
```

### Breakdown of Fields:

1. **username** – The user's login name (e.g., `alice`).
2. **password** – Usually an `x` if shadow passwords are used (real hash is in `/etc/shadow`).
3. **UID (User ID)** – Numeric ID for the user (e.g., `1000` for a regular user).
4. **GID (Group ID)** – Numeric ID of the user's primary group (defined in `/etc/group`).
5. **GECOS** – Comment field, usually contains the user's full name or other info.
6. **home_directory** – Path to the user's home directory (e.g., `/home/alice`).
7. **shell** – Default shell for the user (e.g., `/bin/bash` or `/usr/sbin/nologin`).

### Example:
```
alice:x:1000:1000:Alice Smith,,,:/home/alice:/bin/bash
```

The actual password hash is stored in /etc/shadow
/etc/shadow is only readable by root and contains encrypted password hashes and additional password aging information.


```bash
cat /etc/passwd
flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash
```

42hDRfypTqqnw

sudo apt install john

### decode with john
```bash
vi pw.txt
minh-ngu@ngoc:~$ john pw.txt 
Loaded 1 password hash (descrypt, traditional crypt(3) [DES 128/128 SSE2-16])
Press 'q' or Ctrl-C to abort, almost any other key for status
abcdefg          (flag01)
1g 0:00:00:00 100% 2/3 100.0g/s 139300p/s 139300c/s 139300C/s raquel..bigman
Use the "--show" option to display all of the cracked passwords reliably
Session completed
minh-ngu@ngoc:~$ john pw.txt --show
flag01:abcdefg

1 password hash cracked, 0 left
```

### get the flag
```bash
level01@SnowCrash:~$ su flag01
Password: 
Don't forget to launch getflag !
flag01@SnowCrash:~$ getflag
Check flag.Here is your token : f2av5il02puano7naaf6adaaf
```