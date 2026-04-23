# 🏁 picoCTF Writeup: SUDO MAKE ME A SANDWICH

## 📌 Challenge Overview

Description
Can you read the flag? I think you can!
Additional details will be available after launching your challenge instance.

💡 **Hint:** *How do you know what permission you have?*

## 🔎 Step 1: COnnect to the ssh connection

```bash
sudo -l

Reveals that User ctf-player may run the following commands on challenge: (ALL) NOPASSWD: /bin/emacs

Then 
```bash
sudo emacs flag.txt

🚩 Final Flag
picoCTF{ju57_5ud0_17_9418380d}