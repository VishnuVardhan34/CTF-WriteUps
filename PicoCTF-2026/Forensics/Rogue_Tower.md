# Rogue Tower (300 pts) – Writeup

## 🧩 Challenge Overview
A suspicious cell tower was detected in captured network traffic. The goal was to:
- Identify the rogue tower
- Find the compromised device
- Recover the exfiltrated flag

---

## 🔍 Step 1: Detect Rogue Tower Broadcast

Based on the hint, we filtered UDP traffic on port **55000**:

```wireshark
udp.port == 55000
```

Inspecting the packets revealed suspicious broadcast messages:

```
UNAUTHORIZED-TEST-NETWORK PLMN=00101 CELLID=96261
```

These indicate unauthorized cell tower broadcasts.

---

## 📡 Step 2: Identify Connected Device

The hint suggested checking **HTTP User-Agent headers** for the CELLID.

We filtered:

```wireshark
http.user_agent contains "96264"
```

This revealed a request with the following User-Agent:

```
MobileDevice/1.0 (IMSI:310410728284734; CELL:92771)
```

From this:
- **IMSI**: `310410728284734`
- This is the compromised device identifier

---

## 🌐 Step 3: Find Victim IP

Using the identified device, we tracked the source IP of the HTTP requests:

```wireshark
ip.src == 10.100.77.215
```

This confirmed the victim device:
- **Victim IP**: `10.100.77.215`

---

## 📤 Step 4: Extract Exfiltrated Data

We observed multiple HTTP POST requests to:

```
/upload
```

Steps:
1. Open Wireshark → **File → Export Objects → HTTP**
2. Extract all POST payloads
3. Concatenate them:

```bash
cat file1 file2 file3 file4 file5 file6 > combined.txt
```

Resulting data:

```
QlFRV3djdU9ACFVNB2hQB15UbUwEQABGbVkFCwUHUVEBRQ==
```

---

## 🔐 Step 5: Decode & Decrypt

The data appeared to be Base64 encoded.

### Base64 Decode
Decoding produced binary data → likely encrypted.

### Key Derivation
From the hint:
> Encryption key is derived from IMSI

We used the **last 8 digits of IMSI**:

```
IMSI = 310410728284734
Key = 8284734
```

---

## 🧮 Step 6: XOR Decryption

We performed XOR using the key:

```python
import base64

data_b64 = "QlFRV3djdU9ACFVNB2hQB15UbUwEQABGbVkFCwUHUVEBRQ=="

imsi = "310410728284734"
key = imsi[-8:]

encrypted_data = base64.b64decode(data_b64)
key_bytes = key.encode()

decrypted = bytearray()

for i in range(len(encrypted_data)):
    decrypted.append(encrypted_data[i] ^ key_bytes[i % len(key_bytes)])

print(decrypted.decode())
```

---

## 🎯 Final Flag

```
picoCTF{r0gu3_c3ll_t0w3r_a7310be3}
```

---
