# Part 13: Network Packet Troubleshooting – Traceroute, Packet Loss & TCP Connection Failures   

Production mein kabhi-kabhi application, database aur server sab running hote hain, phir bhi requests fail hoti hain. Aise cases mein packet-level investigation useful hoti hai.

---

## 1 Connection Refused vs Connection Timeout

Dono errors ka difference samajhna zaroori hai.

### Connection Refused

- Target machine tak connection attempt pahunch sakta hai.
- Lekin target port par listener nahi hai, ya connection actively reject kiya ja raha hai.
- Example: Spring Boot port `8080` par running nahi hai.

Example:

```text
connect to 10.89.83.253 port 8080 failed: Connection refused
```

### Connection Timeout

- Connection attempt ko expected time ke andar response nahi mila.
- Firewall silently packets drop kar sakta hai.
- Routing issue ya network failure bhi possible hai.

Example:

```text
connect to 10.89.83.253 port 8080 timed out
```

**Production insight:** Error message ek clue hai, final root cause nahi. Dono cases mein server, firewall aur network configuration verify karni chahiye.

| Error | Initial suspicion |
|---|---|
| Connection Refused | No listener or active rejection |
| Connection Timeout | Packet drop, routing issue, unreachable destination |

---

## 2 Traceroute – Network Path Investigation

Suppose Gray laptop se kisi external API ko request bhej rahe ho.

Tumhe investigate karna hai ki network path mein packets kis router ya hop tak pahunch rahe hain.

### Command

```bash
traceroute google.com
```

Traceroute intermediate network hops identify karne ki koshish karta hai.

### Alternative

```bash
tracepath google.com
```

Agar command installed nahi hai toh abhi install karne ki zarurat nahi.

**Important:** Traceroute output mein `* * *` ka matlab automatically network failure nahi hota. Intermediate routers ICMP responses filter ya rate-limit kar sakte hain.

Traceroute ke results ko destination connectivity, TCP tests aur application logs ke saath correlate karna chahiye.

---

## 3 Packet Loss

Suppose Gray se Black ko 100 packets bheje:

- Sent: 100
- Received: 95
- Lost: 5

### Packet Loss Formula

\[
\text{Packet Loss \%} =
\frac{\text{Sent}-\text{Received}}{\text{Sent}}\times100
\]

\[
= \frac{100-95}{100}\times100 = 5\%
\]

5% packet loss ka matlab network communication unreliable ho sakti hai, especially latency-sensitive applications mein.

### Command

```bash
ping -c 100 10.89.83.253
```

**Command explanation:**

| Option | Meaning |
|---|---|
| `ping` | ICMP echo requests bhejta hai |
| `-c 100` | 100 requests bhejne ke baad stop karta hai |
| `10.89.83.253` | Target machine IP |

**Production insight:**

Packet loss ko sirf ek ping result ke basis par final root cause nahi maanna chahiye. ICMP traffic filter ya rate-limit ho sakta hai. Application ke actual TCP traffic par bhi investigation karni chahiye.

---

## 4 TCP Connection Testing – Netcat

`nc` (Netcat) se specific port ki connectivity check kar sakte hain.

### Command

```bash
nc -vz 10.89.83.253 22
```

Is command se verify karenge ki Black machine ka SSH port `22` reachable hai ya nahi.

**Command explanation:**

| Option | Meaning |
|---|---|
| `nc` | Netcat networking utility |
| `-v` | Detailed output |
| `-z` | Connection test without sending application data |
| `10.89.83.253` | Target IP |
| `22` | Target port |

Same command Spring Boot port ke liye:

```bash
nc -vz 10.89.83.253 8080
```

**Important:** Successful TCP connection test ka matlab sirf port-level connectivity establish hui. Isse application API healthy hone ki guarantee nahi milti.

---

## 5 Packet Capture – tcpdump

