# PicoCTF RSA Challenge Writeup

## Challenge Overview
We are given RSA parameters:
- n (modulus)
- e (public exponent)
- c (ciphertext)

The goal is to recover the plaintext (flag).

## Vulnerability
The private exponent `d` is very small (256 bits), making the system vulnerable to:
- Wiener Attack (partial success)
- Boneh–Durfee Attack (intended)

## Approach

### Step 1: Attempt Wiener Attack
We compute continued fractions of e/n and generate convergents to obtain candidates for d.

### Step 2: Validate Candidates
Instead of trusting Wiener blindly, we:
- Decrypt using each candidate d
- Check if plaintext contains `picoCTF{}`

This filters out false positives.

### Step 3: Extract Flag
Once correct d is found:
- Decrypt ciphertext
- Convert to bytes
- Extract flag

## Exploit Code
```python
from Crypto.Util.number import long_to_bytes
import math

def wiener_attack(e, n):
    def cont_frac(n, d):
        while d:
            yield n // d
            n, d = d, n % d

    def convergents(cf):
        n0, d0 = 1, 0
        n1, d1 = cf[0], 1
        yield (n1, d1)
        for q in cf[1:]:
            n2 = q * n1 + n0
            d2 = q * d1 + d0
            yield (n2, d2)
            n0, d0 = n1, d1
            n1, d1 = n2, d2

    cf = list(cont_frac(e, n))

    for k, d in convergents(cf):
        if k == 0:
            continue
        if (e*d - 1) % k != 0:
            continue

        phi = (e*d - 1) // k
        s = n - phi + 1
        discr = s*s - 4*n

        if discr >= 0:
            t = int(math.isqrt(discr))
            if t*t == discr:
                yield d

for d in wiener_attack(e, n):
    pt = long_to_bytes(pow(c, d, n))
    if b"picoCTF{" in pt:
        print(pt)
        break
```

```Final Flag
picoCTF{sm4ll_d_6ea2db76}
```