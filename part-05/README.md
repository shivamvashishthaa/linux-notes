# 🐧 Part 5: Real-World Mistakes & Interview Q&A

Welcome to **Part 5** (the final part) of the Linux Complete Guide. In this section, we will cover common mistakes beginners make and prepare for Linux interview questions.

---

## 📑 Table of Contents
1. [Real-World Mistakes Beginners Make](#1-real-world-mistakes-beginners-make)
2. [Bonus: Interview Questions](#2-bonus-interview-questions)

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

### ⚠️ Golden Rule:
> **"With great power comes great responsibility."**
> Always use `sudo` and `rm -rf` with caution.

---

## 2. Bonus: Interview Questions

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

---

## 🎉 Congratulations!

You have completed all 5 parts of the Linux Complete Guide. You now have a solid understanding of Linux from basics to advanced concepts.

### 📚 Complete Series:
*   [Part 1: Linux Basics](../part-01/README.md)
*   [Part 2: Permissions, Processes & System Info](../part-02/README.md)
*   [Part 3: Networking, Packages & Sudo](../part-03/README.md)
*   [Part 4: User Management, Disk, Archive & Curl](../part-04/README.md)
*   [Part 5: Real-World Mistakes & Interview Q&A](../part-05/README.md)

---

*Happy Learning! Keep Exploring Linux! 🐧*