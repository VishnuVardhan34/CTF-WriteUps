# 🧙 BYTEMANCY-2 Writeup

## 📌 Challenge Info
- **Category:** Binary Exploitation / Input Handling
- **Difficulty:** Easy
- **CTF:** picoCTF

---

## 🧩 Challenge Description

We are given a remote service that prompts:

> **"Send me the HEX BYTE 0xFF 3 times, side-by-side, no space."**

---

## 🔍 Code Analysis

Relevant snippet:

```python
user_input = sys.stdin.buffer.readline().rstrip(b"\n")

if user_input == b"\xff\xff\xff":
    print(open("./flag.txt", "r").read())
```

### Key Observations

- Input is taken using `sys.stdin.buffer.readline()` → **reads raw bytes**
- The condition checks against:
  ```python
  b"\xff\xff\xff"
  ```
- This means:
  - Exactly **3 bytes**
  - Each byte = `0xFF`
- `.rstrip(b"\n")` removes the trailing newline

---

## ❌ Initial Mistake

Tried sending:

```bash
\xff\xff\xff
```

Output:

```
That wasn't it. I got: b'\\xff\\xff\\xff'
```

### Why this failed

- This sends **literal ASCII characters**, not raw bytes
- Python interprets it as:
  ```python
  b'\\xff\\xff\\xff'
  ```
- Which does **not match** `b"\xff\xff\xff"`

---

## 🧠 Correct Approach

We must send **actual binary bytes**, not text.

Also, since the program uses `readline()`, it waits for a **newline (`\n`)** before processing input.

---

## 🚀 Exploitation

### Working Payload

```bash
printf '\xff\xff\xff\n' | nc lonely-island.picoctf.net 61497
```

### Why it works

- `\xff\xff\xff` → sends correct raw bytes
- `\n` → satisfies `readline()`
- `.rstrip()` removes newline → exact match

---

## 🏁 Flag

```
picoCTF{3ff5_4_d4yz_f56ee8d7}
```
