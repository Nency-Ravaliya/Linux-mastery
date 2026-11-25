# 👋 Session 2 — File management

By the end of this session you will be able to:

- ✅ Create, copy, move, and delete files and folders using the terminal
- ✅ Read and scroll through file contents
- ✅ Search for a file without opening every folder
- ✅ Understand what hard links and soft links (shortcuts) are
- ✅ Bundle and compress files for sharing or backup
- ✅ Open and edit a file directly in the terminal using `vi`

## 📚 Topics We Will Cover Tonight

### 1. Working with Files & Folders
> *Creating, copying, moving, renaming, and deleting files and folders*

| Command | What it does |
|:--|:--|
| `mkdir` | Create a folder |
| `touch` | Create an empty file |
| `cp` | Copy a file or folder |
| `mv` | Move or rename a file |
| `rm` | Delete a file |

---

### 2. Reading File Content
> *Different ways to look inside a file*

| Command | What it does |
|:--|:--|
| `cat` | Show the whole file |
| `head` | Show only the first few lines |
| `tail` | Show only the last few lines |
| `tail -f` | Watch a file update in real time |
| `less` | Scroll through a long file |

---

### 3. Finding Files
> *Finding a file without opening every folder manually*

| Command | What it does |
|:--|:--|
| `find` | Search folders right now |
| `locate` | Search a saved index (faster) |

---

### 4. Hard Links & Soft Links
> *Two ways to have one file accessible from multiple places*

| Concept | Simple analogy |
|:--|:--|
| Hard link | One document with two different labels on it |
| Soft link (shortcut) | A desktop shortcut pointing to the original file |

---

### 5. Compression & Archiving
> *Bundling files together and making them smaller for backup or sharing*

| Command | What it does |
|:--|:--|
| `tar -cvf` | Bundle a folder into one file |
| `tar -czvf` | Bundle + compress (smaller file) |
| `tar -tzvf` | Peek inside without unpacking |
| `tar -xzvf` | Unpack / restore everything |

---

### 6. `vi` Text Editor
> *Editing a file directly inside the terminal — no mouse needed*

```text
COMMAND mode  ──( press i )──▶  INSERT mode (type here)
COMMAND mode  ◀──( press Esc )──  INSERT mode
```

Key commands you'll use:

| Press | What happens |
|:--|:--|
| `i` | Start typing |
| `Esc` | Stop typing |
| `:wq` | Save and quit |
| `:q!` | Quit without saving |

---

## Before Class — Quick Checklist

You don't need to install anything. Just make sure:

- [ ] You have access to **KillarCode** (the online terminal we use)
- [ ] You can open the terminal and see a `$` prompt
- [ ] You remember how to type `ls` and `pwd` from last session

If you've forgotten how to open KillarCode, check the Session 1 notes.

---

