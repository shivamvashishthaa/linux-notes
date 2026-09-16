

## Complete Linux Commands Cheatsheet

This is a quick reference guide for all the essential Linux commands covered in this Ultimate Linux Guide.

---

### 📂 Navigation & File System
| Command | Description |
| :--- | :--- |
| `pwd` | Show current directory |
| `ls` | List files/folders |
| `ls -la` | List all files (including hidden) with details |
| `cd folder` | Change directory |
| `cd ..` | Go one level up |
| `cd ~` | Go to home directory |
| `cd -` | Go to previous directory |
| `tree` | Show directory structure in tree format |

### 📄 File Operations
| Command | Description |
| :--- | :--- |
| `touch file.txt` | Create an empty file |
| `mkdir folder` | Create a folder |
| `mkdir -p a/b/c` | Create nested folders |
| `rm file.txt` | Delete a file |
| `rm -r folder` | Delete a folder recursively |
| `rm -rf folder` | Force delete a folder (⚠️ No confirmation) |
| `cp file1 file2` | Copy file |
| `cp -r folder1 folder2` | Copy folder recursively |
| `mv old new` | Rename/Move file |
| `mv file /path/` | Move file to another directory |

### 📖 Viewing & Editing Files
| Command | Description |
| :--- | :--- |
| `cat file.txt` | View entire file |
| `cat -n file.txt` | View with line numbers |
| `head -n 10 file.txt` | View first 10 lines |
| `tail -n 10 file.txt` | View last 10 lines |
| `tail -f log.txt` | Follow live logs |
| `less file.txt` | Scroll view file |
| `nano file.txt` | Edit file (beginner-friendly) |
| `vim file.txt` | Edit file (advanced) |
| `grep "word" file.txt` | Search for a word in file |
| `grep -r "word" .` | Recursive search in directory |
| `grep -i "word" file.txt` | Case-insensitive search |
| `grep -n "word" file.txt` | Search with line numbers |
| `grep -v "word" file.txt` | Invert match (show non-matching lines) |
| `grep -c "word" file.txt` | Count matches |
| `find / -name "file.txt"` | Find file by name |
| `find / -type f -size +100M` | Find files larger than 100MB |
| `find / -mtime -7` | Find files modified in last 7 days |

### 🔐 Permissions
| Command | Description |
| :--- | :--- |
| `chmod 755 file.sh` | Set permissions (rwxr-xr-x) |
| `chmod +x file.sh` | Add execute permission |
| `chmod -w file.txt` | Remove write permission |
| `chmod u+x file.sh` | Add execute for user only |
| `chmod g-w file.txt` | Remove write for group only |
| `chmod o+r file.txt` | Add read for others only |
| `chmod a+r file.txt` | Add read for all |
| `chmod -R 755 folder/` | Recursive permission change |
| `chown user:group file.txt` | Change owner and group |
| `chown -R user:group folder/` | Recursive owner change |
| `chmod u+s file.sh` | Set SUID |
| `chmod g+s folder/` | Set SGID |
| `chmod +t folder/` | Set Sticky Bit |

### 👤 User Management
| Command | Description |
| :--- | :--- |
| `cat /etc/passwd` | List all users |
| `getent passwd` | List all users (modern) |
| `sudo adduser new_user` | Add a new user |
| `sudo usermod -aG sudo user` | Add user to sudo group |
| `id username` | Check user ID and groups |
| `groups username` | Check user's groups |
| `sudo deluser user` | Delete user |
| `sudo deluser --remove-home user` | Delete user with home directory |
| `sudo passwd username` | Change user password |
| `sudo usermod -L user` | Lock user |
| `sudo usermod -U user` | Unlock user |
| `whoami` | Show current user |
| `id` | Show current user ID |

### ⚙️ Process Management
| Command | Description |
| :--- | :--- |
| `ps aux` | List all running processes |
| `ps -ef` | List all processes (full format) |
| `top` | Real-time process viewer |
| `htop` | Better process viewer |
| `kill <PID>` | Graceful kill |
| `kill -9 <PID>` | Force kill |
| `pkill name` | Kill by name |
| `killall name` | Kill all instances by name |
| `jobs` | List background jobs |
| `fg %1` | Bring job to foreground |
| `bg %1` | Send job to background |
| `command &` | Run command in background |

### 📊 System Monitoring
| Command | Description |
| :--- | :--- |
| `df -h` | Disk usage (human-readable) |
| `df -T` | Disk usage with filesystem type |
| `du -sh folder/` | Folder size |
| `du -h --max-depth=1` | Top-level folder sizes |
| `free -h` | RAM usage |
| `uptime` | System uptime and load average |
| `vmstat 1 5` | Virtual memory stats |
| `iostat` | I/O statistics |
| `sar` | System activity reporter |
| `uname -a` | All kernel info |
| `uname -r` | Kernel version |
| `hostname` | System hostname |
| `lscpu` | CPU information |
| `lsblk` | Block devices (disks) |
| `lspci` | PCI devices |
| `lsusb` | USB devices |

