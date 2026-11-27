# Part 8: Linux CPU Management & Troubleshooting

Production environments mein CPU bottleneck ek common performance problem hai.

**Real-world scenario:**

Suppose MiniOrder ki APIs suddenly slow ho gayi hain.

- Orders API response time 200 ms se 5 seconds ho gaya.
- Database healthy hai.
- RAM available hai.
- Lekin API response time continuously increase ho raha hai.

Humein investigate karna hoga ki CPU bottleneck toh nahi hai.

---

### 1 CPU Utilization Kya Hota Hai?

CPU (Central Processing Unit) application ke instructions execute karta hai.

Black server hardware configuration:

| Resource | Specification |
|---|---|
| Physical CPU Cores | 4 |
| Logical CPUs | 8 |
| CPU Threads | 8 |

OS ko 8 logical processors available hain.

CPU utilization batata hai ki CPU kitna busy hai.

Example:

- 20% CPU: Relatively low CPU usage.
- 50% CPU: Moderate CPU usage.
- 90% CPU: High CPU usage.
- 100% CPU: Available CPU capacity fully utilized (per logical CPU or aggregate reporting context).

**Important:** CPU utilization ko load average ke saath correlate karke investigate karna chahiye.

---

### 2 top Command

Command:

```bash
top
```

`top` ek real-time process monitoring tool hai.

Important fields:

| Field | Meaning |
|---|---|
| PID | Process ID |
| USER | Process owner |
| PR | Scheduling priority |
| NI | Nice value |
| VIRT | Virtual memory size |
| RES | Physical resident memory |
| S | Process state |
| %CPU | CPU utilization |
| %MEM | Memory utilization |
| TIME+ | Accumulated CPU time |
| COMMAND | Process name |

Useful interactive shortcuts:

| Key | Purpose |
|---|---|
| `q` | Exit |
| `P` | Sort by CPU usage |
| `M` | Sort by memory usage |
| `1` | Individual CPU utilization |
| `H` | Show threads |
| `k` | Send signal to a process |

Exit karne ke liye `q` press karein.

---

### 3 CPU Utilization Metrics

`top` ke upper section mein CPU breakdown show hota hai.

| Metric | Meaning |
|---|---|
| us | User-space CPU time |
| sy | Kernel/system CPU time |
| id | Idle CPU time |
| wa | I/O wait time |
| hi | Hardware interrupts |
| si | Software interrupts |
| st | CPU time stolen by hypervisor |

Example:

```text
%Cpu(s): 45.0 us, 10.0 sy, 35.0 id, 10.0 wa
```

Interpretation:

- 45% CPU user applications use kar rahi hain.
- 10% kernel activities.
- 35% CPU idle hai.
- 10% I/O wait hai.

**Important:** I/O wait CPU ka normal application execution nahi hai. Ye CPU idle time ka woh portion hai jab system I/O completion ka wait kar raha hota hai.

High I/O wait hone par storage, disk operations aur other I/O dependencies investigate karni chahiye.

---

### 4 Load Average – Important Concept

Command:

```bash
uptime
```

Example:

```text
12:30:00 up 2 days, 3 users, load average: 2.50, 3.00, 2.80
```

Three values represent:

- First: Last 1 minute.
- Second: Last 5 minutes.
- Third: Last 15 minutes.

Load average Linux mein runnable tasks aur uninterruptible sleep state (generally I/O wait) mein tasks ki average count represent karta hai.

Black server mein 8 logical CPUs hain.

| Load Average | Initial Interpretation |
|---|---|
| 2 | Generally low load relative to 8 CPUs |
| 8 | CPU capacity ke aas-paas demand |
| 12 | Potential CPU contention |
| 20 | Significant pressure possible |

Ye rough interpretation hai, absolute rule nahi.

**Important:** Load average 8 hone ka matlab CPU exactly 100% utilized hai, aisa zaroori nahi. I/O wait par blocked tasks bhi load average mein contribute karte hain.

Isliye load average ko CPU utilization aur I/O metrics ke saath analyze karna chahiye.

---

### 5 Production CPU Troubleshooting Workflow

**Scenario: MiniOrder API Latency Increased**

<text> </text>

```text
Incident:
Orders API response time increased

Expected Response Time: 200 ms
Actual Response Time:   5 seconds
```

Investigation steps:

**Step 1 – Overall CPU Utilization**

```bash
top
```

