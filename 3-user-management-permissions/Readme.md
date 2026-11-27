# User Management & Permissions

By the end of this session you will be able to:

- ✅ Find out who you are on a Linux system (`whoami`, `id`)
- ✅ Run administrator commands safely with `sudo`
- ✅ Switch between user accounts with `su`
- ✅ Read the permission label on any file using `ls -l`
- ✅ Change permissions with `chmod` (both letter style and number style)
- ✅ Change who owns a file with `chown` and `chgrp`
- ✅ Understand SUID and the sticky bit
- ✅ Add fine-grained access rules with ACLs (`getfacl`, `setfacl`)

---

## The Hospital Analogy

Think of a Linux server as a hospital. This mental picture helps explain everything in this session:

```text
Linux server    =   St. Penguin's General Hospital
user account    =   staff ID badge
group           =   department / team
file            =   patient record
directory       =   department room
permissions     =   access rules (who can enter, read, change)
```

At a hospital, a doctor can read and edit patient files. A nurse can read them. A visitor can only see the waiting room. Linux permissions work exactly the same way.

---

## What We Are Building

```bash
mkdir -p class3_practice
cd class3_practice
touch patient_record.txt
touch deploy.sh
```

We will use these files throughout the session to practise permissions.

---

## 1. Who Am I?

Before you can understand permissions, you need to know who the system thinks you are.

---

### 1.1 `whoami` — Print the Current Username

Prints the name of the user currently logged in.

```bash
whoami
```

**Example output:**
```text
student
```

> **When is this useful?** After switching between users, or when you log into a remote server and are not sure which account is active.

---

### 1.2 `id` — Show Your Full Identity Profile

Shows your user ID number, your main group ID, and every group you belong to. Like the full details on a hospital ID badge — name, employee number, and which departments you have access to.

```bash
id
```

**Example output:**
```text
uid=1001(student) gid=1001(student) groups=1001(student),27(sudo)
```

**What each part means:**

| Part | Meaning |
|:--|:--|
| `uid=1001(student)` | Your user ID number is 1001, and your username is `student` |
| `gid=1001(student)` | Your main group ID is 1001 |
| `groups=...` | Every group (team) you belong to |
| `27(sudo)` | You are in the `sudo` group — which means you can run administrator commands |

> `uid` is your employee number. Linux uses numbers internally — the name in brackets is just the human-readable label. When you get a "Permission denied" error, run `id` first. It tells you exactly who the system thinks you are.

**Exercise:**

```bash
whoami
id
```

Compare with a classmate — are your uid numbers the same or different? They will likely differ on a multi-user system.

---

## 2. Elevated Access — `sudo`, `su`, `exit`, `passwd`

Some actions need administrator access — like installing software or creating new users. Linux gives you two ways to do this.

```text
sudo  =  borrow the master key for ONE command  (safer)
su    =  become a different user for the whole session
root  =  the Linux administrator account (superuser)
```

---

### 2.1 `sudo` — Run One Command as Administrator

Runs a single command with administrator (root) privileges. Think of it as: *"Use the master key just for this one locked door."*

It asks for **your own** password, not the root password.

```bash
sudo apt update
```

**What you see:**
```text
[sudo] password for student:
```

Type your password and press Enter.

**Why `sudo` is safer than always working as root:**

| `sudo` (recommended) | Logged in as `root` (risky) |
|:--|:--|
| One command at a time gets elevated | Everything runs as root — one typo can delete critical files |
| Your username is logged with the command | Root actions are harder to trace |
| Privilege expires after ~15 minutes | Stays elevated the whole session |

---

### 2.2 `su` — Switch to Another User Account

Opens a new shell as a different user. The `-` loads that user's full environment, home folder, and profile.

```bash
su - sarah
```

**What you see:**
```text
Password:
```

After entering the password, your prompt changes to:
```text
sarah@hostname:~$
```

**Check that you are now Sarah:**
```bash
whoami
```

**Output:**
```text
sarah
```

**Go back to your original account:**
```bash
exit
```

**Output:**
```text
logout
student@hostname:~$
```

> Always run `whoami` after switching users to confirm you are where you think you are.

---

### 2.3 `passwd` — Change a Password

Changes the password for the current user (or another user, if you use `sudo`).

**Change your own password:**
```bash
passwd
```

