# DISKO 4 - PicoCTF Writeup

## Challenge Overview

**Name:** DISKO 4
**Author:** Darkraicg492

**Description:**
A disk image is provided where the flag file has been deleted. The goal is to recover the deleted file and extract the flag.

**Hint:**

> How would you look for deleted files?

---

## Approach

Since the challenge explicitly mentions that the file was *deleted*, the solution involves **disk forensics** techniques to recover deleted files.

We use tools from **The Sleuth Kit (TSK)**:

* `fls` → to list files (including deleted ones)
* `icat` → to extract file contents using inode

---

## Step 1: Identify Deleted Files

We recursively list deleted files using:

```bash
fls -r -d disko-4.dd
```

### Output:

```
r/r * 522629:   log/messages
r/r * 532021:   log/dont-delete.gz
```

* `*` indicates **deleted files**
* The file `dont-delete.gz` looks interesting

---

## Step 2: Extract the Deleted File

Use `icat` with the inode number:

```bash
icat disko-4.dd 532021 > dont-delete.gz
```

---

## Step 3: Decompress the File

Since it's a `.gz` file:

```bash
gunzip dont-delete.gz
```

---

## Step 4: Read the Recovered File

```bash
cat dont-delete
```

### Output:

```
Here is your flag
picoCTF{d3l_d0n7_h1d3_w3ll_3da42114}
```

---

## Final Flag

```
picoCTF{d3l_d0n7_h1d3_w3ll_3da42114}
```

---

## Key Takeaways

* Deleted files are not immediately erased from disk — only their references are removed.
* Tools like `fls` and `icat` can recover such files using metadata (inodes).
* Always check compressed or hidden files when doing forensic analysis.

---

## Tools Used

* Sleuth Kit (`fls`, `icat`)
* `gunzip`

---

## Conclusion

This challenge demonstrates basic **digital forensics** concepts, specifically recovering deleted files from a disk image. Even when files are deleted, their data can often still be retrieved if not overwritten.

---
