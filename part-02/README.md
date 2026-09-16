# 🐧 Part 2: Permissions, Processes & System Info

Welcome to **Part 2** of the Linux Complete Guide. In this section, we will learn about file permissions, managing running processes, and checking system information.

---

## 📑 Table of Contents
1. [File Permissions](#1-file-permissions)
2. [System / Process Management](#2-system--process-management)

---

## 1. File Permissions

### 🔐 Change File Permissions
```bash
chmod 755 file.sh
```
- 7 = read (4) + write (2) + execute (1) → Owner
- 5 = read (4) + execute (1) → Group
- 5 = read (4) + execute (1) → Others

#### Symbolic Mode:

```bash
chmod +x file.sh       # Add execute permission
chmod -w file.txt      # Remove write permission
chmod u+x file.sh      # Add execute for user only
chmod g-w file.txt     # Remove write for group only
chmod o+r file.txt     # Add read for others only
```
- `u` → User (owner)
- `g` → Group
- `o` → Others
- `a` → All (u+g+o)

**Advanced Flags:**

- `-R` → Recursive (apply to all files in directory).
- `-v` → Verbose (shows what is being changed).

### 👤 Change Owner
```bash
chown user:user file.txt
chown -R user:user folder/   # Recursive
chown user file.txt           # Change only user
chown :group file.txt         # Change only group
```
- `-R` → Recursive.
- `-v` → Verbose.

### 📊 Permission Values (Memorize These)
| Number   | Permission   | Symbol      |
| --- | --------- | ----------- |
| 0  | No permission    |  `---` |
| 1  | Execute    |  `-x` |
| 2  | Write    |  `-w-` |
| 3  | Write + Execute  |  `-wx` |
| 4  | Read    |  `r--` |
| 5  | Read + Execute    |  `r-x` |
| 6  | Read + Write    |  `rw-` |
| 7  | Read + Write + Execute    |  `rwx` |

---

## 2. System / Process Management
### 🧠 Running Processes
```bash
ps aux                 # All running processes
```
**Flags Explained:**

- `a` → Show processes for all users.
- `u` → Display user-oriented format.
- `x` → Show processes not attached to a terminal.

**Alternative:**
```bash
ps -ef                 # Detailed view
```

- `-e` → Show all processes.
- `-f` → Full-format listing.

### 🔍 Real-time Processes
```bash
top                    # Live process viewer
```
Useful shortcuts inside top:

- `q` → Quit
- `k` → Kill a process
- `M` → Sort by memory usage
- `P` → Sort by CPU usage

**Better Alternative:**

```bash
htop                   # Better version (needs installation)
```
- Needs installation: sudo apt install htop
- Colorful, interactive, and easier to use.


### ❌ Kill Process
```bash
kill <PID>             # Graceful kill
```
**Advanced:**

```bash
kill -9 <PID>          # Force kill (SIGKILL)
kill -15 <PID>         # Graceful kill (SIGTERM)
pkill process_name     # Kill by name
killall process_name   # Kill all instances by name
```
**Signal Numbers:**
- `-9` → SIGKILL (force kill, cannot be ignored).
- `-15` → SIGTERM (graceful termination, default).
- `-1` → SIGHUP (hang up).


### 🖥️ System Info
```bash
uname -a               # Kernel info
df -h                  # Disk usage (human-readable)
free -h                # RAM usage
uptime                 # System uptime
whoami                 # Current user
```
- `-h` → Human-readable (e.g., 1K, 2M, 1G).

---

## 🎯 What's Next?
Now that you understand Permissions & Processes, move on to Part 3: Networking, Packages & Sudo.

Happy Learning! Keep Exploring Linux! 🐧
---
[< Previous](../part-01/README.md) ---- [Next >](../part-03/README.md)

