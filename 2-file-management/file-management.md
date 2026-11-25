# Session 2 — File Management & Editors

**Batch:** Scaler Working Professionals, 9:00 PM  
**Duration:** 9:00 – 11:00 PM (Break 10:00 – 10:10 PM)  
**Audience:** Beginners — IT and non-IT professionals, Python learners  
**Practice environment:** KillarCode online terminal

---

## 📋 Instructor Schedule

| Time | Topic |
|:--|:--|
| 9:00 – 9:10 | Welcome & orientation |
| 9:10 – 9:33 | File & directory operations |
| 9:33 – 9:53 | Viewing file content |
| 9:53 – 10:00 | Recap & break |
| 10:00 – 10:10 | ☕ Break |
| 10:10 – 10:28 | File searching: `find` and `locate` |
| 10:28 – 10:43 | Hard links & soft links |
| 10:43 – 10:53 | Compression & archiving |
| 10:53 – 11:00 | `vi` editor & full recap |

---

## 🗂️ What We Are Building Together

Imagine you are organising your weekend plans — notes, a shopping list, and a backup copy.  
We will create this folder structure step by step, using only the terminal:

```text
class2_practice/
└── weekend_plan/
    ├── notes/        ← your personal notes
    ├── shopping/     ← grocery list
    ├── backup/       ← safe copies
    └── photos/
        └── family/   ← nested folder example
```

> **Think of it like this:** folders on your phone (Photos → Albums → Family). Same idea, just in the terminal.

---

## 9:00–9:10 — Welcome & Orientation

**Say to class:**

> "Hi everyone — welcome! You might be new to typing commands in a terminal, even if you have worked for 10+ years. That is completely okay. Today we learn together. We will use real-life examples — shopping lists, notes, folders — things everyone uses. I will type a command, explain it in plain words, and then we try it together."

**On the board / iPad, draw this analogy:**

```text
📱 Phone                    💻 Terminal
--------------------------------------------
Photos folder          →    directory / folder
A photo file           →    a file
Rename a photo         →    mv oldname newname
Delete a photo         →    rm filename
```

**Type these two commands first — everyone types along:**

```bash
pwd
ls
```

**What `pwd` does:** Asks *"Where am I right now?"* — like checking your current location on Google Maps.

**Example output:**
```text
/home/student
```

**What `ls` does:** Asks *"What is in this folder?"* — like opening a folder on your phone to see what's inside.

**Example output:**
```text
Documents  Downloads  Pictures
```

> **Say:** "`pwd` = 'Where am I?' and `ls` = 'What's here?' — you will use these two every single day."

---

## 1. File & Directory Operations (9:10 – 9:33)

> **Directory** is just the technical word for **folder**. They are the same thing.

---

### 1.1 `mkdir` — Create a Folder

> **What it does:** Makes a new folder. Think of creating a new folder on your desktop.  
> `-p` lets you create a folder inside a folder inside a folder — all in one command.

**Say:** *"Let's build our weekend plan folders, just like the drawing on the board."*

```bash
mkdir -p class2_practice
cd class2_practice
mkdir weekend_plan
mkdir weekend_plan/notes weekend_plan/shopping weekend_plan/backup
mkdir -p weekend_plan/photos/family
ls weekend_plan
```

**Example output:**
```text
backup  notes  photos  shopping
```

**What each line does:**

| Command | What it does |
|:--|:--|
| `mkdir -p class2_practice` | Creates the main practice folder (no error if it already exists) |
| `cd class2_practice` | Goes inside that folder |
| `mkdir weekend_plan` | Creates the weekend plan folder |
| `mkdir weekend_plan/notes ...` | Creates notes, shopping, and backup folders inside weekend_plan |
| `mkdir -p weekend_plan/photos/family` | Creates `photos` and `family` inside it — in one command |
| `ls weekend_plan` | Shows what's inside weekend_plan |

> **`-p` means "parents"** — create every folder along the path, don't give an error if any already exist.

---

### 1.2 `touch` — Create an Empty File

> **What it does:** Creates a new blank file — like opening Notepad and saving an empty document.  
> The file has no content yet; it's just a placeholder.

