# 🐧 Part 4: Permissions & User Management

Welcome to **Part 4** of the Ultimate Linux Guide. In this section, we will learn about file permissions and user management.

---

## 📑 Table of Contents
1. [File Permissions](#1-file-permissions)
2. [User Management](#2-user-management)
3. [Sudo & Root](#3-sudo--root)

---

## 1. File Permissions

### 🔐 Change File Permissions
```bash
chmod 755 file.sh
```
---

**Explanation of 755:**

- 7 = read (4) + write (2) + execute (1) → Owner
- 5 = read (4) + execute (1) → Group
- 5 = read (4) + execute (1) → Others

**Symbolic Mode:**
```bash
chmod +x file.sh       # Add execute permission
chmod -w file.txt      # Remove write permission
chmod u+x file.sh      # Add execute for user only
chmod g-w file.txt     # Remove write for group only
chmod o+r file.txt     # Add read for others only
chmod a+r file.txt     # Add read for all
```
- `u` → User (owner)
- `g` → Group
- `o` → Others
- `a` → All (`u+g+o`)

**Advanced Flags:**

- `-R` → Recursive (apply to all files in directory).
- `-v` → Verbose.

### Basic Command: ls -l
```bash
ls -l
```
**Output Example:**

```text
-rwxr-xr-- 1 shivam shivam 1024 Sep 17 10:30 file.sh
```
| **Part** | **Meaning** |
| :--- | :--- |
| **`-`** | File type (- = file, d = directory, l = link) |
| **`rwx`** | Owner permissions (read, write, execute) |
| **`r-x`** | Group permissions (read, execute) |
| **`r--`** | Others permissions (read only) |
| **`1`** | Number of hard links |
| **`shivam`** | Owner name |
| **`shivam`** | Group name |
| **`1024`** | File size (in bytes) |
| **`Sep 17 10:30`** | Last modified date/time |
| **`file.sh`** | File name |

### 👤 Change Owner
```bash
chown user:user file.txt
```

**Advanced:**

```bash
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


### 🔢 Special Permissions:
| Permission   | Symbol   | Description      |
| --- | --------- | ----------- |
| SUID  | `s` (user)  |  Run as owner |
| SGID  | `s` (group)  | Run as group |
| Sticky Bit  | `t`  |  Only owner can delete |

```bash
chmod u+s file.sh       # Set SUID
chmod g+s folder/       # Set SGID
chmod +t folder/        # Set Sticky Bit
```

---

## 2. User Management
### 👀 List of Users
```bash
getent passwd
# OR
cat /etc/passwd
```
Flags for `cat`:

- -n → Show line numbers.

### ➕ Add User
```bash
sudo adduser new_username
```
- Creates user, home directory, and prompts for password.

### ➕ Add User to Sudo Group
```bash
sudo usermod -aG sudo new_username
```
Flags Explained:

- `-a` → Append (add to group without removing from other groups).
- `-G` → Supplementary groups.
- ⚠️ Important: Always use -aG together. If you use only -G, the user will be removed from all other groups.

✅ Check if User is Added
```bash
id new_username
groups new_username
```
- id → Shows user ID, group ID, and groups.
- groups → Shows all groups the user belongs to.

### ❌ Delete User
```bash
sudo deluser new_username
```
**Advanced:**

```bash
sudo deluser --remove-home new_username   # Delete user + home directory
```
- --remove-home → Removes home directory and mail spool.

### 🔑 Change Password
```bash
sudo passwd username
```
- Prompts for new password.

### 📝 Modify User
```bash
sudo usermod -c "New Comment" username    # Change comment
sudo usermod -d /new/home username        # Change home directory
sudo usermod -s /bin/bash username        # Change shell
sudo usermod -L username                  # Lock user
sudo usermod -U username                  # Unlock user
```
- `-c` → Comment.
- `-d` → Home directory.
- `-s` → Shell.
- `-L` → Lock.
- `-U` → Unlock.

---

## 3. Sudo & Root
### 🔑 Sudo Command
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

### 📝 Edit Sudoers File (⚠️ Careful)
```bash
sudo visudo
``` 
Always use visudo instead of directly editing /etc/sudoers.

### 👤 Root User
```bash
whoami                 # Check current user
id                     # Check user ID (0 = root)
```
Root user has UID 0.

---

### Group
Groups are used for giving permissions to multiple users at once, without giving permissions to each user individually."

### 👥 1. Check Current User's Groups
```bash
groups
```
**Output:**

```text
shivam sudo docker
```
👉 This shows which groups you belong to.

### 👥 2. Check a Specific User's Groups
```bash
groups username
```
Example:

```bash
groups shivam
```
Output:

```text
shivam : shivam sudo docker
```
### 👥 3. Check Groups Using the id Command
```bash
id
```
Output:

```text
uid=1000(shivam) gid=1000(shivam) groups=1000(shivam),27(sudo),999(docker)
```
- `uid` → User ID
- `gid` → Primary Group ID
- `groups` → All groups (primary + secondary)

**For a specific user:**

```bash
id shivam
```
### 👥 4. Check All Groups on the System
```bash
cat /etc/group
```
Output:

```text
root:x:0:
sudo:x:27:shivam
docker:x:999:shivam
shivam:x:1000:
```
Format: group_name:password:GID:members

| **Part** | **Meaning** |
| :--- | :--- |
| **`group_name`** | Name of the group |
| **`x`** | Password (usually x means it's in the shadow file) |
| **`GID`** | Group ID |
| **`members`** | Members of this group (comma separated) |

**Modern command:**

```bash
getent group
```
👉 This shows groups from /etc/group as well as network groups.

### 👥 5. Check Members of a Specific Group
```bash
getent group sudo
```
Output:

```text
sudo:x:27:shivam
```
👉 This means only shivam is in the sudo group.

### 👥 6. Check Group ID (GID)
```bash
getent group 27
```
Output:

```text
sudo:x:27:shivam
```
### 👥 7. Check a User's Primary Group
```bash
id -gn username
```
Example:

```bash
id -gn shivam
```
Output:

```text
shivam
```
### 👥 8. Check All Group Names of a User (Names Only)
```bash
id -nG username
```
Example:

```bash
id -nG shivam
```
Output:

```text
shivam sudo docker
```

### 📊 Summary Table
| **What to Check?** | **Command** |
| :--- | :--- |
| **Current user's groups** | groups |
| **Specific user's groups** | groups username |
| **User ID + Group ID + Groups** | id |
| **Specific user's details	** | id	username |
| **All system groups** | cat /etc/group |
| **All groups (modern)** | getent group |
| **Members of a specific group** | 	id -gn username |
| **Primary group name** | groups |
| **All group names** | id -nG username |
  
### 🎯 Real-World Examples
Example 1: Check if a User is in the Sudo Group
```bash
groups shivam | grep sudo
```
Output:

```text
shivam : shivam sudo docker
```
👉 If sudo appears, the user has sudo access.

Example 2: Check All Members of the Docker Group
```bash
getent group docker
```
Output:

```text
docker:x:999:shivam
```
Example 3: User's Groups with Numeric IDs
```bash
id shivam
```
Output:

```text
uid=1000(shivam) gid=1000(shivam) groups=1000(shivam),27(sudo),999(docker)
```
💡 Quick Tip
If you want only group names (no IDs):

```bash
id -nG
```
If you want only the primary group:

```bash
id -gn
```
---

## 🎯 What's Next?
Now that you understand Permissions & User Management, move on to Part 5: Process Management & Monitoring.

Happy Learning! Keep Exploring Linux! 🐧

---

[< Previous](../part-03/README.md) ---- [Next >](../part-05/README.md)
