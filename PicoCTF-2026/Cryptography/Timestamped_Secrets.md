# 🏁 picoCTF Writeup: TimeStamped Secrtes

## 📌 Challenge Overview

Description
Someone encrypted a message using AES in ECB mode but they weren’t very careful with their key. Turns out it’s derived from something as simple as the current time! Can you uncover the key and decrypt the flag?
Download the encrypted message: message
You may also find the encryption script helpful: code

💡 **Hint:** *encryption.py is a redacted example of the program*

## Walkthrough:
🔑 Idea
Key = sha256(str(timestamp))[:16]
Mode = AES-ECB
Try timestamps around 1770242610
Decrypt and look for "picoCTF{"

### script for that:
from hashlib import sha256
from Crypto.Cipher import AES
from Crypto.Util.Padding import unpad

# given values
ciphertext_hex = "71cd3848348a45b82789f710c3321aceab2171e004200b57fe9cc64d4ea33cec"
ciphertext = bytes.fromhex(ciphertext_hex)

# approximate timestamp
target_time = 1770242610

# brute-force window (adjust if needed)
WINDOW = 200000  # try ±200k seconds (~2 days)

for ts in range(target_time - WINDOW, target_time + WINDOW):
    key = sha256(str(ts).encode()).digest()[:16]

    try:
        cipher = AES.new(key, AES.MODE_ECB)
        pt = unpad(cipher.decrypt(ciphertext), AES.block_size)

        if b"picoCTF" in pt:
            print("[+] FOUND!")
            print("Timestamp:", ts)
            print("Key:", key.hex())
            print("Plaintext:", pt.decode())
            break

    except:
        continue

🚩 Final Flag
picoCTF{sa3S_sEc9t_91609b3c}