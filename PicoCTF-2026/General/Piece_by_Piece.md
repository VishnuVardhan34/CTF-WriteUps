# 🏁 picoCTF Writeup: Piece by Piece

## 📌 Challenge Overview

Author: Yahaya Meddy

Description
After logging in, you will find multiple file parts in your home directory. These parts need to be combined and extracted to reveal the flag.
Additional details will be available after launching your challenge instance.

💡 **Hint:** *None*

## 🔎 Step 1: Analyze the unzip error and the instructions.txt file

```bash
unzip part_aa
Archive:  part_aa
  End-of-central-directory signature not found.  Either this file is not
  a zipfile, or it constitutes one disk of a multi-part archive.  In the
  latter case the central directory and zipfile comment will be found on
  the last disk(s) of this archive.
unzip:  cannot find zipfile directory in one of part_aa or
        part_aa.zip, and cannot find part_aa.ZIP, period.
        
## 🔎 Step 2: Convert pieces into full part

```bash
cat part_* > full.zip
```bash
unzip full.zip

🚩 Final Flag
picoCTF{z1p_and_spl1t_f1l3s_4r3_fun_28d309dc}
