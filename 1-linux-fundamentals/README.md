# Linux Fundamentals

## 1. What is Linux?

Linux is an **operating system**, just like Windows and macOS.

An operating system manages the computer's hardware, applications, files and resources.

### Examples of operating systems

- Windows
- Linux
- macOS
- Android (uses the Linux kernel)

### Where is Linux used?

- Websites and online applications
- Cloud platforms (AWS, Azure, Google Cloud)
- Android smartphones
- Supercomputers
- Banking and financial systems

### Linux vs Windows

| Feature | Linux | Windows |
|---|---|---|
| Interface | GUI and Terminal | Mainly GUI |
| Command interface | Bash / Shell | CMD / PowerShell |
| File separator | `/` | `\` |
| Hidden files | Names starting with `.` | Different hidden-file attribute |
| Navigation | Commands like `cd`, `ls` | File Explorer / `dir` |

### What is a Terminal?

A terminal is a text-based interface where we communicate with the operating system using commands.

Think of it like sending instructions to your computer.

**Basic flow:**

```text
User enters command
       |
       v
     Shell
       |
       v
 Operating System
       |
       v
     Output
```

### What is Bash?

Bash stands for **Bourne Again Shell**.

It is a commonly used shell in Linux that interprets commands and communicates with the operating system.

---

## 2. Understanding the Linux Terminal

A Linux terminal prompt may look like this:

```bash
alex@company-server:~$
```

Let's understand each part:

| Component | Meaning |
|---|---|
| `alex` | Username |
| `company-server` | Hostname (machine name) |
| `~` | Current location is the home directory |
| `$` | Regular user prompt |
| `#` | Generally indicates root user prompt |

### Opening the Terminal

On Ubuntu:

```text
Ctrl + Alt + T
```

### Your First Linux Commands

| Command | Purpose | Example |
|---|---|---|
| `echo` | Displays a message | `echo "Hello Linux"` |
| `date` | Displays current date and time | `date` |
| `uptime` | Shows how long the system has been running | `uptime` |

**Practice:**

```bash
echo "Hello, Linux!"
echo "My name is Rahul"
date
uptime
```

---

## 3. Getting Help in Linux

You don't need to memorise every Linux command.

Linux provides built-in ways to understand commands and their options.

There are three important ways to get help.

### A. man – Complete Manual

`man` stands for Manual.

Think of it as a complete instruction book for a command.

**Syntax:**

```bash
man command_name
```

**Example:**

```bash
man ls
```

Important navigation keys:

| Key | Purpose |
|---|---|
| Arrow keys | Scroll |
| Page Up / Page Down | Move between pages |
| `/keyword` | Search for a keyword |
| `n` | Find the next match |
| `q` | Quit the manual |

### B. --help – Quick Reference

Use `--help` when you need a quick explanation of a command and its available options.

**Examples:**

```bash
ls --help
cp --help
grep --help
```

### C. help – Bash Built-in Help

Use `help` for Bash built-in commands.

**Examples:**

```bash
help cd
help echo
help pwd
```

### Remember the Difference

| Command | When to use |
|---|---|
| `man ls` | Complete manual |
| `ls --help` | Quick command reference |
| `help cd` | Help for Bash built-ins |

**Easy way to remember:**

- `man` = Full instruction book
- `--help` = Quick reference card
- `help` = Bash built-in instructions

---

## 4. Linux Navigation Commands

Imagine Linux as a big office building.

- `pwd` tells you where you are.
- `ls` tells you what is available.
- `cd` helps you move to another location.
- `mkdir` helps you create a new room (folder).

These are some of the most frequently used Linux commands.

### A. pwd – Print Working Directory

Shows your current location.

```bash
pwd
```

Example output:

```text
/home/alex
```

**Remember:** pwd = Where am I?

### B. ls – List Directory Contents

Displays files and directories.

```bash
ls
```

Useful options:

