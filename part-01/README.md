# 🐧 Part 1: Linux Basics (Navigation, Files, Editing)

Welcome to **Part 1** of the Linux Complete Guide. In this section, we will cover the fundamental commands for navigating the filesystem, managing files/folders, and viewing/editing file contents.

---

## 📑 Table of Contents
1. [Navigation](#1-navigation)
2. [File / Folder Operations](#2-file--folder-operations)
3. [Viewing / Editing Files](#3-viewing--editing-files)

---

## 1. Navigation

### 📂 Check Current Directory
```bash
pwd
```
👉 Present Working Directory — shows where you currently are.

### 📂 List Files/Folders
```bash
ls
```
#### Advanced:

```bash
ls -la
```

- `-l` → Detailed view (permissions, owner, size, date)
- `-a` → Show hidden files too (e.g., .bashrc, .git)
- `-h` → Human-readable sizes (e.g., 1K, 2M)
- `-t` → Sort by modification time (newest first)
- `-r` → Reverse order while sorting

#### Combined Flags Example:
```bash
ls -ltr    # Detailed view, sorted by time, oldest first
```

### 📂 Change Directory
```bash
cd folder_name
```

#### Special:

```bash
cd ..       # Go one level up
cd ~        # Go to home directory
cd -        # Go back to previous directory
cd /        # Go to root directory
```

## 2. File / Folder Operations
### 📄 Create File
```bash
touch file.txt
```
- Creates an empty file if it doesn't exist.
- Updates the timestamp if the file already exists.

### 📁 Create Folder
```bash
mkdir folder_name
mkdir -p parent/child/grandchild   # Create nested folders at once
```
- `-p` → Creates parent directories if they don't exist.

- `-v` → Verbose (shows what is being created).



### ❌ Delete File
```bash
rm file.txt
rm -i file.txt    # Interactive (asks for confirmation)
rm -f file.txt    # Force delete (no confirmation)
rm -v file.txt    # Verbose (shows what is being deleted)
```
- `-i` → Interactive (prompts before deletion).
- `-f` → Force (ignores non-existent files, no prompt).
- `-v` → Verbose (explains what is being done).

### ❌ Delete Folder
```bash
rm -r folder_name       # Recursive (deletes everything inside)
rm -rf folder_name      # Force + Recursive (⚠️ No confirmation)

```
- `-r` → Recursive (deletes everything inside).

- `-f` → Force (no prompt).

- ⚠️ Warning: Never run `rm -rf /` — it will destroy your system.



### 📋 Copy
```bash
cp file.txt copy.txt
cp -r folder1 folder2   # Use -r to copy folders
cp -v file.txt /tmp/    # Verbose (shows what is being copied)
cp -i file.txt /tmp/    # Interactive (prompts before overwrite)
```

- `-r` → Recursive (for directories).
- `-v` → Verbose.
- `-i` → Interactive.
- `-p` → Preserve permissions, ownership, and timestamps.

### 🔁 Move / Rename
```bash
mv old.txt new.txt       # Rename
mv file.txt /tmp/        # Move to another directory
mv -v file.txt /tmp/     # Verbose
mv -i file.txt /tmp/     # Interactive
mv -n file.txt /tmp/     # No-clobber (won't overwrite existing file)
```
- `-v` → Verbose.
- `-i` → Interactive.
- `-n` → No overwrite.

## 3. Viewing / Editing Files
### 👀 View File Content
```bash
cat file.txt           # View entire content at once
cat -n file.txt        # Show line numbers
cat -A file.txt        # Show hidden characters (tabs, line endings)
head -n 10 file.txt    # First 10 lines
tail -n 10 file.txt    # Last 10 lines
tail -f log.txt        # Follow live logs
```

- `-n` → Number all output lines.
- `-A` → Show all (including special characters).

### 📜 Scroll View
```bash
less file.txt
```
- Space → Next page
- b → Previous page
- q → Quit
- /keyword → Search forward
- ?keyword → Search backward

### ✏️ Edit File
```bash
nano file.txt          # Beginner-friendly editor
vim file.txt           # Advanced editor
```
#### Nano Shortcuts:
- `Ctrl + O` → Save file
- `Ctrl + X` → Exit
- `Ctrl + W` → Search

#### Vim Shortcuts:

- `i` → Insert mode
- `Esc` → Normal mode
- `:w` → Save
- `:q` → Quit
- `:wq` → Save and quit
- `:q!` → Quit without saving

### 🔍 Search in File
```bash
grep "keyword" file.txt
grep -r "keyword" .     # Recursive search in current directory
grep -i "keyword" file.txt  # Case-insensitive search
grep -n "keyword" file.txt  # Show line numbers
grep -v "keyword" file.txt  # Invert match (show non-matching lines)
```

- `-r` → Recursive.
- `-i` → Ignore case.
- `-n` → Show line numbers.
- `-v` → Invert match.
- `-c` → Count matches.

---

## 🎯 What's Next?
Now that you understand Linux Basics, move on to Part 2: Permissions, Processes & System Info.

Happy Learning! Keep Exploring Linux! 🐧

---
[< Previous](../README.md) ---- [Next >](../part-02/README.md)