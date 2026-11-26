# 🐧 Part 5: Process Management & Monitoring

Welcome to **Part 5** of the Ultimate Linux Guide. In this section, we will learn how to manage processes and monitor system performance.

---

## 📑 Table of Contents
1. [Process Management](#1-process-management)
2. [System Monitoring](#2-system-monitoring)
3. [System Info Commands](#3-system-info-commands)

---

## 1. Process Management

### 🧠 Running Processes
```bash
ps aux
```
```bash
ps aux --sort=-%cpu
```
**Flags Explained:**

- `a` → Show processes for all users.
- `u` → Display user-oriented format.
- `x` → Show processes not attached to a terminal.

**Alternative:**

```bash
ps -ef
```
- `-e` → Show all processes.
- `-f` → Full-format listing.

### Process details inspect karna
Ab PID 7223 ko detail mein inspect karenge.

```bash
ps -p 7223 -o pid,ppid,user,stat,%cpu,%mem,etime,cmd
```

**Command breakdown:**
- -p 7223 — particular PID select karo.
- -o — custom output columns.
- pid — process ID.
- ppid — parent process ID.
- user — process owner.
- stat — process state.
- %cpu — CPU utilization.
- %mem — memory utilization.
- etime — process kitne samay se exist kar raha hai.
- cmd — command details.

**output:**
```text
PID    PPID   USER      STAT  %CPU  %MEM  ELAPSED     CMD
7223   7083   shivams+  Sl    3.2   2.8   02:29:08   chrome --type=renderer
```

### 🔍 Real-time Processes
```bash
top
```
**Useful shortcuts inside top:**

- `q` → Quit
- `k` → Kill a process
- `M` → Sort by memory usage
- `P` → Sort by CPU usage
- `1` → Show individual CPU cores

**Better Alternative:**

```bash
htop
```
- Needs installation: sudo apt install htop
- Colorful, interactive, and easier to use.

### ❌ Kill Process
```bash
kill <PID>
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

### 🔄 Background & Foreground
```bash
command &              # Run in background
jobs                   # List background jobs
fg %1                  # Bring job 1 to foreground
bg %1                  # Send job 1 to background
```
- `&` → Run in background.
- `jobs` → List jobs.
- `fg` → Foreground.
- `bg` → Background.

### ⏰ Scheduled Tasks (Cron)
```bash
crontab -e             # Edit cron jobs
crontab -l             # List cron jobs
crontab -r             # Remove all cron jobs
```

**Cron Format:**

```text
* * * * * command
| | | | |
| | | | └── Day of week (0-7)
| | | └──── Month (1-12)
| | └────── Day of month (1-31)
| └──────── Hour (0-23)
└────────── Minute (0-59)
```
## 2. System Monitoring
### 📊 CPU & Memory Monitoring
```bash
top                    # Live process viewer
htop                   # Better version
vmstat 1 5             # Virtual memory stats (1s interval, 5 times)
iostat                 # I/O statistics
sar                    # System activity reporter
```
- `vmstat` → Virtual memory statistics.
- `iostat` → CPU and I/O statistics.
- `sar` → Collect and report system activity.

### 💾 Disk Monitoring
```bash
df -h                  # Disk usage (human-readable)
du -sh folder/         # Folder size
du -h --max-depth=1    # Top-level folder sizes
iostat -d 2 5          # Disk I/O stats
```
- `-h` → Human-readable.
- `-s` → Summary.
- `--max-depth=1` → Top-level only.

### 🌐 Network Monitoring
```bash
netstat -tuln          # List listening ports
ss -tuln               # Modern alternative to netstat
iftop                  # Real-time network bandwidth
nload                  # Network load monitor
```
- `-t` → TCP.
- `-u` → UDP.
- `-l` → Listening.
- `-n` → Numeric (don't resolve names).

### 📈 Load Average
```bash
uptime                 # Shows load average (1, 5, 15 min)
```
- Load average > number of CPU cores = system is overloaded.

## 3. System Info Commands
``` bash
uname -a               # All kernel info
uname -r               # Kernel version
hostname               # System hostname
hostnamectl            # Detailed hostname info
whoami                 # Current user
id                     # User ID and groups
uptime                 # System uptime
free -h                # RAM usage
lscpu                  # CPU information
lsblk                  # Block devices (disks)
lspci                  # PCI devices
lsusb                  # USB devices
```

**Flags:**

- -a → All information.
- -r → Kernel release.
- -h → Human-readable.

---

## 4. Linux Services & systemd
Ab tak humne individual processes dekhe. Lekin production server par Spring Boot, PostgreSQL, Nginx jaise applications ko manually terminal se start nahi karte.

Unhe generally Linux Service Manager – systemd manage karta hai.



systemctl kya hai?
systemctl systemd service manager se interact karne ka command-line tool hai.

| **Command** | **Purpose** |
 | :--- | :--- |
 |systemctl status SERVICE	 | Service ki current condition |
 |systemctl start SERVICE	 | Service start |
 |systemctl stop SERVICE	 | Service stop |
 |systemctl restart SERVICE	 |Restart|
 |systemctl enable SERVICE	 | Boot par automatic start |
 |systemctl disable SERVICE	 | Boot par automatic start band |
 |systemctl reload SERVICE	 | Configuration reload, if supported |

---

## Production troubleshooting commands
Ye commands regularly use hongi:



Sirf failed services:
```bash
systemctl --failed
```

Currently active services:
```bash
systemctl list-units --type=service --state=running
```

Boot par enabled services:
```bash
systemctl list-unit-files --type=service --state=enabled
```

Service logs:
```bash
sudo journalctl -u ssh
```

Latest 50 log entries:
```bash
sudo journalctl -u ssh -n 50
```

Live logs:
```bash
sudo journalctl -u ssh -f
```

`-f` ka meaning follow hai: naye log entries aate hi display honge. Exit ke liye Ctrl+C.

---

## 🎯 What's Next?
Now that you understand Process Management & Monitoring, move on to Part 6: Networking, Disk & Packages.


*Happy Learning! Keep Exploring Linux! 🐧*

---
[< Previous](../part-04/README.md) ---- [Next >](../part-06/README.md)