| Command | Purpose |
|---|---|
| `ls` | List files and directories |
| `ls -l` | Detailed listing |
| `ls -a` | Show hidden files |
| `ls -la` | Detailed listing including hidden files |
| `ls -lh` | Human-readable file sizes |
| `ls /etc` | List contents of a specific directory |

**Understanding ls -l output:**

```text
-rw-r--r-- 1 alex alex 4096 Feb 28 10:30 notes.txt
```

| Part | Meaning |
|---|---|
| `-rw-r--r--` | Permissions |
| `1` | Number of hard links |
| First `alex` | Owner |
| Second `alex` | Group |
| `4096` | File size in bytes |
| `Feb 28 10:30` | Modification date and time |
| `notes.txt` | File name |

Basic permission symbols:

- `r` = Read
- `w` = Write
- `x` = Execute

### C. cd – Change Directory

Moves from one directory to another.

```bash
cd Documents
```

Important commands:

| Command | Purpose |
|---|---|
| `cd Documents` | Enter Documents directory |
| `cd /etc` | Go directly to /etc |
| `cd ..` | Move one level up |
| `cd ~` | Go to home directory |
| `cd -` | Return to previous directory |

**Remember:** cd = Where do I want to go?

### D. mkdir – Make Directory

Creates a new directory (folder).

Create one folder:

```bash
mkdir projects
```

Create nested directories:

```bash
mkdir -p projects/web/frontend
```

**What does `-p` do?**

It creates missing parent directories automatically.

Without `-p`, the command can fail if the parent directories do not exist.

### Navigation Practice

Execute these commands:

```bash
pwd
ls
mkdir DevOps
cd DevOps
mkdir Linux
mkdir Python
mkdir AWS
ls
pwd
```

Expected directory structure:

```text
DevOps/
    |
    |---- Linux/
    |
    |---- Python/
    |
    |---- AWS/
```

---

## 5. Linux Directory Structure

Linux follows a hierarchical directory structure.

Think of it as a building with different departments. Each directory has a specific purpose.

### A. Root Directory `/`

The root directory is the main starting point of the entire Linux filesystem.

Everything exists under `/`.

```bash
ls /
```

You may see directories such as:

```text
bin  boot  dev  etc  home  lib
opt  proc  root  srv  tmp  usr  var
```

### B. Important Linux Directories

| Directory | Purpose | Easy Example |
|---|---|---|
| `/` | Root of the filesystem | Main entrance |
| `/home` | Personal files of regular users | Personal office |
| `/etc` | Configuration files | Administration department |
| `/var` | Frequently changing data | Records department |
| `/var/log` | Log files | Office register |
| `/tmp` | Temporary files | Classroom whiteboard |
| `/usr` | Programs, tools and shared resources | Workshop |

### C. /home – Personal Files

Each regular user generally has a personal directory here.

Example:

```text
/home/alex
/home/priya
/home/nensi
```

Useful command:

```bash
ls /home
```

The shortcut `~` represents your home directory.

### D. /etc – Configuration Files

Stores system and application configuration files.

Examples:

```text
/etc/passwd
/etc/hostname
/etc/hosts
/etc/nginx/nginx.conf
/etc/ssh/sshd_config
```

Remember:

- `/etc/passwd` – User account information.
- `/etc/hostname` – Machine hostname.
- `/etc/hosts` – Local hostname-to-IP mappings.
- `/etc/nginx/nginx.conf` – Nginx configuration.
- `/etc/ssh/sshd_config` – SSH server configuration.

### E. /var – Variable Data

Stores data that changes during system operation.

The important directory is:

```text
/var/log
```

Examples:

```text
/var/log/syslog
/var/log/auth.log
/var/log/nginx/access.log
/var/log/nginx/error.log
```

Remember:

- `syslog` – General system messages.
- `auth.log` – Login and authentication events.
- `access.log` – Application request records.
- `error.log` – Error records.

### F. /tmp – Temporary Files

Used to store temporary files.

Temporary files may be automatically removed, including during a system restart.

