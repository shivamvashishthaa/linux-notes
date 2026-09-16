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
```

### 👤 Change Owner
```bash
chown user:user file.txt
chown -R user:user folder/   # Recursive
```
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
ps -ef                 # Detailed view
```
### 🔍 Real-time Processes
```bash
top                    # Live process viewer
htop                   # Better version (needs installation)
```
### ❌ Kill Process
```bash
kill <PID>             # Graceful kill
kill -9 <PID>          # Force kill (⚠️ May cause data loss)
pkill process_name     # Kill by name
```
### 🖥️ System Info
```bash
uname -a               # Kernel info
df -h                  # Disk usage (human-readable)
free -h                # RAM usage
uptime                 # System uptime
whoami                 # Current user
```
## 🎯 What's Next?
Now that you understand Permissions & Processes, move on to Part 3: Networking, Packages & Sudo.

Happy Learning! Keep Exploring Linux! 🐧
---
[< Previous](../part-01/README.md) ---- [Next >](../part-03/README.md)

