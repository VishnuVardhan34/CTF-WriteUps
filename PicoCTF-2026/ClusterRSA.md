# 🏴‍☠️ PicoCTF Writeup — Multi-Prime RSA Challenge

## 📌 Challenge Description

We are given an RSA public key and ciphertext:

```
n = 8749002899132047699790752490331099938058737706735201354674975134719667510377522805717156720453193651
e = 65537
ct = 3891158515405030211396309867177046660195995913985068178988858029936868358096672572274111514200511662
```

Hint:

> RSA usually means two primes... but what if someone got greedy?

---

## 🧠 Analysis

Standard RSA uses:

```
n = p × q
```

But the hint suggests that **n is composed of more than two primes**.

Using factorization tools (e.g., factordb), we get:

```
n = p1 × p2 × p3 × p4
```

Where:

```
p1 = 9671406556917033397931773
p2 = 9671406556917033398314601
p3 = 9671406556917033398439721
p4 = 9671406556917033398454847
```

---

## 🔓 Exploitation

For multi-prime RSA:

```
φ(n) = (p1−1)(p2−1)(p3−1)(p4−1)
```

Then compute the private key:

```
d = e⁻¹ mod φ(n)
```

Finally decrypt:

```
m = ct^d mod n
```

---

## ⚙️ Solution Script

```python
n = 8749002899132047699790752490331099938058737706735201354674975134719667510377522805717156720453193651
e = 65537
ct = 3891158515405030211396309867177046660195995913985068178988858029936868358096672572274111514200511662

p1 = 9671406556917033397931773
p2 = 9671406556917033398314601
p3 = 9671406556917033398439721
p4 = 9671406556917033398454847

phi = (p1-1)*(p2-1)*(p3-1)*(p4-1)
d = pow(e, -1, phi)

m = pow(ct, d, n)
flag = m.to_bytes((m.bit_length()+7)//8, 'big')

print(flag)
```

---

## 🏁 Flag

```
picoCTF{mul71_rsa_0f2cedbf}
```

---

## 💡 Key Takeaways

* RSA security depends on difficulty of factoring `n`
* Using **multiple primes** (especially small ones) weakens security
* Multi-prime RSA still follows the same math — just extend φ(n)
* Always check factorization when RSA looks suspicious

---

## 🚀 Tools Used

* factordb.com (factorization)
* Python (modular arithmetic)

---

## ✅ Conclusion

The challenge demonstrates that improper RSA implementation (using multiple primes) can make factorization feasible, leading to full decryption of the ciphertext.

---