```bash
touch weekend_plan/shopping/groceries.txt
touch weekend_plan/notes/saturday.txt
touch weekend_plan/notes/sunday.txt
ls weekend_plan/shopping
```

**Example output:**
```text
groceries.txt
```

> **Why `.txt`?** The `.txt` ending is just a label — it tells humans (and programs) this is a plain text file. Linux doesn't require it, but it's good habit.

**🧑‍💻 Quick Exercise — Try it yourself:**

Create `weekend_plan/notes/idea.txt` and list the notes folder.

```bash
touch weekend_plan/notes/idea.txt
ls weekend_plan/notes
```

**Expected output:**
```text
idea.txt  saturday.txt  sunday.txt
```

---

### 1.3 `echo` — Write Text Into a File

> **What it does:** Prints text. When combined with `>`, it saves that text into a file.  
> Think of it as: *"type this sentence, then save it to that file".*

```bash
echo "Milk, Eggs, Bread" > weekend_plan/shopping/groceries.txt
```

**Check what's inside the file:**
```bash
cat weekend_plan/shopping/groceries.txt
```

**Example output:**
```text
Milk, Eggs, Bread
```

> **`>` means "write to file"** — it creates the file if it doesn't exist, or overwrites it if it does.  
> **`>>` means "add to file"** — appends a new line without erasing what's already there.

```bash
echo "Tomatoes" >> weekend_plan/shopping/groceries.txt
cat weekend_plan/shopping/groceries.txt
```

**Output:**
```text
Milk, Eggs, Bread
Tomatoes
```

---

### 1.4 `cp` — Copy a File or Folder

> **What it does:** Makes a copy of a file. The original stays exactly where it is.  
> Same as Ctrl+C, Ctrl+V on Windows — but in the terminal.

```bash
cp weekend_plan/shopping/groceries.txt weekend_plan/shopping/groceries_backup.txt
ls weekend_plan/shopping
```

**Example output:**
```text
groceries.txt  groceries_backup.txt
```

**Copy an entire folder** (need `-r` for folders):

```bash
cp -r weekend_plan weekend_plan_fullbackup
ls
```

**Example output:**
```text
class2_practice  weekend_plan  weekend_plan_fullbackup
```

| Flag | Meaning |
|:--|:--|
| *(none)* | Copy a single file |
| `-r` | Copy a folder and **everything inside** it (r = recursive) |

> **Say:** *"Where have you used copy-paste before? This is the same thing, just in the terminal. `cp` = copy."*

---

### 1.5 `mv` — Move or Rename

> **What it does:** One command, two uses:
> - **Same folder + new name** → it renames the file
> - **Different folder** → it moves the file there

**Rename a file:**
```bash
mv weekend_plan/notes/saturday.txt weekend_plan/notes/saturday_plan.txt
ls weekend_plan/notes
```

**Example output:**
```text
idea.txt  saturday_plan.txt  sunday.txt
```

**Move a file to another folder:**
```bash
mv weekend_plan/notes/sunday.txt weekend_plan/backup/
ls weekend_plan/notes
ls weekend_plan/backup
```

**`ls weekend_plan/notes` output:**
```text
idea.txt  saturday_plan.txt
```

**`ls weekend_plan/backup` output:**
```text
sunday.txt
```

> **Think of it like:** dragging a file from one folder to another on your desktop.

---

### 1.6 `rm` — Remove / Delete a File

> **What it does:** Permanently deletes a file.  
> ⚠️ **Unlike Windows, there is NO Recycle Bin.** The file is gone immediately.  
> Always use `-i` (interactive) while learning — it asks for confirmation first.

```bash
touch weekend_plan/notes/mistake.txt
rm -i weekend_plan/notes/mistake.txt
```

**What you see in the terminal:**
```text
rm: remove regular file 'weekend_plan/notes/mistake.txt'? 
```

Type `y` and press Enter to confirm.

**Check it's gone:**
```bash
ls weekend_plan/notes
```

**Output:**
```text
idea.txt  saturday_plan.txt
```

**Remove a whole folder and everything inside it:**
```bash
rm -r weekend_plan/photos/family
```

