# 🐧 Part 2: Folder Structure & Navigation

Welcome to **Part 2** of the Ultimate Linux Guide. In this section, we will learn how to navigate the Linux filesystem and understand its structure.

---

## 📑 Table of Contents
1. [Navigation Commands](#1-navigation-commands)
2. [Understanding Paths](#2-understanding-paths)
3. [File / Folder Operations](#3-file--folder-operations)

---

## 1. Navigation Commands

### 📂 Check Current Directory
```bash
pwd
```

- Present Working Directory — shows where you currently are.

### 📂 List Files/Folders
```bash
ls
```
**Advanced:**

```bash
ls -la
```
- `-l` → Detailed view (permissions, owner, size, date)

- `-a` → Show hidden files too (e.g., .bashrc, .git)

- `-h` → Human-readable sizes (e.g., 1K, 2M)

- `-t` → Sort by modification time (newest first)

- `-r` → Reverse order while sorting

**Combined Flags Example:**

```bash
ls -ltr    # Detailed view, sorted by time, oldest first
```
### 📂 Change Directory
```bash
cd folder_name
```

**Special:**

```bash
cd ..       # Go one level up
cd ~        # Go to home directory
cd -        # Go back to previous directory
cd /        # Go to root directory
```
## 2. Understanding Paths
### Absolute Path:
Starts with `/` (e.g., `/home/shivam/file.txt`).

### Relative Path:
Starts from current directory (e.g., `../file.txt`).

### Special Path Symbols:

| #   | Symbol    | Meaning     |
| --- | -------- | ------------- |
| 01  | `.`    | Current directory |
| 02  | `..`    | Parent directory |
| 03  | `~`    | Home directory |
| 04  | `/`    | Root directory |


## 3. File / Folder Operations
### 📄 Create File
```bash
touch file.txt
```
- Creates an empty file if it doesn't exist.
- Updates the timestamp if the file already exists.

### 📁 Create Folder
```bash
mkdir folder_name
```
**Advanced:**

```bash
mkdir -p parent/child/grandchild   # Create nested folders at once
```
- `-p` → Creates parent directories if they don't exist.

- `-v` → Verbose (shows what is being created).

### ❌ Delete File
```bash
rm file.txt
```
**Advanced:**

```bash
rm -i file.txt    # Interactive (asks for confirmation)
rm -f file.txt    # Force delete (no confirmation)
rm -v file.txt    # Verbose (shows what is being deleted)
```
- `-i` → Interactive (prompts before deletion).

- `-f` → Force (ignores non-existent files, no prompt).

- `-v` → Verbose.

### ❌ Delete Folder
```bash
rm -r folder_name
```
**Advanced:**

```bash
rm -rf folder_name      # Force + Recursive (⚠️ No confirmation)
```
- `-r` → Recursive (deletes everything inside).
- `-f` → Force (no prompt).
- ⚠️ Warning: Never run rm -rf / — it will destroy your system.

### 📋 Copy
```bash
cp file.txt copy.txt
```

**Advanced:**

```bash
cp -r folder1 folder2   # Copy folders recursively
cp -v file.txt /tmp/    # Verbose
cp -i file.txt /tmp/    # Interactive
cp -p file.txt /tmp/    # Preserve permissions and timestamps
```
- `-r` → Recursive (for directories).
- `-v` → Verbose.
- `-i` → Interactive.
- `-p` → Preserve.

### 🔁 Move / Rename
```bash
mv old.txt new.txt       # Rename
mv file.txt /tmp/        # Move to another directory
```

**Advanced:**

```bash
mv -v file.txt /tmp/     # Verbose
mv -i file.txt /tmp/     # Interactive
mv -n file.txt /tmp/     # No-clobber (won't overwrite)
```
- `-v` → Verbose.
- `-i` → Interactive.
- `-n` → No overwrite.

---

## 🎯 What's Next?
Now that you understand Folder Structure & Navigation, move on to Part 3: File Management & VI Editor.

Happy Learning! Keep Exploring Linux! 🐧

---

[< Previous](../part-01/README.md) ---- [Next >](../part-03/README.md)