**What you see:**
```text
Changing password for student.
Current password:
New password:
Retype new password:
passwd: password updated successfully
```

**Administrator resets another user's password:**
```bash
sudo passwd sarah
```

> The administrator does not need the old password — they can set a new one directly.

---

## 3. The Permission Model & Reading `ls -l`

Every file and folder on Linux has a label — just like a patient file in a hospital has a label showing which doctor owns it, which team can read it, and what visitors can do.

---

### 3.1 The Three Ownership Classes

```text
u = user / owner   →  the person who created the file (primary doctor)
g = group          →  the team that the file belongs to (department)
o = others         →  everyone else (visitors)
```

---

### 3.2 The Three Permissions

```text
r = read     →  open and view the file
w = write    →  change or delete the file
x = execute  →  run the file as a program (or enter the folder)
```

**Permission on a file vs a folder:**

| Permission | On a File | On a Folder (Directory) |
|:--|:--|:--|
| `r` | Read the file contents | List the files inside (`ls`) |
| `w` | Edit or delete the file | Create, rename, or delete items inside |
| `x` | Run the file as a program | Enter the folder (`cd`) |

> Think of `x` on a folder as the ability to walk through a door. Without it, the room is locked — even if you can see the door exists.

---

### 3.3 `ls -l` — Read the Full Permission Label

Shows a detailed list of every file — including permissions, owner, group, size, and date.

```bash
ls -l
```

**Example output:**
```text
-rw-r--r-- 1 student student    0 Nov 27 21:00 patient_record.txt
-rw-r--r-- 1 student student    0 Nov 27 21:00 deploy.sh
```

**How to read one permission string — left to right:**

```text
- rw- r-- r--
|  |   |   |
|  |   |   +-- others:  r only  (everyone else can only read)
|  |   +------ group:   r only  (the team can only read)
|  +---------- owner:   rw-     (you can read and write)
+------------- type:    - = file,  d = directory,  l = link
```

**A more complex example:**

```bash
ls -l /usr/bin/passwd
```

**Example output:**
```text
-rwsr-xr-x 1 root root 68208 Jan 10 10:00 /usr/bin/passwd
```

```text
- rws r-x r-x
|  |   |   |
|  |   |   +-- others: r-x  (can read and execute)
|  |   +------ group:  r-x  (can read and execute)
|  +---------- owner:  rws  (read, write, and special "s" = SUID — covered below)
+------------- type:   - = regular file
```

**Exercise — read this together:**

```bash
ls -l class3_practice/
```

Questions to ask yourself:
- Who is the owner?
- What can the owner do?
- Can others write to this file?

---

## 4. Changing Permissions with `chmod`

Changes the permission label of a file or folder.

`chmod` = **ch**ange **mod**e.

You can use two styles: **symbolic** (letters) or **numeric** (numbers).

---

### 4.1 Symbolic Mode — Using Letters

**Pattern:** `chmod  who + or - permission  file`

```text
who:        u = owner,  g = group,  o = others,  a = all three
action:     + = add,    - = remove,  = = set exactly
permission: r, w, x
```

**Add execute permission for the owner:**
```bash
chmod u+x deploy.sh
ls -l deploy.sh
```

**Example output:**
```text
-rwxr--r-- 1 student student 0 Nov 27 21:00 deploy.sh
```

The `x` now appears in the owner's section (`rwx`).

**Remove write permission from the group:**
```bash
chmod g-w patient_record.txt
ls -l patient_record.txt
```

**Remove write from others:**
```bash
chmod o-w patient_record.txt
ls -l patient_record.txt
```

---

### 4.2 Numeric (Octal) Mode — Using Numbers

Each permission has a number. Add them up to get the total for each class.

```text
r = 4     (read)
w = 2     (write)
x = 1     (execute)

rwx = 4+2+1 = 7
rw- = 4+2+0 = 6
r-x = 4+0+1 = 5
r-- = 4+0+0 = 4
--- = 0+0+0 = 0
```

**Three digits: owner | group | others**

```bash
chmod 754 deploy.sh
ls -l deploy.sh
```

**Example output:**
```text
-rwxr-xr-- 1 student student 0 Nov 27 21:00 deploy.sh
```

Translated:
- `7` = `rwx` → owner can read, write, execute
- `5` = `r-x` → group can read and execute
- `4` = `r--` → others can only read