| Flag | What it does |
|:--|:--|
| `-i` | Ask "are you sure?" before deleting (safe, use this while learning) |
| `-r` | Delete a folder and everything inside (recursive) |
| `-rf` | Force delete everything, no questions asked — **be very careful** |

> ⚠️ **Warning:** `rm -rf` on the wrong folder can delete everything instantly. Always double-check the name. We will not use `-rf` in this class.

**🧑‍💻 Quick Exercise:**

```bash
touch weekend_plan/notes/delete_me.txt
rm -i weekend_plan/notes/delete_me.txt
# type y and press Enter
ls weekend_plan/notes
```

---

## 2. Viewing File Content (9:33 – 9:53)

**Say:** *"Now imagine someone left you a note. There are different ways to read a short note versus a very long note."*

First, let's create a sample notes file to practice with:

```bash
printf 'Plan the week\nBuy groceries\nCall family\nRead a book\nPrepare lunch\n' > weekend_plan/notes/weekly_notes.txt
```

This creates a file with 5 lines. We will use this file for all the commands below.

---

### 2.1 `cat` — Read the Whole File

> **What it does:** Prints every line of a file to the screen — all at once.  
> Great for short files. If the file has 10,000 lines, it all rushes past instantly.

```bash
cat weekend_plan/notes/weekly_notes.txt
```

**Example output:**
```text
Plan the week
Buy groceries
Call family
Read a book
Prepare lunch
```

> **Python connection:** When you open a file in Python with `open('file.txt').read()`, it does the same thing — reads the whole file. `cat` is the terminal version.

---

### 2.2 `head` — Read Only the Top of a File

> **What it does:** Shows only the **first few lines**. Default is 10 lines.  
> Use `-n` to choose how many lines you want.

```bash
head -n 3 weekend_plan/notes/weekly_notes.txt
```

**Example output:**
```text
Plan the week
Buy groceries
Call family
```

**Show just the first line:**
```bash
head -n 1 weekend_plan/notes/weekly_notes.txt
```

**Output:**
```text
Plan the week
```

> **Real-world use:** Imagine a CSV file with 50,000 rows of data. You just want to check what the columns are. `head -n 5 data.csv` shows the first 5 rows — without loading the whole thing.

**🧑‍💻 Quick Exercise:**

Show only the first 2 lines of the weekly notes.

```bash
head -n 2 weekend_plan/notes/weekly_notes.txt
```

**Expected output:**
```text
Plan the week
Buy groceries
```

---

### 2.3 `tail` — Read Only the Bottom of a File

> **What it does:** Shows only the **last few lines**. Default is 10 lines.

```bash
tail -n 2 weekend_plan/notes/weekly_notes.txt
```

**Example output:**
```text
Read a book
Prepare lunch
```

**Show just the last line:**
```bash
tail -n 1 weekend_plan/notes/weekly_notes.txt
```

**Output:**
```text
Prepare lunch
```

---

### 2.4 `tail -f` — Watch a File Update in Real Time

> **What it does:** Keeps the file open and shows any new lines added to it, live.  
> Think of it as watching a WhatsApp group — new messages appear as they arrive.  
> Press `Ctrl+C` to stop watching.

```bash
tail -f weekend_plan/notes/weekly_notes.txt
```

**What you see:** The last few lines appear. The cursor blinks and waits.  
If someone (or a program) adds a new line to the file, it appears here instantly.  
Press `Ctrl+C` to exit.

> **Say:** *"Imagine you're writing a Python script that saves progress to a text file line by line. `tail -f` lets you watch that file update in real time. Very handy."*

---

### 2.5 `less` — Scroll Through a Long File

> **What it does:** Opens a file in a **scrollable reader**. The whole file does NOT print at once.  
> You navigate with keyboard keys, search inside it, and press `q` to leave.

```bash
less weekend_plan/notes/weekly_notes.txt
```

**What you see:** The file opens in a full-screen view.

**Keys to use inside `less`:**

| Key | What it does |
|:--|:--|
| `↑` / `↓` arrow keys | Scroll one line at a time |
| `Space` | Scroll down one full page |
| `/family` then `Enter` | Search for the word "family" |
| `n` | Jump to next match |
| `q` | **Quit** — returns you to the terminal |

