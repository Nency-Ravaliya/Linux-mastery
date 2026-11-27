# 👋 Session 3 — User Management & Permissions

By the end of this session you will be able to:

- ✅ Find out who you are on a Linux system (`whoami`, `id`)
- ✅ Run commands as an administrator safely using `sudo`
- ✅ Read and understand the permission label on any file
- ✅ Change who can read, write, or execute a file using `chmod`
- ✅ Change the owner and group of a file using `chown` and `chgrp`
- ✅ Understand SUID and the sticky bit (special permission rules)
- ✅ Give one specific person access to a file without changing everyone else's access (ACLs)

## 📚 Topics We Will Cover Tonight

### 1. Who Am I?
> *Checking your identity before working with permissions*

| Command | What it does |
|:--|:--|
| `whoami` | Print the current username |
| `id` | Show your user ID, group ID, and all groups you belong to |

---

### 2. Elevated Access
> *Running commands that need administrator power — safely*

| Command | What it does |
|:--|:--|
| `sudo` | Run one command as administrator (safest approach) |
| `su - username` | Switch to another user account for the whole session |
| `exit` | Return to your original account |
| `passwd` | Change your password |

---

### 3. Reading Permissions
> *Every file has a label — learn to read it*

```text
- rw- r-- r--
|  |   |   |
|  |   |   +-- others  (everyone else)
|  |   +------ group   (the team)
|  +---------- owner   (you)
+------------- type    (- = file, d = folder, l = link)
```

| Letter | Means |
|:--|:--|
| `r` | Read — open and view |
| `w` | Write — edit or delete |
| `x` | Execute — run a program (or enter a folder) |

---

### 4. Changing Permissions with `chmod`
> *Set who is allowed to read, write, or run a file*

| Style | Example | What it does |
|:--|:--|:--|
| Symbolic | `chmod u+x file` | Add execute for the owner |
| Symbolic | `chmod g-w file` | Remove write from the group |
| Numeric | `chmod 755 file` | Owner: all, Group: read+run, Others: read+run |
| Numeric | `chmod 644 file` | Owner: read+write, Group: read, Others: read |

---

### 5. Changing Ownership
> *Changing who owns a file and which team it belongs to*

| Command | What it does |
|:--|:--|
| `chown bob file` | Change the file owner to bob |
| `chown bob:dev file` | Change owner to bob and group to dev |
| `chgrp nurses file` | Change only the group, leave the owner alone |

---

### 6. Special Permissions
> *Two extra rules for unusual situations*

| Concept | Simple analogy |
|:--|:--|
| SUID | A vending machine that temporarily uses the master key to do one job |
| Sticky bit | A shared supply room where you can only take back what you put there |

---

### 7. Access Control Lists (ACLs)
> *Giving one specific person access without changing anything else*

| Command | What it does |
|:--|:--|
| `getfacl file` | View all permissions, including any extra ACL entries |
| `setfacl -m u:bob:r-- file` | Give bob read access only |
| `setfacl -b file` | Remove all extra ACL entries |

---

## Before Class — Quick Checklist

You don't need to install anything. Just make sure:

- [ ] You have access to **KillarCode** (the online terminal we use)
- [ ] You can open the terminal and see a `$` prompt
- [ ] You remember `ls`, `mkdir`, and `touch` from Session 2

If you've forgotten how to open KillarCode, check the Session 1 notes.

---
