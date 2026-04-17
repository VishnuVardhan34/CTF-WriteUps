# picoCTF - BYTEMANCY 3 

## 🧩 Challenge Overview

We are given:

* A binary: `spellbook`
* A Python service: `app.py`
* Remote connection:

  ```
  nc green-hill.picoctf.net 60627
  ```

### Objective

The server asks for the **addresses of specific functions** inside the binary.
We must send them as **raw 4-byte little-endian values**.

---

## 🔍 Binary Analysis

We inspect the binary using:

```bash
objdump -t spellbook
```

From the symbol table, we identify the required functions:

| Function Name | Address    |
| ------------- | ---------- |
| ember_sigil   | 0x08049176 |
| glyph_conflux | 0x0804919a |
| astral_spark  | 0x080491c1 |
| binding_word  | 0x080491e3 |

---

## 🧠 Understanding the Challenge Logic

From `app.py`:

* The program randomly selects **3 functions**
* For each function:

  * It retrieves the address using `elf.symbols`
  * Converts it using `p32()` (little-endian)
  * Compares with user input

Key takeaway:

> We must send **exactly 4 raw bytes in little-endian format**

---

## 🔄 Address Conversion (Little Endian)

We convert each address:

| Function Name | Address    | Little Endian Bytes |
| ------------- | ---------- | ------------------- |
| ember_sigil   | 0x08049176 | `\x76\x91\x04\x08`  |
| glyph_conflux | 0x0804919a | `\x9a\x91\x04\x08`  |
| astral_spark  | 0x080491c1 | `\xc1\x91\x04\x08`  |
| binding_word  | 0x080491e3 | `\xe3\x91\x04\x08`  |

---

## ⚡ Exploit Script

### Using pwntools

```python
from pwn import *

HOST = "green-hill.picoctf.net"
PORT = 60627

elf = ELF('./spellbook')

mapping = {
    'ember_sigil': p32(elf.symbols['ember_sigil']),
    'glyph_conflux': p32(elf.symbols['glyph_conflux']),
    'astral_spark': p32(elf.symbols['astral_spark']),
    'binding_word': p32(elf.symbols['binding_word']),
}

p = remote(HOST, PORT)

for _ in range(3):
    line = p.recvline().decode()

    for name in mapping:
        if name in line:
            p.send(mapping[name])
            break

print(p.recvall().decode())
```

---

### Alternative (No pwntools)

```python
import socket
import struct

HOST = "green-hill.picoctf.net"
PORT = 60627

mapping = {
    'ember_sigil': struct.pack("<I", 0x08049176),
    'glyph_conflux': struct.pack("<I", 0x0804919a),
    'astral_spark': struct.pack("<I", 0x080491c1),
    'binding_word': struct.pack("<I", 0x080491e3),
}

s = socket.socket()
s.connect((HOST, PORT))

for _ in range(3):
    data = s.recv(1024).decode()

    for name in mapping:
        if name in data:
            s.send(mapping[name])
            break

print(s.recv(4096).decode())
s.close()
```

---

## ❗ Important Notes

* Input must be **raw bytes**, not strings
* Must send **exactly 4 bytes per query**
* Order is random → script must handle dynamically

---

## 🏁 Flag

```
picoCTF{0bjdump_m4g1c_de613af3}
```