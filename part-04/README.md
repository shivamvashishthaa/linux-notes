# 🐧 Part 4: User Management, Disk, Archive & Curl

Welcome to **Part 4** of the Linux Complete Guide. In this section, we will learn how to manage users, check disk usage, archive files, and use curl for data transfer.

---

## 📑 Table of Contents
1. [User Management](#1-user-management)
2. [Disk Usage & Storage](#2-disk-usage--storage)
3. [Archiving & Compression](#3-archiving--compression)
4. [Curl (Data Transfer Tool)](#4-curl-data-transfer-tool)

---

## 1. User Management

### 👀 List of Users
```bash
getent passwd
# OR
cat /etc/passwd
```

### ➕ Add User
```bash
sudo adduser new_username
```
### ➕ Add User to Sudo Group
```bash
sudo usermod -aG sudo new_username
```
### ✅ Check if User is Added
```bash
id new_username
groups new_username
```
### ❌ Delete User
bash
sudo deluser new_username
```
#### ❌ Delete User with Data
```bash
sudo deluser --remove-home new_username
```
### 🔑 Change Password
```bash
sudo passwd username
```
## 2. Disk Usage & Storage
### 💾 Check Disk Space
```bash
df -h                  # Filesystem usage
du -sh folder/         # Folder size
du -h --max-depth=1    # Top-level folder sizes
```
### 🔍 Find Large Files
```bash
find / -type f -size +100M 2>/dev/null
```
### 🔗 Mount / Unmount
```bash
mount /dev/sdb1 /mnt
umount /mnt
```
## 3. Archiving & Compression
### 📦 Tar (Archive)
```bash
tar -cvf archive.tar folder/        # Create tar
tar -xvf archive.tar                # Extract tar
tar -czvf archive.tar.gz folder/    # Create gzip tar
tar -xzvf archive.tar.gz            # Extract gzip tar
```
### 🗜️ Zip / Unzip
```bash
zip -r archive.zip folder/
unzip archive.zip
```
## 4. Curl (Data Transfer Tool)
Curl is a command-line tool used for transferring data over the internet. You can use it to call APIs, download files, fetch data from websites, etc.

### 🌐 Basic GET Request
```bash
curl https://api.github.com
```
### 📥 Download File
```bash
curl -O https://example.com/file.zip
curl -o newname.zip https://example.com/file.zip
```
### 📤 POST Request
```bash
curl -X POST -d "name=shivam" https://api.example.com/user
```
### 📋 View Headers
```bash
curl -I https://google.com
```
### 🔑 Send Headers
```bash
curl -H "Authorization: Bearer token" https://api.example.com
```
## 🎯 What's Next?
Now that you understand User Management, move on to Part 5: Real-World Mistakes & Interview Q&A.

Happy Learning! Keep Exploring Linux! 🐧

---

[< Previous](../part-03/README.md) ---- [Next >](../part-05/README.md)
