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

- -l → Detailed view (permissions, owner, size, date)

- -a → Show hidden files too (e.g., .bashrc, .git)

### 📂 Change Directory
```bash
cd folder_name
```

#### Special:

```bash
cd ..       # Go one level up
cd ~        # Go to home directory
cd -        # Go back to previous directory
```

## 2. File / Folder Operations
### 📄 Create File
```bash
touch file.txt
```
### 📁 Create Folder
```bash
mkdir folder_name
mkdir -p parent/child/grandchild   # Create nested folders at once
```
### ❌ Delete File
```bash
rm file.txt
rm -i file.txt    # Interactive (asks for confirmation)
```
### ❌ Delete Folder
```bash
rm -r folder_name       # Recursive (deletes everything inside)
rm -rf folder_name      # Force + Recursive (⚠️ No confirmation)
```
### 📋 Copy
```bash
cp file.txt copy.txt
cp -r folder1 folder2   # Use -r to copy folders
```
### 🔁 Move / Rename
```bash
mv old.txt new.txt       # Rename
mv file.txt /tmp/        # Move to another directory
```
## 3. Viewing / Editing Files
### 👀 View File Content
```bash
cat file.txt           # View entire content at once
head -n 10 file.txt    # First 10 lines
tail -n 10 file.txt    # Last 10 lines
tail -f log.txt        # Follow live logs
```
### 📜 Scroll View
```bash
less file.txt
```
- Space → Next page
- b → Previous page
- q → Quit

### ✏️ Edit File
```bash
nano file.txt          # Beginner-friendly editor
vim file.txt           # Advanced editor
```
### 🔍 Search in File
```bash
grep "keyword" file.txt
grep -r "keyword" .    # Recursive search in current directory
```
---

## 🎯 What's Next?
Now that you understand Linux Basics, move on to Part 2: Permissions, Processes & System Info.

Happy Learning! Keep Exploring Linux! 🐧

---
[< Previous](../README.md) ---- [Next >](../part-02/README.md)