**Common permission numbers:**

| Number | Permission | When to use |
|:--|:--|:--|
| `777` | `rwxrwxrwx` | Everyone can do everything — ⚠️ unsafe, avoid |
| `755` | `rwxr-xr-x` | Standard for scripts and programs |
| `644` | `rw-r--r--` | Standard for text files and documents |
| `600` | `rw-------` | Private files — only you can read/write |
| `400` | `r--------` | Read-only even for you — good for backups |

**Exercise:**

```bash
touch myfile.txt
chmod 644 myfile.txt
ls -l myfile.txt
```

**Expected output:**
```text
-rw-r--r-- 1 student student 0 Nov 27 21:05 myfile.txt
```

> What does `644` mean? Owner can read and write (`6` = `rw-`). Group and others can only read (`4` = `r--`).

---

## 5. Ownership: `chown` & `chgrp`

Permissions say what each class can do. Ownership says which user is the owner and which group is the team.

---

### 5.1 `chown` — Change Owner (or Owner and Group)

Changes who owns a file. Usually needs `sudo` because only an administrator can reassign ownership.

**Show the current owner first:**
```bash
ls -l patient_record.txt
```

**Example output:**
```text
-rw-r--r-- 1 student student 0 Nov 27 21:00 patient_record.txt
```

**Change just the owner:**
```bash
sudo chown bob patient_record.txt
ls -l patient_record.txt
```

**Example output:**
```text
-rw-r--r-- 1 bob student 0 Nov 27 21:00 patient_record.txt
```

**Change both the owner AND the group at once:**
```bash
sudo chown bob:developers patient_record.txt
ls -l patient_record.txt
```

**Example output:**
```text
-rw-r--r-- 1 bob developers 0 Nov 27 21:00 patient_record.txt
```

**Change ownership of a whole folder and everything inside it:**
```bash
sudo chown -R bob:developers class3_practice/
```

> ⚠️ **Be careful with `-R`** — it changes everything inside the folder. Always double-check the path before pressing Enter.

| `chown` Pattern | What it changes |
|:--|:--|
| `chown bob file` | Owner only → `bob` |
| `chown bob:developers file` | Owner → `bob`, Group → `developers` |
| `chown -R bob:developers folder/` | Owner and group for folder and everything inside |

---

### 5.2 `chgrp` — Change Only the Group

Changes the group label but leaves the owner unchanged.

```bash
sudo chgrp developers patient_record.txt
ls -l patient_record.txt
```

**Example output:**
```text
-rw-r--r-- 1 bob developers 0 Nov 27 21:00 patient_record.txt
```

> If only the team should change and the owner should stay the same, use `chgrp`. If you need to change both, use `chown user:group file`.

**Exercise:**

```bash
touch class3_practice/notes.txt
ls -l class3_practice/notes.txt
```

Look at the output — which column is the owner and which is the group?

---

## 6. Special Permissions — SUID & Sticky Bit

Normal permissions don't cover every situation. Linux has special permission bits for specific needs.

---

### 6.1 SUID — Set User ID

When you run a file with SUID set, it runs with the **file owner's** permissions — not yours.

Think of it as a vending machine that temporarily uses a master key to do its one job. You don't get the master key — the machine just uses it briefly to serve you.

**Real example — the `passwd` command:**

```bash
ls -l /usr/bin/passwd
```

**Example output:**
```text
-rwsr-xr-x 1 root root 68208 Jan 10 10:00 /usr/bin/passwd
```

> See the `s` in the owner's execute position (`rws`)? That `s` means SUID is set.
> When you run `passwd`, the program temporarily gets root's permissions — just long enough to update the password file. Without SUID, a regular user could never change their own password.

**Set SUID on a file (symbolic mode):**
```bash
touch myprogram
chmod u+s myprogram
ls -l myprogram
```

**Example output:**
```text
-rwSr--r-- 1 student student 0 Nov 27 21:10 myprogram
```

> `S` (capital) = SUID is set, but execute is NOT.
> `s` (lowercase) = SUID is set AND execute is set.

**Set SUID using numeric mode (leading `4`):**
```bash
chmod 4755 myprogram
ls -l myprogram
```