**To search:** type `/` followed by any word and press Enter. The matching word is highlighted.

> **Say:** *"If you have a Python error log that's 500 lines long, `less` lets you scroll through it and search for the word 'Error' — much better than `cat`."*

---

## ☕ 10:00–10:10 — Break

> **Recap before break — ask the class:**
> - "Which command shows the whole file?" → `cat`
> - "Which shows only the top?" → `head`
> - "Which shows only the bottom?" → `tail`
> - "How do you quit `less`?" → press `q`

---

## 3. File Searching (10:10 – 10:28)

**Say:** *"Has anyone ever saved a file and then forgotten which folder it went into? We've all been there. Let's learn how to find files from the terminal."*

**Two ways to search:**

```text
find   →  searches your folders right now (like searching your whole phone)
locate →  checks a saved list of file paths (faster, but the list may be old)
```

---

### 3.1 `find` — Search for Files Right Now

> **What it does:** Walks through folders and finds files or folders that match what you're looking for.  
> You tell it: *where to look*, and *what to look for*.

**Basic pattern:**
```bash
find <where to look> <what to look for>
```

---

**Search by exact filename:**
```bash
find weekend_plan -name "weekly_notes.txt"
```

**Example output:**
```text
weekend_plan/notes/weekly_notes.txt
```

---

**Search by file extension — find all `.txt` files:**
```bash
find weekend_plan -name "*.txt"
```

The `*` means *"anything"* — so `*.txt` means *"any file that ends in .txt"*.

**Example output:**
```text
weekend_plan/notes/weekly_notes.txt
weekend_plan/notes/saturday_plan.txt
weekend_plan/notes/idea.txt
weekend_plan/shopping/groceries.txt
weekend_plan/shopping/groceries_backup.txt
```

---

**Find only files (not folders):**
```bash
find weekend_plan -type f
```

**Example output:**
```text
weekend_plan/notes/weekly_notes.txt
weekend_plan/notes/saturday_plan.txt
weekend_plan/shopping/groceries.txt
```

---

**Find only folders (directories):**
```bash
find weekend_plan -type d
```

**Example output:**
```text
weekend_plan
weekend_plan/notes
weekend_plan/shopping
weekend_plan/backup
weekend_plan/photos
```

---

**Find files larger than a certain size:**
```bash
find weekend_plan -size +100M
```

**Example output:**
```text
(nothing — our practice files are tiny)
```

> **Real-world use:** `find /home -size +500M` — find files bigger than 500 MB anywhere in your home folder. Useful when your storage is full and you don't know why.

---

**Find files changed within the last day:**
```bash
find weekend_plan -mtime -1
```

**Example output:**
```text
weekend_plan/notes/weekly_notes.txt
weekend_plan/shopping/groceries.txt
```

*(All files you just created today will appear here.)*

---

**`find` quick-reference table:**

| What you want | Command |
|:--|:--|
| Find by exact name | `find folder -name "notes.txt"` |
| Find all `.txt` files | `find folder -name "*.txt"` |
| Find only files | `find folder -type f` |
| Find only folders | `find folder -type d` |
| Find large files (>100 MB) | `find folder -size +100M` |
| Find recently changed files | `find folder -mtime -1` |

**🧑‍💻 Quick Exercise:**

Find the file `idea.txt` inside the weekend_plan folder.

```bash
find weekend_plan -name "idea.txt"
```

**Expected output:**
```text
weekend_plan/notes/idea.txt
```

---

**Searching from the very top of Linux:**
```bash
find / -name "weekly_notes.txt" 2>/dev/null
```

**Example output:**
```text
/home/student/class2_practice/weekend_plan/notes/weekly_notes.txt
```

> `2>/dev/null` hides "Permission denied" messages so the output stays clean. We won't memorise this — just know it exists.

---

### 3.2 `locate` — Search a Saved Index (Faster)

> **What it does:** Instead of searching folders right now, `locate` checks a pre-built list of all file paths on the computer — like a book's index page.  
> It's faster than `find`, but it might miss very recently created files.

