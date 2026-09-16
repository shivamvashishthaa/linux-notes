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
ifconfig               # Legacy command (requires net-tools)
hostname -I            # Only IP address
```

### 🔌 SSH Connect
```bash
ssh user@IP
ssh -i key.pem user@IP   # With private key
```
### 📡 Ping
```bash
ping google.com
ping -c 4 google.com     # Send only 4 packets
```
### 🌍 DNS Lookup
```bash
nslookup google.com
dig google.com
```
### 📥 File Transfer
```bash
scp file.txt user@IP:/path/     # Local to remote
scp user@IP:/path/file.txt .    # Remote to local
```
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
## 3. Sudo (Admin Power)
```bash
sudo command
```
👉 Used for admin privileges. To switch to root user:

```bash
sudo su -              # Switch to root user
sudo -i                # Root shell
```
### Edit Sudoers File (⚠️ Careful)
```bash
sudo visudo
```
### 🎯 What's Next?
Now that you understand Networking & Packages, move on to Part 4: User Management, Disk, Archive & Curl.

Happy Learning! Keep Exploring Linux! 🐧

---
[< Previous](../part-02/README.md) ---- [Next >](../part-04/README.md)
