# 🐧 Part 3: Networking, Packages & Sudo

Welcome to **Part 3** of the Linux Complete Guide. In this section, we will cover networking commands, package management on Ubuntu, and using sudo for admin privileges.

---

## 📑 Table of Contents
1. [Networking (Important for SSH)](#1-networking-important-for-ssh)
2. [Package Management (Ubuntu)](#2-package-management-ubuntu)
3. [Sudo (Admin Power)](#3-sudo-admin-power)

---

## 1. Networking (Important for SSH)

### 🌐 Check IP Address
```bash
ip a                   # Modern command
```
- `a` → Show all interfaces (up and down).
```bash
ifconfig               # Legacy command (requires net-tools)
hostname -I            # Only IP address
```
- `-I` → Show all network addresses.

### 🔌 SSH Connect
```bash
ssh user@IP
```
**Advanced:**

```bash
ssh -i key.pem user@IP   # With private key
ssh -p 2222 user@IP       # Custom port
```
- `-i` → Identity file (private key).
- `-p` → Port number.

### 📡 Ping
```bash
ping google.com
```
**Advanced:**
```bash
ping -c 4 google.com     # Send only 4 packets
ping -i 2 google.com     # Interval of 2 seconds between packets
```
- `-c` → Count (number of packets).
- `-i` → Interval (time between packets).
- `-W` → Timeout for each reply.


### 🌍 DNS Lookup
```bash
nslookup google.com
dig google.com
```

Flags for `dig`:

- `+short` → Short output.
- `+trace` → Trace DNS resolution path.

### 📥 File Transfer
```bash
scp file.txt user@IP:/path/     # Local to remote
scp user@IP:/path/file.txt .    # Remote to local
```

**Advanced:**

```bash
scp -r folder/ user@IP:/path/   # Recursive (for folders)
scp -P 2222 file.txt user@IP:/  # Custom port
```
- `-r` → Recursive.
- `-P` → Port (uppercase).

## 2. Package Management (Ubuntu)
### 📦 Install Package
```bash
sudo apt install package_name
sudo apt install -y package_name    # Auto-confirm
```
### 🔄 Update
```bash
sudo apt update         # Update package list
sudo apt upgrade        # Upgrade installed packages
sudo apt full-upgrade   # Upgrade including kernel
```
### ❌ Remove Package
```bash
sudo apt remove package_name
sudo apt purge package_name      # Delete config files too
sudo apt autoremove              # Remove unused dependencies
```
### 🔍 Search Package
```bash
apt search package_name
apt list --installed             # List installed packages
```
- `--installed` → Show only installed packages.
- `--upgradable` → Show upgradable packages.

## 3. Sudo (Admin Power)
```bash
sudo command
```
👉 Used for admin privileges. To switch to root user:

```bash
sudo su -              # Switch to root user
sudo -i                # Root shell
```
- `-i` → Simulate initial login.
- `-s` → Run shell as root.


### Edit Sudoers File (⚠️ Careful)
```bash
sudo visudo
```
Always use `visudo` instead of directly editing `/etc/sudoers`.

### 🎯 What's Next?
Now that you understand Networking & Packages, move on to Part 4: User Management, Disk, Archive & Curl.

Happy Learning! Keep Exploring Linux! 🐧

---
[< Previous](../part-02/README.md) ---- [Next >](../part-04/README.md)
