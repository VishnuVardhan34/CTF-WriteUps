# 🏁 picoCTF Writeup: StegoRSA

## 📌 Challenge Overview

In this challenge, we are provided with:

- `image.jpg`
- `flag.enc`

💡 **Hint:** *Explore Metadata*

The objective is to retrieve and decrypt the hidden flag.

---

## 🔎 Step 1: Metadata Analysis

We begin by inspecting the metadata of the provided image file using `exiftool`:

```bash
exiftool image.jpg
🔐 Discovery

Within the metadata, we discover an encoded block of data. After decoding it using CyberChef, we obtain an RSA private key.

🔑 Extracted Private Key

-----BEGIN PRIVATE KEY-----
MIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQDYBgAHyqqQzOXC
ul6Ou/JnDC54T5xRwn+kN38MmmF9o+OK4O48zIFfIBW+O2Oz7rGm8FKQUolmlMY/
POXjTz0/KKYKPbuJ322IWM1AAvBmkmlzRu6D2Td9joRZ1dIpEDCe5jru9IIF2G6K
195dVhKcgK4urkNAkKeVh++5bXvBKitb5q8cuCDwnsPDdO6RKX2DrXyC7OoPHrrp
TaS9sAsmFqBUVuAkUSsqH6x3IeHwKu/ZDi9DRW246RJTUzkH+8FoLnAVOUL3fd5e
tw3ZIL6K1Ag/MA4maiO9uv9+mEWAHQQ4uY/FCkG+2joaBSBimd4AORfhOi3Dbb0T
WBdVgH7hAgMBAAECggEADPyh23YPNmKyEKiPgxdYm6I1aDtZgYcO0USal0tv71un
JCARBH/TKoDJW9kSTbS8soV9wjJLEzsgFrjKlYV70DBpeIHE9LVFXoo/pyCyGwmD
7EesMsogElTbPzMaBgjUknXy/q79gtxTy3YuNqhkOKtKBXGJpp/Xqlc8dcZnp2lt
Pkg14hN9KI/CH1u65ab5vKz+9+pJAa2TdFnPkh4m033Smj7ZxTvV8wTeUcOSGiMj
2RkjUpRReQPijCAFLudcc0kJBrzlMcLWOnrFMZuZ36btHaXpvDusqUbreKgu0/gY
S3VATTXG45j+kpmrGUQ5hx2MtAAvmXi6QAE5ivJeMQKBgQDxXzg0AZxKgTSqbDdB
3ObO/rn5qdlyfudIHUHf7Htfx8zCzFbrxRZfMsWw26apunyT51ziINam8nfBnNTv
kC3eI0UD977DP/g5tUHuDAc9dmtuHDykN7mmIdp7kQ8zpwwdHJxQGsS4F9hNUr1n
9OOx1xRNFrJczhcHcOvJ9c1i0QKBgQDlHYJrtEwSJQGpvM60OHUoo618htdL6xiO
WwUTzcaoNz+x0pQjfXcCWBJL+T+Sj6HcRImWBedAB9JKm8rDex+NJH995IIRo1Ia
GeLCZgI/ihWaSC4VC1yaDh18qnl/MSp0G4v6j925e4vflyE1nMknmj2xlNTQYjZs
KhXyTrO/EQKBgQDPaT9mkSu4aibTe4JQOn6ryQAOpgGQ/bPIqDt/LDsoJwyxJ95Z
Y1bCH2L5gwZIO1Pp1JpgRk+tzhVSbm4cHg0MIcqgijeGmGW5USSCZhuimSvfxqvl
gW0qcVTJcfFaNWWXbopz20zH1NWuPDc+KZWvsF5lj+ddEEuBvWsgdPQ0wQKBgFbR
odQyVAkkIMczFpjQNAUcUOc5KWhJQ9rdvsTMWxTvKqG1jBEOwAQRX42Oe3qMFuei
yQgiYIiw7gz7kBAXHdOcGvuXlXodi0T8viKwCPYO2zTFWUD8NzDhXGcbKkL6XH32
2kouLfTVTiGB4UGxkcACAJLENQhpzvmZ0Qsqq44hAoGBAOkG2yJhbe4h3g3U1Wlk
7hqQnaG17cg0A8+3in10FnehrWQBlBvYsNePERZstTxMQiElFLv5MCjRfmjm+SJP
K4gM/lUgeoNXCZWz1L34GtU88wt7NW6j+XmbeyNl4vFbwouzQTI01rHhvxmn+qmq
+HuuC/g9jASiX2r038RKYSgx
-----END PRIVATE KEY-----
💾 Step 2: Save the Key

Save the extracted key into a file:
```bash
nano pico.pem

🔓 Step 3: Decrypt the Flag

Use OpenSSL to decrypt the encrypted flag:
```bash
openssl rsautl -decrypt -inkey pico.pem -in flag.enc -out flag.txt

🚩 Final Flag
picoCTF{rs4_k3y_1n_1mg_26586619}