Check:

- CPU utilization.
- Idle percentage.
- I/O wait.
- Top CPU-consuming processes.

**Step 2 – CPU-Intensive Processes**

```bash
ps aux --sort=-%cpu | head -10
```

Purpose:

- Highest CPU-consuming processes identify karna.
- Java application CPU usage check karna.
- Unwanted background processes identify karna.

**Step 3 – Load Average**

```bash
uptime
```

Check:

- 1-minute load.
- 5-minute load.
- 15-minute load.

Isse pata chalega ki CPU pressure recent hai ya continuously exist kar raha hai.

**Step 4 – Java Thread-Level Investigation**

Java application mein high CPU usage identify karne ke liye thread-level investigation karni hogi.

Useful commands:

```bash
jps -lv
```

Java processes identify karta hai.

```bash
top -H -p <PID>
```

Java process ke individual threads ka CPU usage show karta hai.

```bash
jstack <PID>
```

Java thread dump generate karta hai.

Thread dump se investigate kar sakte hain:

- Infinite loops.
- Excessive computation.
- Thread contention.
- BLOCKED threads.
- Deadlocks.
- Excessive thread creation.

**Step 5 – Root Cause Analysis**

Possible root causes:

| Root Cause | Explanation |
|---|---|
| Infinite Loop | Thread continuously CPU consume kar raha hai |
| Excessive Computation | Heavy calculations CPU usage increase kar rahi hain |
| Lock Contention | Threads lock acquire karne ke liye wait kar rahe hain |
| Excessive Threads | Too many active threads scheduling overhead increase kar sakte hain |
| Background Jobs | Batch processing application CPU consume kar sakti hai |
| High Traffic | Incoming requests available CPU capacity se zyada hain |

**Step 6 – Resolution**

Root cause ke according:

- Infinite loop fix karein.
- Unnecessary computations optimize karein.
- Lock contention reduce karein.
- Thread pool size review karein.
- Background jobs schedule optimize karein.
- Application profiling karein.
- Zaroorat hone par CPU capacity scale karein.

**Production Note:** Thread dump ek snapshot hota hai. CPU-consuming thread identify karne ke liye multiple thread dumps aur CPU profiling ko correlate karna useful hai.

---

### 6 More Useful CPU Commands

**1. Logical CPU Count**

```bash
nproc
```

Purpose:

Available processing units ka count show karta hai.

Expected output on Black:

```text
8
```

**2. Detailed CPU Information**

```bash
lscpu
```

Purpose:

- Architecture.
- CPU model.
- Physical cores.
- Logical CPUs.
- Threads per core.
- CPU frequency.

**3. CPU Statistics**

```bash
mpstat -P ALL 1 5
```

Breakdown:

| Argument | Meaning |
|---|---|
| `-P ALL` | All logical CPUs show karega |
| `1` | 1-second interval |
| `5` | 5 reports generate karega |

Note: `mpstat` ke liye `sysstat` package installed hona chahiye. Abhi install karne ki zaroorat nahi.

**4. Process CPU Usage**

```bash
ps aux --sort=-%cpu | head -10
```

Highest CPU-consuming 10 processes show karega.

**5. CPU Usage Per Core**

```bash
top
```

Then press:

```text
1
```

Individual logical CPU utilization show hogi.

---

### 7 CPU Troubleshooting Quick Reference

| Task | Command |
|---|---|
| Real-time monitoring | `top` |
| Logical CPU count | `nproc` |
| CPU information | `lscpu` |
| Load average | `uptime` |
| Top CPU processes | `ps aux --sort=-%cpu \| head -10` |
| Per-core CPU statistics | `mpstat -P ALL 1 5` |
| Java process identification | `jps -lv` |
| Java thread CPU usage | `top -H -p <PID>` |
| Java thread dump | `jstack <PID>` |

---

## Conclusion

Linux production troubleshooting CPU management is an important skill hain.

CPU troubleshooting se application performance degradation, resource contention aur high CPU consumption investigate kar sakte hain.

---

MiniOrder jaise production applications mein systematic investigation approach follow karna chahiye:
1. Identify the incident.
2. Collect system metrics.
3. Identify the affected process.
4. Investigate application threads.
5. Find the root cause.
6. Apply corrective action.
7. Verify performance recovery.

---

[< Previous](../part-07/README.md) ---- [Next >](../part-09/README.md)