**Example output:**
```text
-rwsr-xr-x 1 student student 0 Nov 27 21:10 myprogram
```

> ⚠️ **Safety:** Never set SUID on a script or on programs carelessly. Only set it on programs you fully understand — in the wrong hands, SUID can be used to gain unauthorized root access.

---

### 6.2 Sticky Bit — Protect a Shared Folder

In a shared folder, users can normally delete anyone's files (if they have write access to the folder). The sticky bit changes that rule: **you can only delete files you own**.

Think of it as: "In the shared supply room, everyone can add items. But you can only take back what you personally put there."

**The best real example — `/tmp`:**
```bash
ls -ld /tmp
```

**Example output:**
```text
drwxrwxrwt 15 root root 4096 Nov 27 20:00 /tmp
```

> See the `t` at the end of the permissions (`rwt`)? That `t` is the sticky bit.
> `/tmp` is a folder everyone can write to — but you can only delete your own files there.

**Create a shared folder and set the sticky bit:**
```bash
mkdir shared_room
chmod 777 shared_room
chmod +t shared_room
ls -ld shared_room
```

**Output:**
```text
drwxrwxrwt 2 student student 4096 Nov 27 21:12 shared_room
```

The `t` appeared at the end — the sticky bit is now set.

**Using numeric mode (leading `1`):**
```bash
chmod 1777 shared_room
```

> `1` = sticky bit, `7` = rwx for each class.
> `1777` → `drwxrwxrwt`

---

## 7. Access Control Lists — `getfacl` & `setfacl`

Standard permissions have exactly three buckets: owner, group, and others. But what if a specialist from outside the hospital needs to see one specific patient file? They are not the owner, not in the group, and we don't want to give every visitor access.

ACLs solve this — they let you add a named permission for one specific person.

```text
Normal permissions:    owner  |  group  |  others
ACL:                   owner  |  group  |  others  +  NAMED users/groups
```

---

### 7.1 `getfacl` — View Current Permissions (Including ACLs)

Shows all permissions on a file — standard permissions AND any extra ACL entries.

```bash
touch patient_007.txt
getfacl patient_007.txt
```

**Example output:**
```text
# file: patient_007.txt
# owner: student
# group: student
user::rw-
group::r--
other::r--
```

> The `::` with nothing between the colons means the **standard** owner, group, or others entry. No extra ACL entries yet.

---

### 7.2 `setfacl` — Add a Specific User's Permission

Adds (or changes) a named permission for one specific user or group.

**Give the user `bob` read permission on `patient_007.txt`:**
```bash
setfacl -m u:bob:r-- patient_007.txt
getfacl patient_007.txt
```

**Example output:**
```text
# file: patient_007.txt
# owner: student
# group: student
user::rw-
user:bob:r--
group::r--
mask::r--
other::r--
```

> `user:bob:r--` means: specifically the user `bob` gets read-only access.
> The owner and others access did not change — only `bob` got a new entry.

**What `-m` means:** modify — add or update an ACL entry.

**Pattern:**
```text
setfacl -m u:username:permissions  file
setfacl -m g:groupname:permissions file
```

---

### 7.3 Remove All ACLs from a File

Removes every extra ACL entry and returns to standard permissions.

```bash
setfacl -b patient_007.txt
getfacl patient_007.txt
```

**Example output:**
```text
# file: patient_007.txt
# owner: student
# group: student
user::rw-
group::r--
other::r--
```

> `bob`'s entry is gone. `-b` = remove all ACL entries (back to basics).

---

### 7.4 How Linux Decides Who Gets Access (Priority Order)

```text
Step 1 → Is the user the OWNER?           → Apply owner permissions
Step 2 → Is there a named user ACL entry? → Apply that (takes priority over groups)
Step 3 → Is the user in a matching group? → Apply group or named group ACL
Step 4 → None of the above?               → Apply "others" permissions
```

Think of it like a hospital receptionist checking your badge. First they check your name. Then they check if you're on the VIP list. Then they check your department. If none of those match, you get visitor access.

**`getfacl` vs `setfacl` — side by side:**

| Command | What it does |
|:--|:--|
| `getfacl file` | Show all permissions (standard + ACL) |
| `setfacl -m u:bob:r-- file` | Give `bob` read access |
| `setfacl -m g:nurses:rw- file` | Give the `nurses` group read+write |
| `setfacl -b file` | Remove all extra ACL entries |

