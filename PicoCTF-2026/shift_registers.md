# CTF Writeup: shift registers

## 🧠 Challenge Overview

Author: Philip Thayer

Description
I learned about lfsr today in school so i decided to implement it in my program. It must be safe right? chall.py output.txt

``` python
lfsr = key & 0xFF
```

This means only the **last 8 bits of the key** are used.

------------------------------------------------------------------------

## 🔍 Vulnerability

-   LFSR state size = **8 bits**
-   Total possible states = **256**
-   This makes brute force trivial

------------------------------------------------------------------------

## ⚙️ Exploit Strategy

1.  Try all 256 possible seeds
2.  Generate keystream using LFSR
3.  XOR with ciphertext
4.  Look for readable plaintext / flag format

------------------------------------------------------------------------

## 💻 Solve Script

``` python
from Crypto.Util.number import long_to_bytes

ct_hex = "21c1b705764e4bfdafd01e0bfdbc38d5eadf92991cdd347064e37444e517d661cea9"
ct = bytes.fromhex(ct_hex)

def steplfsr(lfsr):
    b7 = (lfsr >> 7) & 1
    b5 = (lfsr >> 5) & 1
    b4 = (lfsr >> 4) & 1
    b3 = (lfsr >> 3) & 1
    feedback = b7 ^ b5 ^ b4 ^ b3
    return ((feedback << 7) | (lfsr >> 1)) & 0xFF

def decrypt(ct, seed):
    lfsr = seed
    pt = []
    for c in ct:
        lfsr = steplfsr(lfsr)
        pt.append(c ^ lfsr)
    return bytes(pt)

for seed in range(256):
    pt = decrypt(ct, seed)
    if b"{" in pt or b"flag" in pt.lower():
        print(seed, pt)
```

------------------------------------------------------------------------

## 🚩 Flag
picoCTF{l1n3ar_f33dback_sh1ft_r3g}

------------------------------------------------------------------------