```bash
locate weekly_notes.txt
```

**Example output (if `locate` is installed):**
```text
/home/student/class2_practice/weekend_plan/notes/weekly_notes.txt
```

> If you get no output, the index is outdated. It can be refreshed with `sudo updatedb`.  
> KillarCode may not have `locate` installed — that's okay, understand the concept.

**`find` vs `locate` — when to use which:**

| | `find` | `locate` |
|:--|:--|:--|
| Searches | Right now, live | A saved index |
| Speed | Slower (checks every file) | Faster |
| Finds brand new files? | ✅ Yes | ❌ Not until index updates |
| Always available? | ✅ Yes | ❌ Needs to be installed |

---

## 4. Hard Links & Soft Links (10:28 – 10:43)

**Say:** *"Imagine you have one shopping list saved on your phone, and you want to access it from two different apps — without making a second copy. Linux links work like that."*

**Draw on the board:**

```text
Hard link:   [ File data on disk ] ← name1.txt
                                  ← name2.txt  (same file, two names)

Soft link:   shortcut.txt  ──points to──▶  original.txt  ──▶  [ File data ]
```

---

### 4.1 `ln` — Hard Link (Two Names, One File)

> **What it does:** Creates a second name for the exact same file.  
> Both names point to the **same content on disk**. Edit through one name — the other sees the change too.  
> Think of it as: one document with two labels on it.

```bash
echo "My shopping list" > original.txt
ln original.txt hardlink_copy.txt
ls -li original.txt hardlink_copy.txt
```

**Example output:**
```text
524291 -rw-r--r-- 2 student student 17 Nov 25 21:00 original.txt
524291 -rw-r--r-- 2 student student 17 Nov 25 21:00 hardlink_copy.txt
```

> The first column is the **inode number** — the file's internal ID on disk.  
> Both files show **the same inode number** (`524291`). That proves they are the same file, just with two different names.

**Edit through one name, check the other:**
```bash
echo "Also buy milk" >> hardlink_copy.txt
cat original.txt
```

**Output:**
```text
My shopping list
Also buy milk
```

The change shows up in `original.txt` even though we edited `hardlink_copy.txt` — because they are the same file.

**Delete one name — the file still exists:**
```bash
rm -i original.txt
cat hardlink_copy.txt
```

**Output:**
```text
My shopping list
Also buy milk
```

> The file data is **not deleted** until every hard link to it is removed.

---

### 4.2 `ln -s` — Soft Link / Symbolic Link (a Shortcut)

> **What it does:** Creates a shortcut file that points to another file's **location (path)**.  
> Think of it like a **desktop shortcut** on Windows.  
> If the original file is moved or deleted, the shortcut stops working.

```bash
ln -s hardlink_copy.txt my_shortcut.txt
ls -la my_shortcut.txt
cat my_shortcut.txt
```

**`ls -la` output:**
```text
lrwxrwxrwx 1 student student 16 Nov 25 21:05 my_shortcut.txt -> hardlink_copy.txt
```

The `->` shows it's a shortcut pointing to `hardlink_copy.txt`.

**`cat my_shortcut.txt` output:**
```text
My shopping list
Also buy milk
```

**Delete the shortcut — the original file is unaffected:**
```bash
rm -i my_shortcut.txt
cat hardlink_copy.txt
```

**Output:**
```text
My shopping list
Also buy milk
```

**What happens if the target file is deleted first (dangling link)?**
```bash
ln -s hardlink_copy.txt broken_shortcut.txt
rm -i hardlink_copy.txt
cat broken_shortcut.txt
```

**Output:**
```text
cat: broken_shortcut.txt: No such file or directory
```

> This is called a **dangling link** — a shortcut pointing to something that no longer exists.

---

**Hard link vs Soft link — side by side:**

| | Hard Link | Soft Link |
|:--|:--|:--|
| Create with | `ln file link` | `ln -s file link` |
| Points to | Same file content (inode) | File's path/location |
| Still works if original deleted? | ✅ Yes | ❌ No (dangling link) |
| Works across different drives? | ❌ No | ✅ Yes |
| Similar to | A file with two names | A Windows shortcut |

