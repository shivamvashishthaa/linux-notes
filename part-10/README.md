## Part 10: Linux Networking & Troubleshooting

Java Production Support mein networking bahut important hai. Suppose MiniOrder Spring Boot application running hai, lekin Postman se API access nahi ho rahi.

Application start hone ke baad bhi connection fail ho sakta hai. Humein identify karna hoga ki issue application mein hai, port mein, firewall mein, DNS mein ya network connectivity mein.

---

### 1 IP Address aur Port

IP address machine ko network par identify karta hai aur port us machine par particular application ya service ko identify karta hai.

**Example – MiniOrder Server**

| Service | IP Address | Port |
|---|---|---:|
| Black Server | `10.89.83.253` | — |
| Spring Boot | `10.89.83.253` | `8080` |
| PostgreSQL | `10.89.83.253` | `5432` |
| SSH | `10.89.83.253` | `22` |

Agar API URL hai:

```text
http://10.89.83.253:8080/api/orders
```

Toh:

- `10.89.83.253` = Black server IP address
- `8080` = Spring Boot application port
- `/api/orders` = API endpoint

**Important:** IP address machine tak pahunchne mein help karta hai, jabki port us machine par correct service tak pahunchne mein help karta hai.

---

### 2 TCP vs UDP

**TCP – Transmission Control Protocol**

- Connection-oriented protocol.
- Reliable data delivery provide karta hai.
- Data ordering maintain karta hai.
- Lost packets retransmit ho sakte hain.
- Connection establish karne ke liye three-way handshake use karta hai.

Examples: HTTP/HTTPS over TCP, SSH, PostgreSQL connections.

**UDP – User Datagram Protocol**

- Connectionless protocol.
- Delivery guarantee nahi deta.
- TCP ke comparison mein lower protocol overhead.
- Built-in retransmission aur ordering guarantee nahi hoti.

Examples: DNS (commonly), voice/video traffic.

**Production understanding:** Agar Spring Boot application PostgreSQL se connect nahi kar pa rahi, toh TCP connectivity, destination IP, port aur firewall rules investigate karne honge.

---

### 3 Port Listening Check – `ss`

`ss` Linux mein network sockets aur listening ports inspect karne ke liye useful command hai.

```bash
sudo ss -tulnp
```

**Command options:**

| Option | Meaning |
|---|---|
| `-t` | TCP sockets |
| `-u` | UDP sockets |
| `-l` | Listening sockets |
| `-n` | Numeric IP addresses and ports |
| `-p` | Process information |
| `sudo` | Process details dekhne ke liye elevated permissions |

Specific port check karne ke liye:

```bash
sudo ss -tulnp | grep :8080
```

Suppose output:

```text
tcp LISTEN 0 100 0.0.0.0:8080 0.0.0.0:* users:(("java",pid=1234,fd=45))
```

Iska matlab:

- `tcp` = TCP socket.
- `LISTEN` = Connections accept karne ke liye ready.
- `0.0.0.0:8080` = All IPv4 interfaces par port 8080 listening.
- `java` = Socket Java process ke paas hai.
- `pid=1234` = Java process ID.

**Important difference:**

| Binding Address | Meaning |
|---|---|
| `127.0.0.1:8080` | Sirf local machine se accessible |
| `0.0.0.0:8080` | All IPv4 network interfaces par listening |
| `10.89.83.253:8080` | Specific interface IP par listening |

**Production scenario:**

Agar Spring Boot application `127.0.0.1:8080` par bind hai, toh remote machine se direct access nahi milega, chahe application successfully running ho.

---

### 4 Connectivity Troubleshooting

#### A. Ping – Network Reachability

```bash
ping -c 4 10.89.83.253
```

Options:

- `-c 4` = Maximum 4 ICMP echo requests send karega.
- `10.89.83.253` = Destination IP address.

Ping se pata chalta hai ki destination ICMP request ka response de raha hai ya nahi.

**Important:** Ping fail hone ka matlab hamesha server down hona nahi hai. Firewall ya network policy ICMP traffic block kar sakti hai.

#### B. Netcat – Port Connectivity

```bash
nc -vz 10.89.83.253 8080
```

Options:

- `-v` = Verbose output.
- `-z` = Zero-I/O mode; connection test karta hai, application data send nahi karta.

Isse check kar sakte hain ki destination machine ka port `8080` TCP connection accept kar raha hai ya nahi.

Possible outcomes:

- `succeeded` = TCP connection establish ho gaya.
- `Connection refused` = Destination tak reach ho sakte hain, lekin port par service listening nahi hai ya connection reject ho raha hai.
- `Connection timed out` = Expected time mein response nahi mila; firewall, routing ya network issue possible hai.

#### C. Curl – HTTP Response Check

```bash
curl -v http://10.89.83.253:8080/actuator/health
```

`-v` verbose mode enable karta hai.

