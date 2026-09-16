# 🐧 Part 7: Real-World Mistakes, Interview Q&A & Bonus

Welcome to **Part 7** (the final part) of the Ultimate Linux Guide. In this section, we will cover common mistakes, interview questions, and bonus topics.

---

## 📑 Table of Contents
1. [Real-World Mistakes Beginners Make](#1-real-world-mistakes-beginners-make)
2. [Interview Questions & Answers](#2-interview-questions--answers)
3. [Bonus: Shell Scripting Basics](#3-bonus-shell-scripting-basics)
4. [Bonus: Systemd & Services](#4-bonus-systemd--services)

---

## 1. Real-World Mistakes Beginners Make

| Mistake | What Happens | Prevention |
| :--- | :--- | :--- |
| `rm -rf /` | 💀 **Destroys the system** | Never run `rm -rf /` |
| `rm -rf *` | Deletes all files | Always check with `ls` first |
| Wrong permissions | SSH will fail | Use `chmod 600` for SSH keys |
| Wrong user | Permission denied | Use `chown` to change owner |
| `chmod 777` | Security risk | Grant minimum permissions |
| `sudo` without reason | System damage | Use only when necessary |
| `mv` without `-i` | Overwrites files | Use `-i` for interactive |
| Forgetting `&` | Process blocks terminal | Use `&` for background |

### ⚠️ Golden Rule:
> **"With great power comes great responsibility."**
> Always use `sudo` and `rm -rf` with caution.

### 🛡️ Safety Tips:
1. **Always double-check** before running `rm -rf`.
2. **Use `-i` flag** with `rm`, `cp`, `mv` for interactive mode.
3. **Never use `chmod 777`** unless absolutely necessary.
4. **Use `visudo`** to edit sudoers file.
5. **Backup important files** before major changes.
6. **Test commands in a VM** before running on production.

---

## 2. Interview Questions & Answers

**Q1: How do you check file permissions in Linux?**
**A:** Using `ls -l`. The output shows permissions in `rwxr-xr-x` format.

**Q2: What does `chmod 755` mean?**
**A:** Owner gets read+write+execute (7), group gets read+execute (5), others get read+execute (5).

**Q3: What is the difference between a Hard Link and a Soft Link?**
**A:** A Hard link is a duplicate of the original file (same inode). A Soft link (symlink) is a shortcut to the original file.

**Q4: What is the difference between `ps aux` and `top`?**
**A:** `ps aux` gives a snapshot. `top` gives real-time updates.

**Q5: How do you generate an SSH key?**
**A:** `ssh-keygen -t rsa -b 4096`

**Q6: What is the difference between `df -h` and `du -sh`?**
**A:** `df -h` shows filesystem disk usage. `du -sh` shows the size of a specific folder.

**Q7: How do you add a user to the sudo group?**
**A:** `sudo usermod -aG sudo username`

**Q8: What is the `grep` command used for?**
**A:** To search for specific text inside a file.

**Q9: What is the difference between `kill` and `kill -9`?**
**A:** `kill` sends SIGTERM (graceful termination). `kill -9` sends SIGKILL (force kill, cannot be ignored).

**Q10: What is the purpose of the `-r` flag in `cp` and `rm`?**
**A:** `-r` means recursive, used to copy or delete directories and their contents.

**Q11: What is the difference between `apt` and `dpkg`?**
**A:** `apt` is a high-level package manager that handles dependencies. `dpkg` is a low-level tool that installs `.deb` files.

**Q12: What is a Cron Job?**
**A:** A scheduled task that runs automatically at specified intervals.

**Q13: What is the difference between `sudo su -` and `sudo -i`?**
**A:** Both switch to root, but `su -` simulates a full login shell, while `-i` runs an interactive login shell.

**Q14: What is the difference between a process and a thread?**
**A:** A process is an independent program with its own memory. A thread is a lightweight unit within a process that shares memory.

**Q15: What is the purpose of `/etc/fstab`?**
**A:** It contains information about filesystems and their mount points, used at boot time.

---

## 3. Bonus: Shell Scripting Basics

### 📝 What is a Shell Script?
A shell script is a file containing a series of commands that are executed by the shell.

### 🚀 Basic Script:
```bash
#!/bin/bash
# This is a comment
echo "Hello, World!"
```
### 📋 Variables:
```bash
NAME="Shivam"
echo "Hello, $NAME"
```
### 🔄 Loops:
```bash
# For loop
for i in {1..5}; do
  echo "Number: $i"
done

# While loop
count=1
while [ $count -le 5 ]; do
  echo "Count: $count"
  ((count++))
done
```
### 🔀 Conditionals:
```bash
if [ $1 -gt 10 ]; then
  echo "Greater than 10"
else
  echo "Less than or equal to 10"
fi
```
### 📂 Functions:
```bash
greet() {
  echo "Hello, $1"
}
greet "Shivam"
```
### 🏃 Running a Script:
```bash
chmod +x script.sh
./script.sh
```
## 4. Bonus: Systemd & Services
### 🔧 What is Systemd?
Systemd is the init system and service manager for Linux. It starts services at boot and manages them.

### 📋 Common Systemctl Commands:
```bash
systemctl start service_name      # Start a service
systemctl stop service_name       # Stop a service
systemctl restart service_name    # Restart a service
systemctl status service_name     # Check status
systemctl enable service_name     # Enable at boot
systemctl disable service_name    # Disable at boot
systemctl list-units --type=service  # List all services
```
- `start` → Start.
- `stop` → Stop.
- `restart` → Restart.
- `status` → Check status.
- `enable` → Enable at boot.
- `disable` → Disable at boot.

### 📝 Creating a Custom Service:
1. Create a file: `/etc/systemd/system/myapp.service`
2. Add the following content:

```ini
[Unit]
Description=My App
After=network.target

[Service]
ExecStart=/usr/bin/myapp
Restart=always

[Install]
WantedBy=multi-user.target
```

3. Reload systemd and start:

```bash
sudo systemctl daemon-reload
sudo systemctl start myapp
sudo systemctl enable myapp
```
---
## 🎉 Congratulations!
You have completed all 7 parts of the Ultimate Linux Guide. You now have a solid understanding of Linux from basics to advanced concepts.

---

## 📚 Complete Series:
[Part 1: Linux Fundamentals & Setup](../part-01/README.md)

[Part 2: Folder Structure & Navigation](../part-02/README.md)

[Part 3: File Management & VI Editor](../part-03/README.md)

[Part 4: Permissions & User Management](../part-04/README.md)

[Part 5: Process Management & Monitoring](../part-05/README.md)

[Part 6: Networking, Disk & Packages](../part-06/README.md)

[Part 7: Mistakes, Interview Q&A & Bonus](../part-07/README.md)

[CheatSheet](../CheatSheet/README.md)

---

Happy Learning! Keep Exploring Linux! 🐧