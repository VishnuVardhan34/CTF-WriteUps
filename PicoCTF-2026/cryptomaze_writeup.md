# 🏁 picoCTF Writeup: cryptomaze

## 📌 Challenge Overview

This challenge involves recovering a hidden flag encrypted using a combination of **LFSR (Linear Feedback Shift Register)** and **AES**.

### Given:
- Initial state of the LFSR  
- Tap positions  
- Encrypted flag (hex format)

### Objective:
Reconstruct the AES key using the LFSR and decrypt the ciphertext.

---

## 💡 Key Insights

1. The LFSR generates a **128-bit keystream**.
2. This keystream is converted into a **16-byte AES key**:
   - Split into 8-bit chunks (16 total)
   - Convert each chunk to a byte
3. The encrypted flag is decrypted using:
   - **AES (ECB mode)**
   - Ciphertext converted from hex → bytes

---

## 🧠 Approach

The exact LFSR configuration (direction, tap indexing, shifting) was not fully specified.

To handle this, we brute-forced all possible variations of:
- Output bit selection (left/right)
- Tap indexing direction
- Shift direction

This results in **8 possible configurations**, which we test to find the correct key.

---

## 🛠️ Exploit Script

```python
from Crypto.Cipher import AES
from itertools import product

state = [0,0,1,0,0,1,0,1,1,1,1,0,1,1,0,0,1,0,0,1,0,1,1,0,1,0,0,1,0,1,0,1,
         0,1,0,0,1,1,0,1,1,0,0,0,1,0,1,1,1,1,0,0,0,1,0,0,0,1,0,1,1,0,1,1]

taps = [63, 61, 60, 58]

ct = bytes.fromhex(
    "8f0e6d0f5b0dc1db201948b9e0cebd8f06069ee9ff30c87bd50b31d6fd72c4c4"
    "38338e7e04fbddef0c6260a4eb758417"
)

def lfsr(state, taps, mode):
    s = state.copy()
    out = []

    for _ in range(128):
        # Output bit
        out.append(s[-1] if mode[0] == 0 else s[0])

        # Feedback calculation
        fb = 0
        for t in taps:
            fb ^= s[t] if mode[1] == 0 else s[len(s) - 1 - t]

        # Shift operation
        if mode[2] == 0:
            s = [fb] + s[:-1]
        else:
            s = s[1:] + [fb]

    return out

for mode in product([0, 1], repeat=3):
    bits = lfsr(state, taps, mode)

    key = bytes(
        int(''.join(map(str, bits[i:i+8])), 2)
        for i in range(0, 128, 8)
    )

    try:
        pt = AES.new(key, AES.MODE_ECB).decrypt(ct)

        if b"flag" in pt or b"pico" in pt:
            print("[+] Key found!")
            print("[+] Plaintext:", pt)
    except:
        pass
```

---

## 🔍 Explanation

- **LFSR Generation:** Produces 128 bits using different configurations  
- **Key Derivation:** Groups bits into bytes → forms AES key  
- **Brute Force:** Tests all 8 LFSR behavior combinations  
- **Validation:** Checks decrypted output for known flag patterns  

---

## 🚩 Flag

```
picoCTF{scr8mbledt_flvg_42186d25}
```

---

## ✅ Takeaways

- When implementation details are unclear, **brute-forcing configurations** is effective  
- LFSR-based schemes are predictable if internal parameters are known  
- AES-ECB is insecure when key derivation is weak  
