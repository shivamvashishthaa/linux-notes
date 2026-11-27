# Part 9: Linux Disk I/O – Input / Output Management & Troubleshooting

Production environments mein Disk I/O bottleneck ek common performance problem hai.

**Real-world scenario:**

Suppose MiniOrder ki Orders API suddenly slow ho gayi.

- CPU utilization normal hai.
- RAM available hai.
- PostgreSQL running hai.
- Lekin API response time 200 ms se 5 seconds ho gaya.

Possible reason: **Disk I/O bottleneck**.

PostgreSQL ko database files read/write karne ke liye disk access chahiye. Agar disk operations slow hain, toh database queries aur transaction commit latency bhi increase ho sakti hai.

---

### 1 Disk I/O Kya Hota Hai?

I/O ka full form Input/Output hai.
Disk I/O ka matlab storage device se data read aur write karna.

**Read Operation:**

- Disk se data RAM mein read karna.

**Write Operation:**

- Data ko storage device par write karna.

Example: Customer MiniOrder par order create karta hai.

### PostgreSQL Order Creation Flow

```text
Customer
   |
   v
Spring Boot Application
   |
   v
PostgreSQL
   |
   v
Transaction Processing
   |
   v
WAL (Write-Ahead Log)
   |
   v
Physical Storage
   |
   v
Transaction Commit
   |
   v
Spring Boot Response
```

Step-by-step:

1. Spring Boot request receive karta hai.
2. PostgreSQL INSERT query execute karta hai.
3. PostgreSQL transaction ke changes ko WAL mein record karta hai.
4. Commit ke waqt configured durability requirements ke according WAL disk par flush hota hai.
5. Transaction commit complete hoti hai.
6. Application response return karti hai.

**Important:** PostgreSQL mein WAL write aur data-page write alag operations hain. Normal transaction commit ke liye har modified data page ko immediately disk par write karna zaroori nahi hota. WAL durability mechanism important role play karta hai.

Agar storage slow hai, toh commit latency increase ho sakti hai.

---

### 2 Disk Utilization vs Disk Latency

Dono alag metrics hain.

| Metric | Meaning |
|---|---|
| IOPS | Per second kitne I/O operations complete ho rahe hain |
| Throughput | Per second kitna data transfer ho raha hai |
| Latency | Ek I/O operation complete hone mein kitna time lag raha hai |
| `%util` | Device busy time ka percentage |
| `await` | Average I/O request completion time, including queueing |
| `aqu-sz` | Average I/O queue size |

**Example:**

- Agar disk ka `await` bahut high hai, toh I/O requests complete hone mein zyada time lag raha hai.
- Agar `aqu-sz` high hai, toh requests queue mein wait kar rahi ho sakti hain.

**Important:** `%util = 100%` ko blindly bottleneck mat samajhna. SSD, NVMe aur parallel I/O devices par ye metric akela sufficient nahi hai.

Metrics ko device type, workload aur latency ke saath correlate karna chahiye.

---

### 3 Important Linux Disk Commands

**Command 1 – Disk Space**

```bash
df -h
```

Purpose:
- Filesystem ki total, used aur available space show karta hai.

Example:
```text
Filesystem      Size  Used  Avail  Use%
/dev/sda1        50G   35G    15G    70%
```

Important columns:
| Column | Meaning |
|---|---|
| Filesystem | Storage device/filesystem |
| Size | Total capacity |
| Used | Used space |
| Avail | Available space |
| Use% | Used percentage |
| Mounted on | Mount location |

**Command 2 – Directory Size**

```bash
du -sh /var/log
```

Purpose:
`/var/log` directory ka total disk usage show karta hai.

Useful variations:
```bash
du -sh /var/log/*
```

```bash
du -sh /home/*
```

**Command 3 – Disk Devices**
```bash
lsblk
```

Purpose:
- Available disks.
- Partitions.
- Disk hierarchy.
- Mount points.

Show karta hai.

**Command 4 – Disk I/O Statistics**
```bash
iostat -xz 1 5
```

Purpose:
Disk devices ki detailed I/O statistics show karta hai.

Arguments:
| Argument | Meaning |
|---|---|
| `-x` | Extended device statistics |
| `-z` | Zero-activity devices omit karta hai |
| `1` | 1-second interval |
| `5` | Five reports |

Important output metrics:
| Metric | Meaning |
|---|---|
| `r/s` | Read operations per second |
| `w/s` | Write operations per second |
| `rkB/s` | Read throughput |
| `wkB/s` | Write throughput |
| `r_await` | Average read request time |
| `w_await` | Average write request time |
| `aqu-sz` | Average queue size |
| `%util` | Device busy percentage |