**🧑‍💻 Quick Exercise — which is which?**

> "Which command makes a shortcut? `cp`, `ln`, or `ln -s`?"  
> **Answer:** `ln -s` makes a shortcut. `ln` makes another name for the same file. `cp` makes a completely separate copy.

---

## 5. Compression & Archiving (10:43 – 10:53)

**Say:** *"Suppose you want to send all your weekend notes to a friend in one attachment. You'd bundle them together — that's archiving. And if you want the bundle to be smaller, you compress it. These are two separate ideas."*

**On the board:**

```text
Archiving   = packing many files into ONE file   (like a zip folder)
Compression = making a file SMALLER              (squeezing it)
tar          = one tool that can do BOTH together
```

---

### 5.1 `tar -cvf` — Bundle Files Together (No Compression)

> **What it does:** Packs an entire folder into a single `.tar` file.  
> `-c` = create, `-v` = show what's being packed, `-f` = the filename comes next.

```bash
tar -cvf weekend_plan.tar weekend_plan/
```

**Example output:**
```text
weekend_plan/
weekend_plan/notes/
weekend_plan/notes/weekly_notes.txt
weekend_plan/notes/saturday_plan.txt
weekend_plan/notes/idea.txt
weekend_plan/shopping/
weekend_plan/shopping/groceries.txt
weekend_plan/backup/
```

> The `v` flag shows each file being added. Without `v`, the command is silent.

---

### 5.2 `tar -czvf` — Bundle AND Compress with gzip

> **What it does:** Packs AND compresses the folder. Result is a `.tar.gz` file (smaller than `.tar`).  
> `z` = use gzip compression.

```bash
tar -czvf weekend_plan.tar.gz weekend_plan/
```

**Example output:** *(same list as above, but now saved as a compressed `.tar.gz`)*

**Compare the sizes:**
```bash
ls -lh weekend_plan.tar weekend_plan.tar.gz
```

**Example output:**
```text
-rw-r--r-- 1 student student 20K Nov 25 21:10 weekend_plan.tar
-rw-r--r-- 1 student student  2K Nov 25 21:10 weekend_plan.tar.gz
```

> The `.tar.gz` is much smaller — compression is working!

---

### 5.3 `tar -tzvf` — Peek Inside an Archive

> **What it does:** Lists the contents of an archive **without unpacking it**.  
> Like reading the label on a closed box.

```bash
tar -tzvf weekend_plan.tar.gz
```

**Example output:**
```text
drwxr-xr-x student/student   0 2026-11-25 21:00 weekend_plan/
drwxr-xr-x student/student   0 2026-11-25 21:00 weekend_plan/notes/
-rw-r--r-- student/student  61 2026-11-25 21:00 weekend_plan/notes/weekly_notes.txt
-rw-r--r-- student/student   5 2026-11-25 21:00 weekend_plan/shopping/groceries.txt
```

---

### 5.4 `tar -xzvf` — Unpack / Extract an Archive

> **What it does:** Unpacks (extracts) all files from the archive.  
> `-C folder` tells tar where to put the extracted files.

```bash
mkdir restored_files
tar -xzvf weekend_plan.tar.gz -C restored_files
ls restored_files
```

**Example output:**
```text
weekend_plan
```

```bash
ls restored_files/weekend_plan
```

**Output:**
```text
backup  notes  shopping
```

> Everything is restored exactly as it was.

---

### 5.5 Archive Only a Subfolder

> **What it does:** You don't have to archive everything — just pick the folder you need.

```bash
tar -czvf notes_only.tar.gz weekend_plan/notes/
```

**Example output:**
```text
weekend_plan/notes/
weekend_plan/notes/weekly_notes.txt
weekend_plan/notes/saturday_plan.txt
weekend_plan/notes/idea.txt
```

---

### `tar` Flags — Plain English

