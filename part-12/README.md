# Part 12: TCP Connection States, Routing & Network Troubleshooting

Java Production Support mein `TIME_WAIT`, `CLOSE_WAIT`, Connection Refused aur Connection Timeout jaise issues frequently milte hain.

---

## 1 TCP Connection States – Introduction

Jab Spring Boot application kisi client ya database ke saath TCP connection establish karti hai, toh TCP connection different states se pass hota hai.

### Basic TCP Connection Lifecycle

**Client (Gray Laptop) → Server (Black – Spring Boot)**

TCP connection establish hone ke liye Three-Way Handshake hota hai:

1. **SYN:** Client connection request bhejta hai.
2. **SYN-ACK:** Server request acknowledge karta hai.
3. **ACK:** Client confirmation bhejta hai.

Iske baad connection `ESTABLISHED` state mein aa jata hai.

### Important TCP States

| State | Meaning |
|---|---|
| `LISTEN` | Server incoming connections ka wait kar raha hai |
| `ESTABLISHED` | Connection active hai |
| `TIME_WAIT` | Connection close hone ke baad temporary wait |
| `CLOSE_WAIT` | Remote side ne connection close kiya; local application ko socket close karna baaki hai |
| `FIN_WAIT_1` | Local side ne connection close initiate kiya |
| `FIN_WAIT_2` | Remote side ke FIN ka wait |
| `SYN_SENT` | Connection request bheji gayi; response ka wait |

---

## 2 TIME_WAIT vs CLOSE_WAIT

### TIME_WAIT

TCP connection close hone ke baad active closer side kuch time wait karti hai.

**Purpose:**
- Delayed packets ko handle karna.
- Purane connection ke packets ko naye connection ke saath confuse hone se bachana.
- Connection termination ko reliable banana.

**Production understanding:**

`TIME_WAIT` normal TCP behaviour hai. Sirf high count dekhkar problem declare nahi karni chahiye. Actual impact, connection creation rate aur available resources investigate karne chahiye.

### CLOSE_WAIT

Remote side ne FIN bhej diya hai, lekin local application ne apna socket close nahi kiya.

Agar bahut saare `CLOSE_WAIT` connections continuously accumulate ho rahe hain, toh application mein socket cleanup issue ho sakta hai.

**Example – Incorrect Java resource handling:**

```java
Connection connection = dataSource.getConnection();

// Query execute hui.

// Connection.close() call nahi hua.
```

**Better approach – Try-with-resources:**

```java
try (Connection connection = dataSource.getConnection()) {

    // Database operation.

} // Connection automatically close/release hoga.
```

**Important: Connection Pool Behaviour**

Connection pool use karne par `Connection.close()` generally connection ko pool mein return karta hai. Physical database connection immediately destroy nahi hota.

---

## 3 TCP States Inspect Karne Ke Commands

### All TCP connections

```bash
ss -tan
```

### Only established connections

```bash
ss -tan state established
```

### TIME_WAIT connections

```bash
ss -tan state time-wait
```

### CLOSE_WAIT connections

```bash
ss -tan state close-wait
```

### Port-specific connections

```bash
sudo ss -tanp | grep ':8080'
```

Yahan `8080` ko target application port se replace kar sakte hain.

**Command explanation:**

| Option | Meaning |
|---|---|
| `ss` | Socket statistics utility |
| `-t` | TCP sockets |
| `-a` | All sockets |
| `-n` | Numeric IP addresses and ports |
| `-p` | Process information |

---

## 4 Routing – Network Packet Kahan Jayega?

Suppose Gray se Black ko request bhejte hain:

```text
Gray:  10.89.83.153
Black: 10.89.83.253
```
Dono same subnet `10.89.83.0/24` mein hain.

Isliye, agar dono interfaces par correct subnet configuration hai, toh local network par direct communication ho sakta hai. Default gateway ki zarurat nahi hoti.

### Routing Table Inspect Karna

```bash
ip route
```

Example output:

```text
default via 10.89.83.59 dev wlp2s0
10.89.83.0/24 dev wlp2s0
```

