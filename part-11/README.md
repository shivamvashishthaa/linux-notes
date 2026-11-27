## Part 11 Linux Firewall & DNS Resolution

---

### 1 Firewall Kya Hota Hai?

Firewall ek security mechanism hai jo incoming aur outgoing network traffic ko configured rules ke according allow ya block karta hai.

Suppose Black server par MiniOrder application running hai.

Humein decide karna hai ki kaunsi service ko network se access milna chahiye aur kaunsi service restricted rehni chahiye.

**Example – Black Server Firewall Policy**

| Service | Port | Access Policy |
|---|---:|---|
| SSH | 22 | Gray laptop se allowed |
| Spring Boot | 8080 | Direct public access nahi |
| PostgreSQL | 5432 | Sirf internal/private access |
| HTTP | 80 | Public access |
| HTTPS | 443 | Public access |

Production architecture mein generally database port ko public internet par expose nahi karte.

**Important:** Firewall rules source IP, destination IP, protocol, port aur network architecture ke according define kiye jaate hain.

---

### 2 UFW Kya Hai?

UFW ka full form hai **Uncomplicated Firewall**.

Ubuntu Linux mein firewall rules manage karne ke liye ye relatively simple interface provide karta hai.

#### A. Firewall Status Check

```bash
sudo ufw status
```

Firewall ka current status show karta hai.

Possible output:

```text
Status: inactive
```

Iska matlab UFW currently inactive hai.

Detailed status check:

```bash
sudo ufw status verbose
```

Ye firewall status ke saath default policies aur logging information bhi show kar sakta hai.

#### B. Firewall Rule – Allow

```bash
sudo ufw allow 443/tcp
```

TCP port `443` par incoming traffic allow karta hai.

#### C. Firewall Rule – Deny

```bash
sudo ufw deny 5432/tcp
```

TCP port `5432` par incoming traffic deny karta hai.

#### D. Firewall Rule – Delete

```bash
sudo ufw delete allow 443/tcp
```

Previously added matching allow rule remove karta hai.

**Important:** Ye commands firewall configuration modify karti hain. Inhe practical lab mein execute karne se pehle existing rules aur remote access requirements verify karna zaroori hai.

---

### 3 Default Firewall Policies

Common production approach:

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

Meaning:

| Policy | Meaning |
|---|---|
| `deny incoming` | Incoming connections default se block |
| `allow outgoing` | Outgoing connections default se allowed |

Lekin actual firewall policy business requirements aur deployment architecture ke according define hoti hai.

#### Critical SSH Safety Warning

Remote server par UFW enable karne se pehle SSH access allow hona chahiye.

Example:

```bash
sudo ufw allow 22/tcp
```

Agar SSH port allow kiye bina firewall enable kar diya, toh remote server ka SSH access block ho sakta hai.

**Production safety checklist:**

1. Current SSH port verify karo.
2. Required SSH source IP allow karo.
3. Existing firewall rules inspect karo.
4. Firewall configuration validate karo.
5. Remote access verify karne ke baad hi firewall enable karo.

Agar SSH kisi non-standard port par configured hai, toh usi actual port ko allow karna hoga.

---

### 4 Production Architecture Example

Typical MiniOrder production architecture:

```text
           Internet / Client
                   |
                   |
               Firewall
                   |
          Allow Required Ports
                   |
                   v
             Nginx Proxy
              80 / 443
                   |
                   |
                   v
          Spring Boot Application
                  8080
            Internal Access
                   |
                   |
                   v
              PostgreSQL
                  5432
             Private Network
```

**Architecture explanation:**

- Internet se required HTTP/HTTPS traffic allow hota hai.
- Nginx reverse proxy ke through Spring Boot application tak requests forward hoti hain.
- Spring Boot ka port `8080` direct public exposure se protected rehta hai.
- PostgreSQL port `5432` private network tak restricted rehta hai.

Ye ek typical architecture hai. Actual firewall rules server deployment aur network topology ke according configure honge.

---

### 5 Firewall vs Application Port

Ye difference production troubleshooting aur interviews dono ke liye important hai.

Suppose:

- Spring Boot port `8080` par listening hai.
- Firewall port `8080` par incoming traffic block kar raha hai.

Toh:

- Application process running rahega.
- `ss` command mein port listening dikhega.
- Local machine se application accessible ho sakti hai.
- Remote client ka connection fail ho sakta hai.

**Important distinction:**

| Condition | Meaning |
|---|---|
| Application running | Java process alive hai |
| Port listening | Application socket connections accept karne ke liye ready hai |
| Firewall allowing | Firewall traffic ko block nahi kar raha |
| Network reachable | Network path available hai |
| API responding | Application HTTP request process kar rahi hai |

Ye alag-alag conditions hain. Ek condition successful hone se baaki automatically successful nahi hoti.

---

### 6 Practical Lab – Black Machine

Abhi firewall mein koi configuration change nahi karenge.

Sirf current status inspect karenge:

```bash
sudo ufw status verbose
```

Observe karo:

- Firewall active hai ya inactive?
- Default incoming policy kya hai?
- Default outgoing policy kya hai?
- Koi existing allow/deny rules present hain?