Ye details show kar sakta hai:

- DNS resolution (hostname use karne par).
- TCP connection establishment.
- HTTP request aur response headers.
- HTTP status code.
- Connection errors.

**Note:** `/actuator/health` endpoint tabhi work karega jab Spring Boot Actuator configured ho aur endpoint accessible ho.

Agar Actuator configured nahi hai, toh kisi known API endpoint ko test kar sakte hain.

---

### 5 DNS Troubleshooting

Suppose MiniOrder application PostgreSQL ko hostname se access karti hai:

```text
postgres-db.internal
```

Application configuration:

```properties
spring.datasource.url=jdbc:postgresql://postgres-db.internal:5432/miniorder
```

Agar hostname IP address mein resolve nahi hua, toh database connection fail ho sakta hai.

#### A. `getent hosts`

```bash
getent hosts postgres-db.internal
```

Ye system ke configured name-resolution mechanism se hostname lookup karta hai.

Possible output:

```text
10.89.83.254 postgres-db.internal
```

Iska matlab hostname `10.89.83.254` IP address mein resolve ho gaya.

#### B. `dig`

```bash
dig google.com
```

`dig` DNS query aur response details inspect karne ke liye use hota hai.

Useful information:

- DNS server response.
- Resolved IP address.
- Query status.
- DNS response time.

**Difference:**

| Command | Purpose |
|---|---|
| `getent hosts` | System ke configured name-resolution mechanism se lookup |
| `dig` | DNS query aur DNS response inspect karna |

**Note:** `dig` available nahi hai toh system par DNS utilities package ki zarurat ho sakti hai. Abhi installation ki zarurat nahi; pehle available commands se troubleshooting karenge.

---

### 6 Production Troubleshooting Flow – MiniOrder API Unavailable

**Scenario:** MiniOrder Spring Boot application running hai, lekin Postman se API access nahi ho rahi.

Is situation mein random configuration change karne ke bajaye step-by-step investigation karenge.

**Step 1 – Application Process Check**

```bash
ps aux | grep java
```

Verify karo ki Java application process running hai.

**Step 2 – Port Listening Check**

```bash
sudo ss -tulnp | grep :8080
```

Verify karo ki Spring Boot port `8080` par listening hai.

**Step 3 – Local Connectivity Check**

```bash
curl -v http://localhost:8080/actuator/health
```

Isse same server ke andar se application access test kar sakte hain.

**Step 4 – Remote Port Connectivity Check**

```bash
nc -vz 10.89.83.253 8080
```

Verify karo ki remote machine se TCP connection establish ho raha hai ya nahi.

**Step 5 – Firewall Check**

Firewall rules inspect karo. Linux server par firewall implementation ke according `ufw` ya `firewalld` rules investigate kiye ja sakte hain.

**Step 6 – Network Connectivity Check**

```bash
ping -c 4 10.89.83.253
```

Network-level connectivity investigate karo. Yaad rakho, ICMP block hone par bhi TCP application traffic work kar sakta hai.

**Step 7 – DNS Check**

```bash
getent hosts postgres-db.internal
```

Agar hostname use ho raha hai, toh verify karo ki woh expected IP mein resolve ho raha hai.

**Step 8 – HTTP Response and Application Logs**

```bash
curl -v http://10.89.83.253:8080/api/orders
```

Application logs inspect karke actual error identify karo.

---

### 7 Quick Reference – Networking Commands

| Command | Purpose |
|---|---|
| `ip addr` | Machine ke IP addresses inspect karna |
| `ip route` | Routing table inspect karna |
| `ss -tulnp` | Listening ports aur processes check karna |
| `ping -c 4 <IP>` | ICMP connectivity test |
| `nc -vz <IP> <PORT>` | TCP port connectivity test |
| `curl -v <URL>` | HTTP request/response troubleshoot karna |
| `getent hosts <HOST>` | System hostname resolution check |
| `dig <HOST>` | DNS query inspect karna |

---

### 8 Important Production Engineering Notes

1. Application process running hone ka matlab ye nahi ki remote clients usse access kar sakte hain.
2. Port listening hone ka matlab ye nahi ki firewall ya network path traffic allow kar raha hai.
3. Ping fail hone ka matlab server definitely down nahi hai.
4. TCP connection successful hone ka matlab API successfully response degi, ye guarantee nahi hai.
5. DNS resolution successful hone ka matlab destination service reachable hai, ye guarantee nahi hai.
6. Troubleshooting hamesha layer-by-layer karo: Process → Port → Local Connectivity → Network → Firewall → HTTP → Application Logs.

**Golden Rule:** Production issue troubleshoot karte waqt pehle failure ki exact layer identify karo. Bina evidence ke application restart ya firewall configuration change mat karo.

---

[< Previous](../part-09/README.md) ---- [Next >](../part-01/README.md)