`tcpdump` advanced production troubleshooting tool hai. Isse network interfaces par packets capture aur inspect kar sakte hain.

### Command

```bash
sudo tcpdump -i any port 8080
```

**Command explanation:**

| Part | Meaning |
|---|---|
| `sudo` | Required privileges ke saath command run karta hai |
| `tcpdump` | Network packets capture karta hai |
| `-i any` | Available interfaces par capture |
| `port 8080` | Port 8080 se related traffic filter |

Capture stop karne ke liye `Ctrl+C` press karo.

### Production use cases

- Client request server tak pahunch rahi hai ya nahi.
- Server response bhej raha hai ya nahi.
- TCP SYN packets aa rahe hain ya nahi.
- TCP connection reset ho raha hai ya nahi.
- Retransmissions ya packet-level delays investigate karna.

**Security note:** Packet captures mein sensitive information aa sakti hai. Production mein capture scope limited rakho, duration control karo aur capture files/access secure rakho.

---

## 6 Production Incident Example – MiniOrder Unreachable

Suppose MiniOrder Gray laptop se accessible nahi hai.

| Investigation | Result |
|---|---|
| Java process | Running |
| Port 8080 | Listening |
| Local curl | Successful |
| Remote curl | Timeout |
| Firewall | Port 8080 blocked |

### Investigation Reasoning

1. Java process running hai, isliye application process completely down nahi hai.
2. Port 8080 listening hai, isliye server par listener available hai.
3. Local curl successful hai, isliye local HTTP request respond kar rahi hai.
4. Remote curl timeout de raha hai, isliye remote connectivity investigate karni hogi.
5. Firewall rules mein port 8080 blocked mila.

**Likely root cause:** Firewall incoming traffic block kar raha hai.

### Senior Engineer Approach

Evidence ke basis par root cause identify karna chahiye, sirf application restart nahi karni chahiye.

**Important:** Production firewall rule change karne se pehle access requirements aur security policy verify karni chahiye.

---

## 7 Network Troubleshooting – Quick Reference

| Problem | Investigation command |
|---|---|
| Network reachability | `ping -c 4 <IP>` |
| Network path | `traceroute <HOST>` |
| Alternative path check | `tracepath <HOST>` |
| Packet loss | `ping -c 100 <IP>` |
| TCP port connectivity | `nc -vz <IP> <PORT>` |
| HTTP connectivity | `curl -v http://<IP>:<PORT>` |
| Packet capture | `sudo tcpdump -i any port <PORT>` |
| Routing table | `ip route` |
| Socket inspection | `ss -tan` |

### Initial Observations

| Component | Status |
|---|---|
| Spring Boot process | Running |
| Server CPU utilization | 35% |
| Available RAM | 5 GB |
| PostgreSQL process | Running |
| Orders API | Timeout |
| Recent deployment | No |

### Investigation Approach

Initial investigation mein ye steps follow karenge:

1. **Application Logs:** Orders API timeout ke time ke errors aur stack traces check karna.
2. **Database Connectivity:** PostgreSQL port, connection attempts aur database health verify karna.
3. **Connection Pool Metrics:** HikariCP active, idle, pending connections aur connection acquisition timeout inspect karna.
4. **Disk I/O:** Database disk latency, queue size aur utilization investigate karna.
5. **Network Evidence:** Application-to-database connectivity, TCP states aur packet-level evidence correlate karna.
6. **Resource Analysis:** CPU, memory, load average aur process-level metrics verify karna.

### Senior Engineer Thinking

Sabse pehle application logs, database connectivity aur connection pool metrics check karunga. Uske baad disk I/O aur network evidence correlate karunga.

**Golden Rule:**

Symptom ko nahi, root cause ko identify aur fix karna hai.

Restart temporary recovery de sakta hai, lekin permanent resolution ke liye root cause aur preventive action zaroori hain.

---

[< Previous](../part-12/README.md) ---- [Next >](../part-14/README.md)