**Exercise:**

```bash
touch class3_practice/lab_report.txt
getfacl class3_practice/lab_report.txt
```

Look at the output. Can you identify the owner line, the group line, and the others line?

---

## Troubleshooting Permissions — Step by Step

When you get a "Permission denied" error, always follow this order:

```text
1. Run id              → Who am I? What groups do I belong to?
2. Run ls -l           → Who owns the file? What are the permissions?
3. Run getfacl         → Are there any extra ACL rules?
4. Use chmod / chown   → Make the smallest safe change needed
5. Verify with ls -l   → Confirm the change worked
```

---

## All Commands — Quick Reference

| Command | What It Does | Simple Example |
|:--|:--|:--|
| `whoami` | Print the current username | `whoami` |
| `id` | Show uid, gid, and all groups | `id` |
| `sudo command` | Run one command as administrator | `sudo apt update` |
| `su - username` | Switch to another user account | `su - sarah` |
| `exit` | Return to the previous user | `exit` |
| `passwd` | Change your own password | `passwd` |
| `sudo passwd username` | Admin resets another user's password | `sudo passwd sarah` |
| `ls -l` | Show detailed permissions for files | `ls -l` |
| `ls -ld folder/` | Show permissions for a folder itself | `ls -ld /tmp` |
| `chmod u+x file` | Add execute for the owner | `chmod u+x deploy.sh` |
| `chmod g-w file` | Remove write from the group | `chmod g-w report.txt` |
| `chmod 755 file` | Set exact permissions (numeric) | `chmod 755 script.sh` |
| `chmod 644 file` | Standard text file permissions | `chmod 644 notes.txt` |
| `chmod +t folder/` | Set sticky bit on a folder | `chmod +t shared_room/` |
| `chmod u+s file` | Set SUID on a file | `chmod u+s myprogram` |
| `chown bob file` | Change the file owner to bob | `chown bob record.txt` |
| `chown bob:dev file` | Change owner and group | `chown bob:dev record.txt` |
| `chown -R bob:dev folder/` | Change owner+group recursively | `chown -R bob:dev class3/` |
| `chgrp nurses file` | Change only the group | `chgrp nurses record.txt` |
| `getfacl file` | View all permissions including ACLs | `getfacl patient_007.txt` |
| `setfacl -m u:bob:r-- file` | Give `bob` read-only access via ACL | `setfacl -m u:bob:r-- file.txt` |
| `setfacl -b file` | Remove all extra ACL entries | `setfacl -b patient_007.txt` |

---

## Practice Exercises

Try these yourself. Don't just read them!

### Exercise 1: Who Am I?

```bash
whoami
id
```

Note your uid and which groups you belong to.

---

### Exercise 2: Create Practice Files

```bash
mkdir -p class3_practice
cd class3_practice
touch patient_record.txt deploy.sh
ls -l
```

Read the permission string on each file. Identify the type, owner permissions, group permissions, and others permissions.

---

### Exercise 3: Change Permissions (Symbolic)

```bash
# Add execute for the owner
chmod u+x deploy.sh
ls -l deploy.sh

# Remove write from others
chmod o-w patient_record.txt
ls -l patient_record.txt

# Give read to everyone
chmod a+r patient_record.txt
ls -l patient_record.txt
```

---

### Exercise 4: Change Permissions (Numeric)

```bash
touch myfile.txt

# Standard file permissions
chmod 644 myfile.txt
ls -l myfile.txt

# Standard script permissions
chmod 755 deploy.sh
ls -l deploy.sh

# Private file — only you
chmod 600 myfile.txt
ls -l myfile.txt
```

For each result, decode the permission string yourself before looking at the number.

---

### Exercise 5: Sticky Bit on a Shared Folder

```bash
mkdir shared_room
chmod 777 shared_room
ls -ld shared_room

chmod +t shared_room
ls -ld shared_room
```

What changed in the output? Where does the `t` appear?

---

### Exercise 6: SUID

```bash
touch myprogram
chmod u+s myprogram
ls -l myprogram

chmod 4755 myprogram
ls -l myprogram
```

Look at the difference between `S` (capital) and `s` (lowercase) in the output.

Also check a real SUID file:
```bash
ls -l /usr/bin/passwd
```