### 🌐 Networking
| Command | Description |
| :--- | :--- |
| `ip a` | Show all network interfaces |
| `ifconfig` | Legacy network command |
| `hostname -I` | Show IP address |
| `ping google.com` | Ping a host |
| `ping -c 4 google.com` | Ping 4 times |
| `nslookup google.com` | DNS lookup |
| `dig google.com` | DNS lookup (detailed) |
| `ssh user@IP` | SSH connect |
| `ssh -i key.pem user@IP` | SSH with private key |
| `ssh -p 2222 user@IP` | SSH with custom port |
| `scp file.txt user@IP:/path/` | Copy file to remote |
| `scp user@IP:/path/file.txt .` | Copy file from remote |
| `scp -r folder/ user@IP:/path/` | Copy folder recursively |
| `netstat -tuln` | List listening ports |
| `ss -tuln` | Modern alternative to netstat |
| `sudo ufw status` | Check firewall status |
| `sudo ufw enable` | Enable firewall |
| `sudo ufw allow 22/tcp` | Allow SSH |

### 💾 Disk & Storage
| Command | Description |
| :--- | :--- |
| `fdisk -l` | List all disks and partitions |
| `fdisk /dev/sdb` | Partition a disk |
| `mkfs.ext4 /dev/sdb1` | Format partition |
| `mount /dev/sdb1 /mnt` | Mount a partition |
| `umount /mnt` | Unmount a partition |
| `ncdu` | Interactive disk usage analyzer |

### 📦 Package Management (Ubuntu)
| Command | Description |
| :--- | :--- |
| `sudo apt install package` | Install package |
| `sudo apt install -y package` | Install with auto-confirm |
| `sudo apt update` | Update package list |
| `sudo apt upgrade` | Upgrade installed packages |
| `sudo apt full-upgrade` | Upgrade including kernel |
| `sudo apt remove package` | Remove package |
| `sudo apt purge package` | Remove package + config |
| `sudo apt autoremove` | Remove unused dependencies |
| `apt search package` | Search for package |
| `apt list --installed` | List installed packages |
| `dpkg -i package.deb` | Install .deb file |
| `dpkg -l` | List installed packages |
| `dpkg -r package` | Remove package |

### 📦 Archiving & Compression
| Command | Description |
| :--- | :--- |
| `tar -cvf archive.tar folder/` | Create tar archive |
| `tar -xvf archive.tar` | Extract tar archive |
| `tar -czvf archive.tar.gz folder/` | Create gzip tar |
| `tar -xzvf archive.tar.gz` | Extract gzip tar |
| `tar -cjvf archive.tar.bz2 folder/` | Create bzip2 tar |
| `tar -xjvf archive.tar.bz2` | Extract bzip2 tar |
| `zip -r archive.zip folder/` | Create zip |
| `unzip archive.zip` | Extract zip |
| `unzip -l archive.zip` | List zip contents |

### 🌐 Curl
| Command | Description |
| :--- | :--- |
| `curl https://api.com` | GET request |
| `curl -O https://file.zip` | Download with same name |
| `curl -o new.zip https://file.zip` | Download with custom name |
| `curl -X POST -d "data" https://api.com` | POST request |
| `curl -I https://google.com` | Fetch headers only |
| `curl -H "Auth: token" https://api.com` | Send custom header |
| `curl -L https://short.url` | Follow redirects |
| `curl -k https://self-signed.com` | Allow insecure SSL |
| `curl -v https://api.com` | Verbose output |
| `curl -s https://api.com` | Silent mode |

### ⏰ Cron Jobs
| Command | Description |
| :--- | :--- |
| `crontab -e` | Edit cron jobs |
| `crontab -l` | List cron jobs |
| `crontab -r` | Remove all cron jobs |

**Cron Format:**
```text
command
| | | | |
| | | | └── Day of week (0-7)
| | | └──── Month (1-12)
| | └────── Day of month (1-31)
| └──────── Hour (0-23)
└────────── Minute (0-59)
```


### 🔧 Systemd & Services
| Command | Description |
| :--- | :--- |
| `systemctl start service` | Start a service |
| `systemctl stop service` | Stop a service |
| `systemctl restart service` | Restart a service |
| `systemctl status service` | Check status |
| `systemctl enable service` | Enable at boot |
| `systemctl disable service` | Disable at boot |
| `systemctl list-units --type=service` | List all services |
| `sudo systemctl daemon-reload` | Reload systemd config |

### 📝 VI Editor Shortcuts
| Command | Action |
| :--- | :--- |
| `i` | Insert before cursor |
| `I` | Insert at beginning of line |
| `a` | Append after cursor |
| `A` | Append at end of line |
| `o` | Open new line below |
| `O` | Open new line above |
| `Esc` | Return to Normal mode |
| `h/j/k/l` | Move left/down/up/right |
| `w` | Next word |
| `b` | Previous word |
| `0` | Beginning of line |
| `$` | End of line |
| `gg` | First line |
| `G` | Last line |
| `:n` | Go to line `n` |
| `x` | Delete character |
| `dd` | Delete line |
| `yy` | Copy line |
| `p` | Paste below |
| `P` | Paste above |
| `u` | Undo |
| `Ctrl + r` | Redo |
| `:w` | Save |
| `:q` | Quit |
| `:wq` | Save and quit |
| `:q!` | Quit without saving |
| `/word` | Search forward |
| `?word` | Search backward |
| `n` | Next match |
| `N` | Previous match |
| `:%s/old/new/g` | Replace all |
| `:%s/old/new/gc` | Replace with confirmation |

---

## 🎉 You're Now a Linux Pro!

This cheatsheet covers **everything** you need for daily Linux usage, DevOps interviews, and system administration.

**Bookmark this page** for quick reference! ⭐

---

*Happy Learning! Keep Exploring Linux! 🐧*