**Remember:** Don't use `/tmp` for important permanent files.

### G. /usr – Programs and Tools

Contains many commonly used applications, commands and shared resources.

| Directory | Purpose |
|---|---|
| `/usr/bin` | Common commands and programs |
| `/usr/local/bin` | Locally installed programs |
| `/usr/lib` | Libraries used by programs |
| `/usr/share` | Shared resources and documentation |

### Directory Structure – Quick Revision

```text
                 /
                 |
     -------------------------
     |       |       |       |
    home    etc     var     usr
     |       |       |       |
    alex   config   log     bin
    priya  files    files   lib
    nensi                  share

                 |
                tmp
```

**Important commands:**

```bash
ls /
ls /home
ls /etc
ls /var/log
ls /tmp
ls /usr/bin
```

---

## 6. sudo and /etc/sudoers

### What is sudo?

`sudo` is commonly expanded as "superuser do".

It allows an authorised user to execute a command with elevated privileges.

Think of sudo as a special access card that allows you to enter a restricted room.

**Example:**

```bash
sudo apt update
```

Here:

- `sudo` requests elevated privileges.
- `apt update` refreshes the package information.

### Regular User vs Root User

| Regular User | Root User |
|---|---|
| Limited permissions | Broad administrative permissions |
| Usually sees `$` | Usually sees `#` |
| Cannot perform every administrative task | Can perform administrative tasks |

### What is /etc/sudoers?

```text
/etc/sudoers
```

It contains rules that determine who can use sudo and what privileges they have.

### What is visudo?

```bash
visudo
```

`visudo` is the recommended tool for editing sudoers rules.

It checks the syntax before saving, helping prevent configuration mistakes.

### Important points to remember

- Don't use sudo unnecessarily.
- Only authorised users can use sudo.
- Root has extensive administrative permissions.
- Use `visudo` to edit sudoers configuration safely.

---

## 7. Absolute and Relative Paths

A path is the address of a file or directory.

Imagine asking someone to visit your house.

You can give them your complete address or directions from where they are currently standing.

Linux uses these two methods.

### A. Absolute Path

An absolute path provides the complete address of a file or directory.

It starts from `/` (root).

**Example:**

```text
/home/alex/Documents/report.txt
```

Characteristics:

- Starts with `/`.
- Gives the complete location.
- Works regardless of your current directory.

Examples:

```bash
cd /etc
cat /etc/hostname
ls /var/log
```

### B. Relative Path

A relative path starts from your current location.

It does not start with `/`.

Example:

Suppose your current location is:

```text
/home/alex
```

And you want to access:

```text
/home/alex/Documents/report.txt
```

You can use:

```bash
cat Documents/report.txt
```

This works because Documents is inside your current directory.

### Absolute vs Relative Path

| Feature | Absolute Path | Relative Path |
|---|---|---|
| Starting point | Root `/` | Current directory |
| Depends on current location | No | Yes |
| Complete address | Yes | Not necessarily |
| Example | `/home/alex/Documents` | `Documents` |

### Special Path Symbols

| Symbol | Meaning |
|---|---|
| `/` | Root directory |
| `.` | Current directory |
| `..` | Parent directory |
| `~` | Home directory |
| `~priya` | Priya's home directory |
| `-` | Previous directory with `cd -` |

### Practical Examples

If your current directory is `/home/alex`:

```bash
cat Documents/report.txt
```

Relative path.

```bash
cat ~/Documents/report.txt
```

Using the home directory shortcut.

```bash
cat /home/alex/Documents/report.txt
```

Absolute path.

### Moving One Level Up

Suppose you are here:

```text
/var/log/nginx
```

You want to move to:

```text
/var/log
```

You can execute:

```bash
cd ..
```

Or:

```bash
cd /var/log
```

Both reach the same destination, but they use different approaches.

### Linux is Case-Sensitive

Linux treats uppercase and lowercase letters differently.

For example:

```text
/home/Alex
/home/alex
```