---

### Exercise 7: ACLs

```bash
touch patient_007.txt

# View current permissions
getfacl patient_007.txt

# Add a named user entry (bob may not exist — that's okay, run the command and study the output)
setfacl -m u:bob:r-- patient_007.txt
getfacl patient_007.txt

# Remove all ACL entries
setfacl -b patient_007.txt
getfacl patient_007.txt
```

---

### Exercise 8: Full Walkthrough

Build from scratch and practise every concept:

```bash
mkdir -p class3_practice
cd class3_practice

# Create files
touch report.txt script.sh notes.txt

# Set appropriate permissions
chmod 644 report.txt
chmod 755 script.sh
chmod 600 notes.txt

# Verify
ls -l

# Create a shared folder with sticky bit
mkdir shared
chmod 1777 shared
ls -ld shared

# View all permissions with getfacl
getfacl report.txt
```

---

## Knowledge Check

Try answering these without looking at the notes.

1. What command shows your current username?
2. What command shows your uid, gid, and all groups?
3. What is the difference between `sudo` and `su`?
4. What does the `-` in `su - sarah` do?
5. What command do you run after `su` to go back to your original user?
6. What do `u`, `g`, and `o` stand for in Linux permissions?
7. What do `r`, `w`, and `x` mean on a file?
8. What does `x` mean on a **folder**?
9. In `ls -l`, what does the first character (`-`, `d`, or `l`) mean?
10. What does `chmod u+x deploy.sh` do?
11. What is the numeric value for: `r` = ?, `w` = ?, `x` = ?
12. What does `chmod 755` set for owner, group, and others?
13. What does `chmod 644` set for owner, group, and others?
14. Which `chmod` number gives you a private file (only you can read and write)?
15. What is the difference between `chown` and `chgrp`?
16. How do you change both the owner and the group in one command?
17. What does the `s` in `-rwsr-xr-x` mean?
18. What is the difference between `s` (lowercase) and `S` (uppercase) in a permission string?
19. What does the sticky bit do? Where do you see it used on a real Linux system?
20. What is the difference between standard permissions and ACLs?
21. Which command views ACLs on a file?
22. How do you give one specific user read access to a file using an ACL?
23. How do you remove all extra ACL entries from a file?
24. What is the access priority order Linux uses when checking permissions?

## Answer Key

1. `whoami`
2. `id`
3. `sudo` runs one command as administrator; `su` switches your entire session to a different user account.
4. It loads that user's full environment — their home folder, profile, and settings.
5. `exit`
6. `u` = user/owner, `g` = group, `o` = others.
7. `r` = read the file, `w` = edit or delete it, `x` = run it as a program.
8. `x` on a folder means permission to enter it with `cd`. Without it, you cannot go inside even if you can see the folder.
9. `-` = regular file, `d` = directory (folder), `l` = symbolic link (shortcut).
10. It adds execute (`x`) permission for the owner (`u`) of `deploy.sh`.
11. `r` = 4, `w` = 2, `x` = 1.
12. `7` = `rwx` (owner), `5` = `r-x` (group), `5` = `r-x` (others).
13. `6` = `rw-` (owner), `4` = `r--` (group), `4` = `r--` (others).
14. `600` (`rw-------`).
15. `chown` can change the owner (and optionally the group); `chgrp` changes only the group.
16. `sudo chown user:group file`
17. The `s` in the owner's execute position means SUID is set — the program runs with the file owner's permissions (usually root) instead of the caller's.
18. `s` (lowercase) = SUID is set AND execute is also set. `S` (uppercase) = SUID is set but execute is NOT set.
19. The sticky bit protects files in a shared folder — you can only delete files you own. Real example: `/tmp`.
20. Standard permissions have three fixed buckets (owner, group, others). ACLs let you add named permissions for specific users or groups beyond those three.
21. `getfacl file`
22. `setfacl -m u:username:r-- file`
23. `setfacl -b file`
24. Owner → named user ACL → group (or named group ACL) → others.

---

**A small reminder:**

You don't need to memorise every command in one day.

Focus on `ls -l` to read permissions, `chmod` to change them, and `id` to know who you are. That is enough to handle 90% of real-world permission issues.

Practise a few commands daily, understand what they do, and gradually build your confidence.

### Happy Learning!