**Output explanation:**

| Entry | Meaning |
|---|---|
| `default via 10.89.83.59` | Default gateway |
| `10.89.83.0/24` | Local network route |
| `dev wlp2s0` | Network interface jiske through packet jayega |

**Production troubleshooting:**

Galat IP, subnet mask ya routing configuration ke kaaran packets destination server tak nahi pahunch paate.

---

## 5 Network Connectivity Diagnostics

| Command | Purpose |
|---|---|
| `ping` | ICMP reachability check |
| `ip addr` | Network interfaces aur IP addresses |
| `ip route` | Routing table |
| `ss` | TCP/UDP sockets |
| `traceroute` | Network path investigation |
| `curl -v` | HTTP connection debugging |
| `dig` | DNS debugging |

### Useful Commands

```bash
ping -c 4 10.89.83.253
```

```bash
ip addr
```

```bash
ip route
```

```bash
nc -vz 10.89.83.253 8080
```

```bash
curl -v http://10.89.83.253:8080
```

**Note:** Ping fail hone ka matlab hamesha server unreachable hona nahi hai. ICMP firewall se blocked ho sakta hai, jabki TCP application port accessible ho.

---

## 6 Connection Refused vs Connection Timeout

| Issue | Meaning | Possible Cause |
|---|---|---|
| Connection Refused | Target tak connection attempt pahunchi, lekin connection accept nahi hua | Port par listener nahi hai, ya actively reject kiya gaya |
| Connection Timeout | Expected time mein connection establish nahi hua | Firewall drop, routing issue, network failure, unreachable host |
| Connection Reset | Existing connection abruptly reset ho gaya | Remote process, proxy, firewall ya network ne connection reset kiya |

**Important:** Error message se initial direction milti hai, lekin final root cause logs, socket state aur network evidence se confirm karna hota hai.

---

## 7 Practical – Black Machine

Abhi sirf safe, read-only inspection karni hai. Koi configuration change nahi karna.

### Step 1: CLOSE_WAIT connections check karo

```bash
ss -tan state close-wait
```

Isse Black server par currently `CLOSE_WAIT` state mein connections dikhenge.

### Step 2: TIME_WAIT connections check karo

```bash
ss -tan state time-wait
```

Isse `TIME_WAIT` state mein connections dikhenge.

### Step 3: Routing table check karo

```bash
ip route
```

Isse server ka routing configuration dikhega.

**Practical objective:**
- TCP connection states observe karna.
- CLOSE_WAIT aur TIME_WAIT ka difference samajhna.
- Black machine ki routing configuration verify karna.

---

## 8 Interview Scenario – CLOSE_WAIT Resource Leak

**Scenario:**

Suppose production Java application mein:

- 2,000 `CLOSE_WAIT` connections hain.
- Connections continuously increase ho rahe hain.
- Application restart karne par temporarily issue solve ho jata hai.

**Question:** Strongest suspicion kya hoga?

A. TCP three-way handshake fail ho raha hai.

B. Application sockets/connections properly close nahi kar rahi; resource leak investigate karna chahiye.

C. DNS resolution slow hai.

**Correct Answer: B**

**Reasoning:**

`CLOSE_WAIT` indicate karta hai ki remote peer ne connection close kar diya hai, lekin local endpoint ne socket close nahi kiya.

Agar count continuously increase ho raha hai, toh application ke resource cleanup mechanism ko investigate karna chahiye.

Possible investigation:
1. Java code mein socket/connection cleanup check karna.
2. Try-with-resources aur `finally` blocks verify karna.
3. Connection pool metrics inspect karna.
4. Thread dumps aur application logs correlate karna.
5. Identify karna ki kaunsa code path resources release nahi kar raha.

Application restart existing sockets ko clear kar sakta hai, lekin underlying bug fix nahi karta.

**Production Golden Rule:** Restart temporary recovery ho sakta hai; permanent resolution ke liye root cause identify aur fix karna zaroori hai.

---

[< Previous](../part-11/README.md) ---- [Next >](../part-13/README.md)
