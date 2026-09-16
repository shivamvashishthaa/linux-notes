# 🐧 Part 6: Networking, Disk & Packages

Welcome to **Part 6** of the Ultimate Linux Guide. In this section, we will learn about networking, disk management, and package management.

---

## 📑 Table of Contents
1. [Networking](#1-networking)
2. [Disk & Storage Management](#2-disk--storage-management)
3. [Package Management](#3-package-management)
4. [Archiving & Compression](#4-archiving--compression)
5. [Curl (Data Transfer Tool)](#5-curl-data-transfer-tool)

---

## 1. Networking

### 🌐 Check IP Address
```bash
ip a                   # Modern command
ifconfig               # Legacy command (requires net-tools)
hostname -I            # Only IP address
```

- `a` → Show all interfaces.

### 🔌 SSH Connect
```bash
ssh user@IP
```
**Advanced:**

```bash
ssh -i key.pem user@IP    # With private key
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
ping -i 2 google.com     # Interval of 2 seconds
```
- `-c` → Count.
- `-i` → Interval.
- `-W` → Timeout.

### 🌍 DNS Lookup
```bash
nslookup google.com
dig google.com
```

Flags for `dig`:

- `+short` → Short output.
- `+trace` → Trace DNS resolution.

### 📥 File Transfer
```bash
scp file.txt user@IP:/path/     # Local to remote
scp user@IP:/path/file.txt .    # Remote to local
``` 
**Advanced:**

```bash
scp -r folder/ user@IP:/path/   # Recursive
scp -P 2222 file.txt user@IP:/  # Custom port
```
- `-r` → Recursive.
- `-P` → Port (uppercase).

### 🔥 Firewall (UFW)
```bash
sudo ufw status                 # Check firewall status
sudo ufw enable                 # Enable firewall
sudo ufw allow 22/tcp           # Allow SSH
sudo ufw deny 80/tcp            # Deny HTTP
sudo ufw delete allow 22/tcp    # Delete rule
```
## 2. Disk & Storage Management
### 💾 Check Disk Space
```bash
df -h
```
**Flags Explained:**

- `-h` → Human-readable (e.g., 1K, 2M, 1G).
- `-T` → Show filesystem type.
- `-i` → Show inode usage.

**Other Commands:**

```bash
du -sh folder/          # Folder size
du -h --max-depth=1     # Top-level folder sizes
```
### 🔍 Find Large Files
```bash
find / -type f -size +100M 2>/dev/null
```
**Flags Explained:**

- `-type f` → Files only.
- `-size +100M` → Larger than 100MB.
- `2>/dev/null` → Suppress permission errors.

### 🔗 Mount / Unmount
```bash
mount /dev/sdb1 /mnt
umount /mnt
```
**Flags:**

- `-t` → Filesystem type (e.g., `ext4`, `ntfs`).
- `-o` → Mount options (e.g., `ro` for read-only).

### 💿 Disk Partitioning
```bash
fdisk -l               # List all disks and partitions
fdisk /dev/sdb         # Partition a disk
mkfs.ext4 /dev/sdb1    # Format partition
```
### 📊 Disk Usage Analysis
```bash
ncdu                   # NCurses Disk Usage (interactive)
```
- Needs installation: sudo apt install ncdu

## 3. Package Management (Ubuntu)
## 📦 Install Package
```bash
sudo apt install package_name
```
**Advanced:**

```bash
sudo apt install -y package_name    # Auto-confirm
sudo apt install --no-install-recommends package_name  # Skip recommended
```
- `-y` → Yes to all prompts.
- `--no-install-recommends` → Only main package.

### 🔄 Update
```bash
sudo apt update         # Update package list
sudo apt upgrade        # Upgrade installed packages
sudo apt full-upgrade   # Upgrade including kernel
```
**Flags:**

- `-y` → Auto-confirm.
- `--fix-missing` → Fix missing packages.

### ❌ Remove Package
```bash
sudo apt remove package_name
```
**Advanced:**

```bash
sudo apt purge package_name      # Delete config files too
sudo apt autoremove              # Remove unused dependencies
```
- `purge` → Removes package + config files.
- `autoremove` → Removes unused dependencies.

### 🔍 Search Package
```bash
apt search package_name
apt list --installed             # List installed packages
apt list --upgradable            # List upgradable packages
📦 Dpkg (Debian Package Manager)
bash
dpkg -i package.deb             # Install .deb file
dpkg -l                         # List installed packages
dpkg -r package_name            # Remove package
dpkg -L package_name            # List files in package
```
## 4. Archiving & Compression
### 📦 Tar (Archive)
```bash
tar -cvf archive.tar folder/
```
**Flags Explained:**

- `-c` → Create a new archive.
- `-v` → Verbose.
- `-f` → Filename of the archive.

**Extract:**

```bash
tar -xvf archive.tar
```
- -x → Extract.

**With Gzip:**

```bash
tar -czvf archive.tar.gz folder/    # Create
tar -xzvf archive.tar.gz            # Extract
```
- `-z` → Compress with gzip.

**With Bzip2:**

```bash
tar -cjvf archive.tar.bz2 folder/   # Create
tar -xjvf archive.tar.bz2           # Extract
```
- `-j` → Compress with bzip2.

### 🗜️ Zip / Unzip
```bash
zip -r archive.zip folder/
unzip archive.zip
```
**Flags for zip:**

- `-r` → Recursive.
- `-9` → Maximum compression.

**Flags for unzip:**

- `-l` → List contents.
- `-d` /path/ → Extract to specific directory.

## 5. Curl (Data Transfer Tool)
Curl is a command-line tool used for transferring data over the internet.

### 🌐 Basic GET Request
```bash
curl https://api.github.com
```
### 📥 Download File
```bash
curl -O https://example.com/file.zip     # Save with same name
curl -o newname.zip https://example.com/file.zip  # Custom name
```
- `-O` → Save with same name.
- `-o` → Custom name.

### 📤 POST Request
```bash
curl -X POST -d "name=shivam" https://api.example.com/user
```
- `-X` → HTTP method.
- `-d` → Data to send.

### 📋 View Headers
```bash
curl -I https://google.com
```
- `-I` → Fetch headers only.

### 🔑 Send Headers
```bash
curl -H "Authorization: Bearer token" https://api.example.com
```
- `-H` → Custom header.

**Other useful flags:**

- `-L` → Follow redirects.
- `-k` → Allow insecure SSL.
- `-v` → Verbose.
- `-s` → Silent mode.

---

## 🎯 What's Next?
Now that you understand Networking, Disk & Packages, move on to Part 7: Mistakes, Interview Q&A & Bonus.

Happy Learning! Keep Exploring Linux! 🐧

---

[< Previous](../part-05/README.md) ---- [Next >](../part-07/README.md)