| Flag | Stands for | What it does |
|:--|:--|:--|
| `c` | **C**reate | Make a new archive |
| `x` | e**X**tract | Unpack an archive |
| `t` | lis**T** | Show contents without unpacking |
| `v` | **V**erbose | Print file names as they're processed |
| `f` | **F**ile | The archive filename comes right after this flag |
| `z` | gzip | Compress/decompress using gzip (`.tar.gz`) |
| `j` | bzip2 | Compress using bzip2 (`.tar.bz2`) — smaller, slower |
| `J` | xz | Compress using xz (`.tar.xz`) — smallest, slowest |
| `-C dir` | **C**hange dir | Extract into this folder |

**🧑‍💻 Quick Exercise:**

> "Which flag lists contents without extracting?" → `t`  
> "Which flag actually extracts?" → `x`  
> "Does `tar` always compress?" → **No.** You need to add `z`, `j`, or `J` to compress.

---

## 6. `vi` / Vim Editor (10:53 – 11:00)

**Say:** *"The last topic — `vi`. It's a text editor that works right inside the terminal. It might feel unusual at first because it has two modes — one for reading/navigating and one for typing. We'll go slowly."*

**Draw on the board:**

```text
                    press i
COMMAND mode   ───────────────▶   INSERT mode  (you can type text)
               ◀───────────────
                    press Esc
```

> `vi` always opens in **Command mode**. You MUST press `i` before you can type anything.

---

### 6.1 Open (or Create) a File

```bash
vi my_notes.txt
```

**What you see:** A blank screen with `~` symbols on empty lines. That's normal.  
You are in **Command mode** — do not type yet.

---

### 6.2 Switch to Insert Mode and Type

Press the `i` key. At the bottom of the screen, you'll see:

```text
-- INSERT --
```

Now type:

```text
Buy apples
Call mom
Read a book
```

---

### 6.3 Save the File

Press `Esc` to leave Insert mode and go back to Command mode.  
You will see `-- INSERT --` disappear from the bottom.

Now type `:wq` and press Enter:

```text
:wq
```

This means: **w**rite (save) + **q**uit.

You're back at the terminal prompt.

**Verify what was saved:**
```bash
cat my_notes.txt
```

**Example output:**
```text
Buy apples
Call mom
Read a book
```

---

### 6.4 Search for a Word Inside `vi`

Open the file again:
```bash
vi my_notes.txt
```

In Command mode, type `/mom` and press Enter:

```text
/mom
```

The cursor jumps to the word `mom`. Press `n` to find the next match.

---

### 6.5 Delete a Line

