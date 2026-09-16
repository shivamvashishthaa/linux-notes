# 🐧 Part 3: File Management & VI Editor

Welcome to **Part 3** of the Ultimate Linux Guide. In this section, we will learn how to view, edit, and manage files, along with mastering the VI Editor.

---

## 📑 Table of Contents
1. [Viewing File Content](#1-viewing-file-content)
2. [Editing Files](#2-editing-files)
3. [VI Editor Shortcuts](#3-vi-editor-shortcuts)
4. [Searching in Files](#4-searching-in-files)

---

## 1. Viewing File Content

### 👀 View File Content
```bash
cat file.txt
```

**Advanced:**

```bash
cat -n file.txt        # Show line numbers
cat -A file.txt        # Show hidden characters (tabs, line endings)
```
- `-n` → Number all output lines.
- `-A` → Show all (including special characters).

### 📜 Scroll View
```bash
less file.txt
```
**Navigation inside** `less`:
- `Space` → Next page
- `b` → Previous page
- `q` → Quit
- `/keyword` → Search forward
- `?keyword` → Search backward

### 📄 Head & Tail
```bash
head -n 10 file.txt    # First 10 lines
tail -n 10 file.txt    # Last 10 lines
tail -f log.txt        # Follow live logs
```
- `-n` → Number of lines.
- `-f` → Follow (continuously output appended data).

## 2. Editing Files
### ✏️ Nano Editor (Beginner-friendly)
```bash
nano file.txt
```
**Nano Shortcuts:**
- `Ctrl + O` → Save file
- `Ctrl + X` → Exit
- `Ctrl + W` → Search
- `Ctrl + K` → Cut line
- `Ctrl + U` → Paste line

### ✏️ Vim Editor (Advanced)
```bash
vim file.txt
```

**Vim Modes:**

- Normal Mode: Default mode (for navigation and commands).
- Insert Mode: Press i to enter (for typing).
- Visual Mode: Press v to enter (for selecting text).
- Command Mode: Press : to enter (for saving, quitting).

## 3. VI Editor Shortcuts
### Basic Commands:

| Command    | Meaning     |
| -------- | ------------- |
| `i`    | Insert before cursor |
| `I`    | Insert at beginning of line |
| `a`    | Append after cursor |
| `A`    | Append at end of line |
| `o`    | Open new line below |
| `O`    | Open new line above |
| `Esc`    | Return to Normal mode |


### Navigation:

| Command    | Meaning     |
| -------- | ------------- |
| `h`    | Move left |
| `j`    | Move down |
| `k`    | Move up |
| `l`    |Move right|
| `w`    |	Move to next word|
| `b`    | Move to previous word|
| `0`    | Move to beginning of line |
| `$`    | Move to end of line|
| `gg`    | Go to first line |
| `G`    | Go to last line |
| `:n`    | Go to line number `n` |

### Editing:

| Command    | Meaning     |
| -------- | ------------- |
| `x`    | Delete character |
| `dd`    | Delete line |
| `yy`    | 	Copy line |
| `p`    |Paste below|
| `P`    |	Paste above|
| `u`    | Undo|
| `Ctrl + r`    | Redo|
| `:w`    | 	Save|
| `:q`    | 	Quit |
| `:wq`    | 	Save and quit |
| `:q!`    | 	Quit without saving |


## Search & Replace:

| Command    | Meaning     |
| -------- | ------------- |
| `/keyword`    | Search forward |
| `?keyword`    | Search backward |
| `n`    | 	Next match |
| `N`    |Previous match|
| `:%s/old/new/g`    | Replace all occurrences|
| `:%s/old/new/gc`    | 	Replace with confirmation|

## 4. Searching in Files
### 🔍 Grep Command
```bash
grep "keyword" file.txt
```

**Advanced:**

```bash
grep -r "keyword" .     # Recursive search
grep -i "keyword" file.txt  # Case-insensitive
grep -n "keyword" file.txt  # Show line numbers
grep -v "keyword" file.txt  # Invert match
grep -c "keyword" file.txt  # Count matches
```
- `-r` → Recursive.
- `-i` → Ignore case.
- `-n` → Show line numbers.
- `-v` → Invert match.
- `-c` → Count.

### 🔍 Find Command
```bash
find /path -name "file.txt"
```
**Advanced:**

```bash
find / -type f -size +100M 2>/dev/null   # Find large files
find / -mtime -7                         # Files modified in last 7 days
find / -name "*.log"                     # Find by extension
```
- `-type f` → Files only.
- `-size +100M` → Larger than 100MB.
- `-mtime -7` → Modified in last 7 days.
- `-name` → By name pattern.

---
## 🎯 What's Next?
Now that you understand File Management & VI Editor, move on to Part 4: Permissions & User Management.

Happy Learning! Keep Exploring Linux! 🐧

---
[< Previous](../part-02/README.md) ---- [Next >](../part-04/README.md)
