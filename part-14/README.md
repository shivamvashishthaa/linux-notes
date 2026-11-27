# Production Incident Investigation – End-to-End RCA

Ab tak humne individual Linux commands aur troubleshooting concepts seekhe. Lekin production mein engineer ke liye alag-alag commands chalane se zyada important hai unke results ko correlate karke actual root cause identify karna.

---

# 1 MiniOrder – Production Incident Simulation

<badge color="danger">P1 – Critical</badge>

**Incident ID:** MINI-101

**Problem:** Customers orders create nahi kar paa rahe hain.

### Initial Incident Status

| Component | Status |
|---|---|
| Linux server | Running |
| Spring Boot | Running |
| PostgreSQL | Running |
| CPU | 35% |
| Available RAM | 5 GB |
| Orders API | Timeout |
| Recent deployment | No |

**Initial observation:** Server aur application processes running hain, lekin Orders API timeout de rahi hai. Iska matlab process running hona application functionality healthy hone ki guarantee nahi hai.

---

# 2 Step-by-Step Investigation

## Step 1 – Application Process

```bash
ps aux | grep java
```

**Check:**
- Java process running hai?
- CPU consumption abnormal hai?
- Memory consumption kitni hai?
- Process multiple instances mein toh nahi chal raha?

Detailed process information:

```bash
ps -p <PID> -o pid,ppid,user,stat,%cpu,%mem,etime,cmd
```

Yahan `<PID>` ko actual Java process ID se replace karna hai.

## Step 2 – Application Logs

```bash
sudo journalctl -u miniorder -n 100 --no-pager
```

**Check:**
- Database connection exceptions?
- Timeout exceptions?
- Repeated errors?
- Application restart ya crash?
- Orders API ke request failures?

Live logs:

```bash
sudo journalctl -u miniorder -f
```

**Important:** `miniorder` service tabhi available hogi jab application ko systemd unit ke through configure kiya gaya ho. Agar systemd service configured nahi hai, toh application ke actual log destination ko inspect karna hoga.

## Step 3 – Database Port

```bash
sudo ss -tlnp | grep 5432
```

**Check:**
- PostgreSQL port 5432 par listen kar raha hai?
- Expected network interface par listener available hai?
- PostgreSQL process port bind kar pa raha hai?

**Important:** Port listening hone ka matlab database queries successfully execute ho rahi hain, aisa zaroori nahi. Database health aur query execution bhi verify karna hoga.

## Step 4 – Disk I/O

```bash
iostat -xz 1 5
```

**Important metrics:**

| Metric | Meaning |
|---|---|
| `await` | I/O request complete hone ka average time |
| `aqu-sz` | Average I/O queue size |
| `%util` | Device busy time percentage |

High I/O latency ya queue size database queries ko slow kar sakti hai.

**Note:** `iostat` sysstat package ka part hai. Agar command available nahi hai, toh abhi installation force karne ki zarurat nahi. `%util` ko akela bottleneck ka proof bhi nahi maanna chahiye, especially SSD/NVMe devices par.

## Step 5 – Memory

```bash
free -h
```

**Check:**
- Available RAM
- Swap consumption
- Memory pressure

`available` memory ko specially observe karna chahiye. Linux filesystem cache ke kaaran `used` memory high dikh sakti hai, jo automatically memory shortage nahi hoti.

## Step 6 – TCP Connections

```bash
ss -tan state established
```

**Check:**
- Active TCP connections kitne hain?
- Application aur PostgreSQL ke beech connections establish ho rahe hain?
- Unexpected connection growth toh nahi ho rahi?

PostgreSQL port-specific connections:

```bash
sudo ss -tanp | grep ':5432'
```

**Important:** Established TCP connection ka matlab database query healthy hai, ye guarantee nahi hai. SQL execution, locks aur connection pool metrics bhi investigate karne honge.

---

# 3 Root Cause Analysis (RCA)

Production mein RCA document mein generally ye sections hote hain:

| Section | Example |
|---|---|
| Incident | Orders API unavailable |
| Impact | Customers unable to place orders |
| Detection | API monitoring alert |
| Root cause | Database connection pool exhaustion |
| Resolution | Stuck connections identified and corrected |
| Prevention | Pool metrics, alerts and timeout configuration |

**Important:** Ye sirf hypothetical example hai. Actual incident ka root cause collected evidence se hi establish hoga.

RCA mein in concepts ko alag rakhna zaroori hai:

- **Symptom:** Customer order create nahi kar paa raha.
- **Evidence:** HikariCP pending threads increase ho rahe hain.
- **Suspected cause:** Database connection pool exhaustion.
- **Confirmed root cause:** Investigation se prove hua ki connections release nahi ho rahe the ya long-running queries unhe occupy kar rahi thi.

---

# 4 Java Developer Perspective – HikariCP Connection Pool

Suppose MiniOrder application mein HikariCP connection pool configured hai:

```yaml
spring:
  datasource:
    hikari:
      maximum-pool-size: 10
      connection-timeout: 30000
```

### Configuration Explanation

| Property | Meaning |
|---|---|
| `maximum-pool-size: 10` | Pool mein maximum 10 connections |
| `connection-timeout: 30000` | Connection acquire karne ke liye maximum 30,000 milliseconds (30 seconds) wait |

### Connection Pool Exhaustion Scenario

Suppose:
- Maximum pool size = 10
- Active connections = 10
- Idle connections = 0
- New requests = 20

Agar saare 10 connections occupied hain, toh naye requests available connection ke liye wait kar sakte hain.

Agar configured timeout ke andar connection available nahi hota, toh connection acquisition timeout aa sakta hai.

### Senior Engineer Investigation

1. **Active Connections:** Kitne connections currently use ho rahe hain?
2. **Idle Connections:** Kitne connections available hain?
3. **Pending Threads:** Kitne threads connection ka wait kar rahe hain?
4. **Long-running SQL:** Kya queries unnecessarily long time le rahi hain?
5. **Connection Leaks:** Kya application connections properly release kar rahi hai?
6. **Database Performance:** Kya database slow queries, locks ya resource pressure face kar raha hai?

### Important Production Insight

Sirf pool size increase karna hamesha solution nahi hai.

Agar database ki capacity limited hai aur application pool size unnecessarily increase kar deti hai, toh database par additional load aa sakta hai.

Isliye pool tuning se pehle:
- Database capacity check karo.
- Query execution time inspect karo.
- Connection usage analyse karo.
- Application resource cleanup verify karo.

---

# 5 Production Engineer Golden Rules

1. Application process running hone ka matlab application healthy hona nahi hai.
2. Database process running hone ka matlab database queries healthy hona nahi hai.
3. Port listening hone ka matlab end-to-end connectivity successful hona nahi hai.
4. High CPU ya memory usage ko bina evidence root cause declare mat karo.
5. Application restart temporary recovery de sakta hai, permanent fix nahi.
6. Logs, metrics, network aur database evidence correlate karo.
7. RCA mein confirmed root cause aur suspected cause ko clearly distinguish karo.

**Golden Rule:** Production troubleshooting ka objective sirf service restore karna nahi, balki root cause identify karke recurrence prevent karna bhi hai.

---

[< Previous](../part-13/README.md) ---- [Next >](../part-15/README.md)