Note: `iostat` ke liye `sysstat` package required hai. Agar command unavailable ho toh abhi install mat karna.

---

### 4 Production Disk I/O Troubleshooting

**Scenario: MiniOrder PostgreSQL Database Slow**

Observation:
- CPU utilization normal.
- RAM available.
- PostgreSQL running.
- Orders API response time increased.

Investigation:

**Step 1 – Disk Space Check**
```bash
df -h
```

Check:
- Filesystem capacity.
- Used space.
- Available space.
- Filesystem usage percentage.

**Step 2 – Directory Size Check**
```bash
du -sh /var/log
```

Purpose:

Check karna ki logs unnecessarily disk space consume toh nahi kar rahe.
**Step 3 – Disk Device Identification**

```bash
lsblk
```

Purpose:

Identify karna ki PostgreSQL data kis storage device par located hai.

**Step 4 – Disk I/O Statistics**
```bash
iostat -xz 1 5
```

Check:
- `await`
- `aqu-sz`
- `%util`
- Read/write throughput.

**Step 5 – Root Cause Investigation**

Metrics ko PostgreSQL monitoring aur application traces ke saath correlate karein.

Possible root causes:

| Observation | Possible Reason |
|---|---|
| High `await` | Disk requests slow |
| High queue depth | I/O requests waiting |
| High `%util` | Device heavily occupied; further investigation required |
| Disk 100% full | Writes fail ho sakti hain |
| Inodes 100% used | New files create nahi ho sakti |
| High write throughput | Heavy database writes or background workload |

---

### 5 Inode Troubleshooting

Disk space available hone ke bawajood new files create nahi ho rahi hain, toh inode exhaustion possible hai.

Check:

```bash
df -i
```

Important columns:

| Column | Meaning |
|---|---|
| Inodes | Total inodes |
| IUsed | Used inodes |
| IFree | Available inodes |
| IUse% | Inode utilization |

**Difference:**

- `df -h` filesystem storage capacity check karta hai.
- `df -i` inode availability check karta hai.

Agar inode usage 100% ho jaye, toh filesystem mein new files create nahi ho sakti, even when some disk space is available.

---

### 6 Disk I/O Troubleshooting Quick Reference

| Task | Command |
|---|---|
| Disk space | `df -h` |
| Inode usage | `df -i` |
| Directory size | `du -sh /var/log` |
| Directory-wise usage | `du -sh /var/log/*` |
| Disk devices | `lsblk` |
| Disk I/O statistics | `iostat -xz 1 5` |
| Disk usage by mount | `df -hT` |

---

### 7 Production Troubleshooting – Important Considerations

Senior developer ko sirf disk utilization dekhkar root cause conclude nahi karna chahiye.

In metrics ko correlate karna chahiye:

1. Application API latency.
2. PostgreSQL query execution time.
3. Transaction commit latency.
4. Disk read/write latency.
5. Disk queue depth.
6. Disk capacity and inode usage.
7. Database WAL activity.
8. Application distributed traces.

**Important:** Slow API ka matlab hamesha slow disk nahi hota. Query execution, locks, connection pool exhaustion aur network latency bhi investigate karni chahiye.

---

## Linux Production Troubleshooting – Combined Quick Reference

| Category | Command |
|---|---|
| Network IP | `ip -br -a` |
| Network connectivity | `ping <IP>` |
| SSH service | `sudo systemctl status ssh` |
| SSH logs | `sudo journalctl -u ssh -f` |
| CPU monitoring | `top` |
| CPU information | `lscpu` |
| CPU count | `nproc` |
| Load average | `uptime` |
| CPU-heavy processes | `ps aux --sort=-%cpu \| head -10` |
| Disk space | `df -h` |
| Inode usage | `df -i` |
| Directory size | `du -sh /var/log` |
| Disk devices | `lsblk` |
| Disk I/O | `iostat -xz 1 5` |

---

## Conclusion

Linux production troubleshooting mein CPU, RAM, Disk I/O aur Network ko collectively investigate karna important hai.

MiniOrder jaise production applications mein systematic approach follow karein:

1. Identify the incident.
2. Collect system metrics.
3. Identify the affected resource.
4. Correlate application and database metrics.
5. Find the root cause.
6. Apply corrective action.
7. Verify performance recovery.

---

[< Previous](../part-08/README.md) ---- [Next >](../part-10/README.md)
