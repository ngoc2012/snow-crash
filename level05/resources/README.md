# level05


when connect vm with ssh:

level05@192.168.182.137's password: 
You have new mail.


level05@SnowCrash:~$ ls
level05@SnowCrash:~$ ls /var/mail/
level05
level05@SnowCrash:~$ cat /var/mail/level05 
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05

level05@SnowCrash:~$ cat /usr/sbin/openarenaserver 
#!/bin/sh

for i in /opt/openarenaserver/* ; do
	(ulimit -t 5; bash -x "$i")
	rm -f "$i"
done
level05@SnowCrash:~$ 

evel05@SnowCrash:/$ cd /opt/openarenaserver/
level05@SnowCrash:/opt/openarenaserver$ vi flag.sh
level05@SnowCrash:/opt/openarenaserver$ cat flag.sh
#!/bin/sh
/bin/getflag > /tmp/flag
level05@SnowCrash:/opt/openarenaserver$ ls /tmp/
ls: cannot open directory /tmp/: Permission denied
level05@SnowCrash:/opt/openarenaserver$ cd /tmp/
level05@SnowCrash:/tmp$ ls
ls: cannot open directory .: Permission denied
level05@SnowCrash:/tmp$ cat flag
Check flag.Here is your token : viuaaale9huek52boumoomioc








The line:

```bash
(ulimit -t 5; bash -x "$i")
```

is a **subshell** that performs two actions for each file in the `/opt/openarenaserver/` directory:

---

### 🔍 Breakdown:

1. **`ulimit -t 5`**
   This sets a **CPU time limit** of 5 seconds for any process launched in this shell.

   * `-t` sets the **maximum CPU time (in seconds)** that a process can use.
   * If a process exceeds this limit, it is **killed** by the system (with `SIGXCPU`).

2. **`bash -x "$i"`**
   This runs the file `"$i"` with `bash` in **debug mode** (`-x` flag).

   * Debug mode prints each command and its arguments to standard error as they are executed.
   * Useful for **troubleshooting or inspecting** what the script does.

3. **Subshell `( ... )`**
   Wrapping these two commands in parentheses runs them in a **subshell**.

   * This confines both the `ulimit` and the `bash` execution to a separate shell environment.
   * Any limits or changes inside it don’t affect the main shell or other iterations.

---

### 🧠 Summary:

This line **safely executes each script in `/opt/openarenaserver/` with a 5-second CPU time cap**, printing each executed command for debugging, and **isolates each run** from the rest using a subshell.

This is likely done to:

* Run **untrusted or temporary scripts** safely.
* **Prevent infinite loops** or long-running executions.
* **Debug** the scripts being executed.


Great question — especially in a restricted environment like **SnowCrash**, where many directories are locked down.

---

### ✅ Goal:

You want to find a directory where:

* You **have write permissions**
* You can **read the result later**
* Ideally, it's **not deleted automatically** (unlike `/tmp` on reboot)

---

### 🔍 Common places you **can** save files as `level05`:

#### 1. **Your home directory — if writable**

Try:

```bash
touch ~/testfile
```

If it works, you can use `~/output.txt` or similar.

But on SnowCrash, the home dir (`/home/level05`) is often **read-only**, so this may fail.

---

#### 2. **`/tmp` (default world-writable)**

You're already using this.

Even if you can’t `ls /tmp`, you can still:

```bash
mkdir /tmp/mystuff
echo test > /tmp/mystuff/output
```

Just remember: it's **cleared on reboot**, and world-readable unless protected.

---

#### 3. **`/dev/shm` (shared memory tmpfs)**

Try:

```bash
mkdir /dev/shm/lvl05
echo "getflag" > /dev/shm/lvl05/script.sh
```

✅ Pros:

* Usually writable by all users
* Resides in memory (faster, not persistent)
* Not as closely watched as `/tmp`

⚠️ Cons:

* Same volatility as `/tmp` (cleared on reboot)

---

#### 4. **Check for world-writable directories**

Run:

```bash
find / -type d -writable 2>/dev/null
```

Look for unusual locations like:

* `/var/tmp` (persistent across reboots, unlike `/tmp`)
* `/dev/shm`
* Any overlooked dev/test folders

⚠️ Be cautious — some folders may be writable but **monitored or trapped**.

---

### 🧠 Pro Tip:

Use `ls -ld <dir>` to check permissions. You're looking for:

```bash
drwxrwxrwt
```

or:

```bash
drwxrwx--x
```

Where you at least have write (`w`) and execute (`x`) rights.

---

### ✅ Summary:

You can safely save files in:

| Path         | Writable? | Persistent? | Notes                             |
| ------------ | --------- | ----------- | --------------------------------- |
| `/tmp`       | ✅         | ❌           | Default, safe for short-term use  |
| `/dev/shm`   | ✅         | ❌           | Fast, RAM-based, less monitored   |
| `/var/tmp`   | ✅         | ✅           | Persistent temp storage (if open) |
| Your `$HOME` | ❓         | ✅           | Depends on challenge permissions  |

---

Let me know if you want help testing any specific path or automating the write check!
