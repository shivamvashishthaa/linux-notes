# 🐧 Part 1: Linux Fundamentals & Setup

Welcome to **Part 1** of the Ultimate Linux Guide. In this section, we will cover the fundamentals of Linux, its core components, comparison with Windows, and how to set up Linux on different platforms.

---

## 📑 Table of Contents
1. [What is Linux?](#1-what-is-linux)
2. [Linux vs Windows](#2-linux-vs-windows)
3. [Core Components of Linux](#3-core-components-of-linux)
4. [Linux Folder Structure (FHS)](#4-linux-folder-structure-fhs)
5. [Setup Linux on Windows & MacOS](#5-setup-linux-on-windows--macos)

---

## 1. What is Linux?

Linux is an open-source, Unix-like operating system kernel. It was created by **Linus Torvalds** in 1991. Today, Linux powers everything from servers and smartphones (Android) to supercomputers and embedded devices.

### Key Features:
- **Open Source:** Free to use, modify, and distribute.
- **Multi-user:** Multiple users can use the system simultaneously.
- **Multitasking:** Multiple processes can run at the same time.
- **Portable:** Runs on various hardware platforms.
- **Secure:** Strong permissions and user management.

---

## 2. Linux vs Windows

| Feature | Linux | Windows |
| :--- | :--- | :--- |
| **Source Code** | Open Source | Closed Source |
| **Cost** | Free | Paid (License) |
| **Security** | More Secure | Less Secure |
| **File System** | ext4, XFS, Btrfs | NTFS, FAT32 |
| **Case Sensitivity** | Case-sensitive | Case-insensitive |
| **Package Manager** | apt, yum, dnf | MSI, EXE |
| **Shell** | Bash, Zsh, Fish | PowerShell, CMD |
| **Use Case** | Servers, DevOps, Cloud | Desktop, Gaming |

---

## 3. Core Components of Linux

1. **Kernel:** The core of the OS. Manages hardware, memory, processes, and system calls.
2. **Shell:** Command-line interface (CLI) that interprets user commands. Examples: Bash, Zsh, Fish.
3. **File System:** Organizes and stores files. Examples: ext4, XFS, Btrfs.
4. **Process Management:** Handles creation, scheduling, and termination of processes.
5. **User Space:** Where user applications run.
6. **Init System:** First process (PID 1) that starts other services. Examples: systemd, SysVinit.

---

## 4. Linux Folder Structure (FHS)

The **Filesystem Hierarchy Standard (FHS)** defines the directory structure in Linux.

| Directory | Purpose |
| :--- | :--- |
| `/` | Root directory (everything starts here) |
| `/bin` | Essential user binaries (e.g., `ls`, `cp`) |
| `/sbin` | System binaries (e.g., `fdisk`, `iptables`) |
| `/etc` | Configuration files (e.g., `/etc/passwd`, `/etc/hosts`) |
| `/home` | User home directories (e.g., `/home/shivam`) |
| `/root` | Root user's home directory |
| `/var` | Variable data (logs, spool, cache) |
| `/tmp` | Temporary files (cleared on reboot) |
| `/usr` | User utilities and applications |
| `/opt` | Optional software packages |
| `/dev` | Device files (e.g., `/dev/sda`, `/dev/null`) |
| `/proc` | Process and kernel information (virtual filesystem) |
| `/sys` | System and hardware information |
| `/mnt` | Temporary mount point |
| `/media` | Removable media (USB, CD-ROM) |

---

## 5. Setup Linux on Windows & MacOS

### On Windows:
1. **WSL2 (Windows Subsystem for Linux):**
   ```bash
   wsl --install
   ```
    - Install Ubuntu from Microsoft Store.
    - Best for development.

2. **VirtualBox/VMware:**

    - Download Ubuntu ISO from ubuntu.com.
    - Create a VM and install.

3. **Dual Boot:**

- Install Linux alongside Windows.

### On MacOS:
1. **Docker:**

```bash
docker run -it ubuntu bash
```

2. **VirtualBox/VMware:**
    - Same as Windows.

3. **Cloud (AWS EC2):**
    - Launch an EC2 instance with Ubuntu AMI.
    - Connect via SSH.

---

## 🎯 What's Next?
Now that you understand Linux Fundamentals, move on to Part 2: Folder Structure & Navigation.


Happy Learning! Keep Exploring Linux! 🐧

---
[< Previous](../README.md) ---- [Next >](../part-02/README.md)