**Important:** Agar output mein `Status: inactive` aaye, iska matlab UFW currently inactive hai. Iska ye matlab nahi ki machine par koi doosra firewall mechanism nahi hai.

Linux server par nftables, iptables, cloud security groups ya network-level firewalls bhi traffic control kar sakte hain.

---

### 7 DNS – Domain Name System

DNS ka full form hai **Domain Name System**.

DNS hostname ko IP address mein resolve karne mein help karta hai.

Suppose user browser mein enter karta hai:

```text
api.example.com
```

DNS resolution flow:

```text
       api.example.com
               |
               v
         DNS Resolver
               |
               v
         DNS Response
               |
               v
          203.0.113.10
```

`203.0.113.10` yahan documentation ke liye reserved example IP address hai.

Ab client resolved IP address ki help se destination server se connection establish kar sakta hai.

**Production scenario:**

MiniOrder application kisi external service ko hostname se access karti hai.

Agar hostname resolve nahi hota, toh application connection establish nahi kar paayegi.

---

### 8 DNS Troubleshooting Commands

#### A. `getent hosts`

```bash
getent hosts google.com
```

System ke configured name-resolution mechanism ke through hostname lookup karta hai.

Possible output:

```text
142.250.x.x google.com
```

Actual IP address DNS response aur location ke according change ho sakta hai.

#### B. `dig`

```bash
dig google.com
```

DNS query aur response details inspect karta hai.

Useful information:

- DNS query status.
- Resolved IP address.
- DNS server response.
- Query execution time.

#### C. `/etc/resolv.conf`

```bash
cat /etc/resolv.conf
```

Configured resolver information inspect karne ke liye use hota hai.

Example:

```text
nameserver 127.0.0.53
```

Iska matlab system local DNS stub resolver ko query bhej raha ho sakta hai.

**Important:** Kuch Linux systems mein `/etc/resolv.conf` dynamically managed hoti hai, jaise `systemd-resolved` ke through. Isliye file mein `127.0.0.53` dikhne ka matlab ye nahi ki wahi actual upstream DNS server hai.

---

### 9 Interview Scenario – Application Local Hai, Remote Access Fail

**Scenario:**

- Spring Boot application running hai.
- Port `8080` listening hai.
- Black server ke andar `curl localhost:8080` successful hai.
- Gray laptop se `curl 10.89.83.253:8080` fail ho raha hai.

**Question:** Sabse pehle kya investigate karoge?

A. Java Heap increase karoge.

B. Application bind address, firewall rules aur network connectivity check karoge.

C. PostgreSQL restart karoge.

<text weight="semibold" color="success">Correct Answer: B</text>

**Reasoning:**

Localhost se successful response milne ka matlab application local machine par accessible hai.

Lekin remote machine se request fail hone ke multiple possible reasons hain:

1. Application sirf `127.0.0.1` par bind hai.
2. Firewall port `8080` block kar raha hai.
3. Network routing ya connectivity issue hai.
4. Server ka IP address incorrect ya changed hai.
5. Cloud/network-level security rules traffic block kar rahe hain.

**Investigation commands:**

```bash
sudo ss -tulnp | grep :8080
```

Application kis address par listen kar rahi hai, check karo.

```bash
sudo ufw status verbose
```

UFW rules inspect karo.

```bash
ip addr
```

Server ka actual IP address verify karo.

```bash
ip route
```

Routing table inspect karo.

```bash
nc -vz 10.89.83.253 8080
```

Remote TCP port connectivity test karo.

**Interview-ready answer:**

"Since the application is accessible through localhost but not from the remote machine, I would first verify the application's bind address. Then I would inspect firewall rules, routing and network connectivity. I would not increase the Java heap or restart PostgreSQL without evidence of a related issue."

---

### 10 Quick Reference – Firewall & DNS

| Command | Purpose |
|---|---|
| `sudo ufw status` | Firewall status |
| `sudo ufw status verbose` | Detailed firewall rules and policies |
| `sudo ufw allow 443/tcp` | Allow TCP port 443 |
| `sudo ufw deny 5432/tcp` | Deny TCP port 5432 |
| `sudo ufw delete allow 443/tcp` | Delete matching allow rule |
| `getent hosts google.com` | System hostname resolution |
| `dig google.com` | DNS query inspection |
| `cat /etc/resolv.conf` | Resolver configuration |

---

### 11 Important Production Engineering Notes

1. Firewall application process ko stop nahi karta; woh network traffic control karta hai.
2. Port listening hone ka matlab remote connectivity guaranteed nahi hai.
3. PostgreSQL ko public internet par expose nahi karna chahiye.
4. UFW enable karne se pehle SSH access allow aur verify karo.
5. UFW inactive hone ka matlab all firewall protection absent hai, ye assume nahi karna chahiye.
6. DNS resolution successful hone ka matlab destination service reachable hai, ye guarantee nahi hai.
7. DNS, routing, firewall aur application bind address ko alag-alag investigate karo.

**Golden Rule:** Production server par minimum required network access allow karo. Firewall rules change karne se pehle existing access requirements verify karo.

---

[< Previous](../part-11/README.md) ---- [Next >](../part-12/README.md)
