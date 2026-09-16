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

- Creates user, home directory, and prompts for password.

### ➕ Add User to Sudo Group
```bash
sudo usermod -aG sudo new_username
```
**Flags Explained:**

- `-a` → Append (add to group without removing from other groups).

- `-G` → Supplementary groups.

- ⚠️ Important: Always use `-aG` together. If you use only `-G`, the user will be removed from all other groups.

### ✅ Check if User is Added
```bash
id new_username

groups new_username
```
- `id` → Shows user ID, group ID, and groups.
- `groups` → Shows all groups the user belongs to.

### ❌ Delete User
```bash
sudo deluser new_username

sudo deluser --remove-home new_username   # Delete user + home directory
```
- `--remove-home` → Removes home directory and mail spool.

#### ❌ Delete User with Data
```bash
sudo deluser --remove-home new_username
```
### 🔑 Change Password
```bash
sudo passwd username
```
- Prompts for new password.
## 2. Disk Usage & Storage
### 💾 Check Disk Space
```bash
df -h                  # Filesystem usage
```
**Flags Explained:**
- `-h` → Human-readable (e.g., 1K, 2M, 1G).
- `-T` → Show filesystem type.
- `-i` → Show inode usage instead of block usage.

```bash
du -sh folder/         # Folder size
```
- `-s` → Summary (total size only).

- `-h` → Human-readable.

```bash
du -h --max-depth=1    # Top-level folder sizes
```
- `--max-depth=1` → Show only top-level folder sizes.


### 🔍 Find Large Files
```bash
find / -type f -size +100M 2>/dev/null
```
**Flags Explained:**
- `/` → Start from root directory.
- `-type f` → Search for files only.
- `-size +100M` → Files larger than 100MB.
- `2>/dev/null` → Suppress permission errors.

**Other useful find flags:**
- `-name "*.log"` → Find by name pattern.
- `-mtime -7` → Files modified in last 7 days.
- `-exec command {} \;` → Execute command on each result.

### 🔗 Mount / Unmount
```bash
mount /dev/sdb1 /mnt
umount /mnt
```
**Flags:**

- `-t` → Filesystem type (e.g., ext4, ntfs).
- `-o` → Mount options (e.g., ro for read-only).


## 3. Archiving & Compression
### 📦 Tar (Archive)
```bash
tar -cvf archive.tar folder/        # Create tar
```
**Flags Explained:**

- `-c` → Create a new archive.
- `-v` → Verbose (show progress).
- `-f` → Filename of the archive.

**Extract:**
```bash
tar -xvf archive.tar                # Extract tar
```
- `-x` → Extract.

**With Gzip Compression:**
```bash
tar -czvf archive.tar.gz folder/    # Create gzip tar
tar -xzvf archive.tar.gz            # Extract gzip tar
```

- `-z` → Compress with gzip.

**With Bzip2 Compression:**

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

- `-r` → Recursive (for folders).
- `-9` → Maximum compression.

**Flags for unzip:**

- `-l` → List contents without extracting.
- `-d` /path/ → Extract to specific directory.


## 4. Curl (Data Transfer Tool)
Curl is a command-line tool used for transferring data over the internet. You can use it to call APIs, download files, fetch data from websites, etc.

### 🌐 Basic GET Request
```bash
curl https://api.github.com
```
### 📥 Download File
```bash
curl -O https://example.com/file.zip
```
- `-O` → Save with the same name as the remote file.
```bash
curl -o newname.zip https://example.com/file.zip
```
` `-o` → Save with a custom name.

### 📤 POST Request
```bash
curl -X POST -d "name=shivam" https://api.example.com/user
```
- `-X` → HTTP method (POST, PUT, DELETE, etc.).
- `-d` → Data to send.


### 📋 View Headers
```bash
curl -I https://google.com
```
- `-I` → Fetch headers only (HEAD request).

### 🔑 Send Headers
```bash
curl -H "Authorization: Bearer token" https://api.example.com
```
- `-H` → Custom header.

Other useful flags:
- `-L` → Follow redirects.
- `-k` → Allow insecure SSL connections.
- `-v` → Verbose (show full request/response).
- `-s` → Silent mode.

## 🎯 What's Next?
Now that you understand User Management, move on to Part 5: Real-World Mistakes & Interview Q&A.

Happy Learning! Keep Exploring Linux! 🐧

---

[< Previous](../part-03/README.md) ---- [Next >](../part-05/README.md)