These can be two different directories.

Always check the spelling and capitalisation of directory names.

---

# 8. Complete Linux Command Cheat Sheet

| Command | Purpose |
|---|---|
| `echo` | Display a message |
| `date` | Show current date and time |
| `uptime` | Show system running time |
| `man` | Open command manual |
| `--help` | Show quick command reference |
| `help` | Show Bash built-in help |
| `pwd` | Show current directory |
| `ls` | List files and directories |
| `ls -l` | Detailed listing |
| `ls -a` | Show hidden files |
| `ls -la` | Detailed listing with hidden files |
| `ls -lh` | Human-readable file sizes |
| `cd` | Change directory |
| `cd ..` | Move one level up |
| `cd ~` | Go to home directory |
| `cd -` | Return to previous directory |
| `mkdir` | Create a directory |
| `mkdir -p` | Create nested directories |
| `sudo` | Execute an authorised command with elevated privileges |
| `visudo` | Safely edit sudoers configuration |

---

# 9. Practice Exercises

Try these commands yourself. Don't just read them!

### Exercise 1: Basic Commands

1. Display your name using echo.
2. Display the current date.
3. Check how long your system has been running.

```bash
echo "My name is Rahul"
date
uptime
```

### Exercise 2: Navigation

1. Find your current location.
2. List the available files.
3. Show hidden files.
4. Navigate to your home directory.
5. Move one directory up.

```bash
pwd
ls
ls -a
cd ~
cd ..
```

### Exercise 3: Directory Creation

Create this directory structure:

```text
MyLearning/
    Linux/
    Python/
    AWS/
```

Commands:

```bash
mkdir -p MyLearning/Linux
mkdir -p MyLearning/Python
mkdir -p MyLearning/AWS
```

Check your result:

```bash
ls MyLearning
```

### Exercise 4: Explore Linux Directories

Execute:

```bash
ls /
ls /home
ls /etc
ls /var/log
ls /tmp
ls /usr/bin
```

Identify what each directory is used for.

### Exercise 5: Getting Help

Try:

```bash
man ls
ls --help
help cd
```

Remember how to exit the manual using `q`.

---

# 10. Quick Knowledge Check

Try answering these questions without looking at the notes.

1. What is Linux?
2. What is an operating system?
3. What is a terminal?
4. What does Bash stand for?
5. What is the difference between `man` and `--help`?
6. Which command displays the current working directory?
7. Which command lists files and directories?
8. What does `ls -a` do?
9. What is the purpose of `cd ..`?
10. What is the purpose of `mkdir`?
11. What does the root directory `/` represent?
12. Where are personal user files generally stored?
13. Where are configuration files generally stored?
14. Where are log files stored?
15. What is the purpose of `/tmp`?
16. What does sudo do?
17. What is `/etc/sudoers`?
18. Why do we use `visudo`?
19. What is an absolute path?
20. What is a relative path?
21. What does `~` represent?
22. What is the difference between `.` and `..`?
23. Is Linux case-sensitive?

## Answer Key

1. An operating system.
2. Software that manages computer hardware and applications.
3. A text-based interface for entering commands.
4. Bourne Again Shell.
5. `man` gives complete documentation; `--help` gives a quick reference.
6. `pwd`.
7. `ls`.
8. Displays hidden files.
9. Moves one directory level up.
10. Creates a directory.
11. The starting point of the Linux filesystem.
12. `/home`.
13. `/etc`.
14. `/var/log`.
15. Stores temporary files.
16. Allows authorised users to execute commands with elevated privileges.
17. Contains sudo access rules.
18. It checks sudoers syntax before saving.
19. A complete path starting from `/`.
20. A path based on the current directory.
21. Home directory.
22. `.` means current directory; `..` means parent directory.
23. Yes.

---
**A small reminder:**

You don't need to memorise every command in one day.

Practise a few commands daily, understand what they do, and gradually build your confidence.

### Happy Learning! 