Move the cursor to the line you want to delete.  
Press `dd` (two d's) in Command mode — that line disappears.

**Exit without saving the deletion:**
```text
:q!
```

The `!` means "I'm sure — don't save." The file stays as it was before.

---

### `vi` Command Reference Card

| Press this | When you're in | What happens |
|:--|:--|:--|
| `i` | Command mode | Switch to Insert mode (start typing) |
| `Esc` | Insert mode | Switch back to Command mode |
| `:w` | Command mode | Save the file (write) |
| `:q` | Command mode | Quit (only if no unsaved changes) |
| `:wq` | Command mode | Save AND quit |
| `:q!` | Command mode | Quit WITHOUT saving — discard changes |
| `dd` | Command mode | Delete the entire current line |
| `/word` | Command mode | Search for "word" in the file |
| `n` | Command mode | Jump to the next search match |

**🧑‍💻 Quick Exercise:**

1. Open `my_notes.txt` with `vi`
2. Press `i`, add a new line: `Prepare lunch`
3. Press `Esc`
4. Type `:wq` and press Enter
5. Run `cat my_notes.txt` — verify all 4 lines are there

**Expected output:**
```text
Buy apples
Call mom
Read a book
Prepare lunch
```

> **`vi` vs `vim`:** `vim` = "Vi Improved". Same modes, same commands — but vim adds colour, more undo steps, and plugins. Everything you learned today works in both.

---

## 7. Final Recap (10:59 – 11:00)

**Say:** *"Before we close — name one command you will remember from tonight."*  
*(Take a few quick answers from the class.)*

> *"You don't need to memorise everything today. Keep this file open, practise the safe commands — `ls`, `mkdir`, `touch`, `cat`, `cp`, `mv`. Use `rm -i` while learning. That's enough to get started. Thank you all!"*

---

## 📌 All Commands — Quick Reference

| Command | What It Does | Simple Example |
|:--|:--|:--|
| `pwd` | Where am I? | `pwd` |
| `ls` | What's in this folder? | `ls weekend_plan` |
| `mkdir folder` | Create a folder | `mkdir notes` |
| `mkdir -p a/b/c` | Create nested folders at once | `mkdir -p photos/family` |
| `touch file.txt` | Create an empty file | `touch ideas.txt` |
| `echo "text" > file` | Write text into a file | `echo "Milk" > list.txt` |
| `echo "text" >> file` | Add text to a file (append) | `echo "Eggs" >> list.txt` |
| `cp file copy` | Copy a file | `cp list.txt list_backup.txt` |
| `cp -r folder copy` | Copy a whole folder | `cp -r notes notes_backup` |
| `mv old new` | Rename a file | `mv notes.txt plan.txt` |
| `mv file folder/` | Move a file to a folder | `mv plan.txt backup/` |
| `rm -i file` | Delete file (asks to confirm) | `rm -i mistake.txt` |
| `rm -r folder` | Delete a whole folder | `rm -r old_notes` |
| `cat file` | Read the whole file | `cat weekly_notes.txt` |
| `head -n 3 file` | Read first 3 lines | `head -n 3 notes.txt` |
| `tail -n 3 file` | Read last 3 lines | `tail -n 3 notes.txt` |
| `tail -f file` | Watch file update live | `tail -f notes.txt` |
| `less file` | Scroll through a file | `less big_file.txt` |
| `find folder -name "*.txt"` | Find all `.txt` files | `find weekend_plan -name "*.txt"` |
| `find folder -type f` | Find only files | `find weekend_plan -type f` |
| `find folder -type d` | Find only folders | `find weekend_plan -type d` |
| `find folder -size +100M` | Find files bigger than 100 MB | `find /home -size +100M` |
| `find folder -mtime -1` | Find files changed today | `find weekend_plan -mtime -1` |
| `locate filename` | Search the saved file index | `locate groceries.txt` |
| `ln file link` | Create a hard link | `ln original.txt copy_name.txt` |
| `ln -s file shortcut` | Create a shortcut | `ln -s original.txt shortcut.txt` |
| `ls -li` | Show inode numbers | `ls -li original.txt` |
| `tar -cvf out.tar folder/` | Bundle a folder | `tar -cvf backup.tar notes/` |
| `tar -czvf out.tar.gz folder/` | Bundle + compress | `tar -czvf backup.tar.gz notes/` |
| `tar -tzvf file.tar.gz` | List archive contents | `tar -tzvf backup.tar.gz` |
| `tar -xzvf file.tar.gz -C dir/` | Extract to a folder | `tar -xzvf backup.tar.gz -C restored/` |
| `vi file` | Open terminal text editor | `vi my_notes.txt` |

---

## ✅ PDF Topic Checklist

- [x] **File & directory operations** — `mkdir`, `mkdir -p`, `touch`, `echo >`, `echo >>`, `cp`, `cp -r`, `mv` (rename + move), `rm -i`, `rm -r`
- [x] **Viewing file content** — `cat`, `head -n`, `tail -n`, `tail -f`, `less` (scroll, search with `/word`, next with `n`, quit with `q`)
- [x] **File searching** — `find` by name, extension (`*.txt`), type (`-type f`, `-type d`), size (`-size`), modification time (`-mtime`); `locate` and `updatedb`; `2>/dev/null`
- [x] **Hard links & soft links** — inode concept, `ln` (hard link), `ln -s` (symbolic link), `ls -li`, `ls -la`, dangling link, what happens when target is deleted
- [x] **Compression & archiving** — archive vs compression difference, `tar` flags: `c` `x` `t` `v` `f` `z` `j` `J`, archive whole folder, archive subfolder, extract with `-C`, compare file sizes
- [x] **`vi` / Vim editor** — Command mode vs Insert mode, `i`, `Esc`, `:w`, `:q`, `:wq`, `:q!`, `dd`, `/word` search, `n` next match, `vi` vs `vim`
