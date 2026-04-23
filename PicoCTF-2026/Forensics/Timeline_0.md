# 🏁 picoCTF Writeup: Timeline0

## 📌 Challenge Overview

Author: LT 'syreal' Jones

Description
Can you find the flag in this disk image? Wrap what you find in the picoCTF flag format.
Download the disk image here.

## 🧠 Approach

```python 
gzip -d partition4.img.gz
```

```python
nix-shell -p sleuthkit
```

USed the tool as per Hint

```python
fls -r -m "/" partition4.img > body.txt
```

```python
mactime -b body.txt > time.csv
mactime -b body.txt > hello
head hello
icat partition4.img 4945
```

```
NzFtMzExbjNfMHU3MTEzcl9oM3JfNDNhMmU3YWYK
```
Decode with CyberChef

## 🚩 Flag

```
picoCTF{71m311n3_0u7113r_h3r_43a2e7af}
```