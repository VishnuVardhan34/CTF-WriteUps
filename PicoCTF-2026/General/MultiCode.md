# 🏁 picoCTF Writeup: MultiCode

## 📌 Challenge Overview

Author: Yahaya Meddy

Description
We intercepted a suspiciously encoded message, but it’s clearly hiding a flag. No encryption, just multiple layers of obfuscation. Can you peel back the layers and reveal the truth?
Download the message.

💡 **Hint:** *A tool like CyberChef can be interesting.*
              *The flag has been wrapped in several layers of common encodings such as ROT13, URL encoding, Hex, and Base64. Can you figure out the order to peel them back?*

## 🔎 Step: Analyze the text message(Let the Cyberchef do the work)

NjM3NjcwNjI1MDQ3NTMyNTM3NDI2MTcyNjY2NzcyNzE1ZjcyNjE3MDMwNzE3NjYxNzQ1ZjM4MzQzODZlMzQzNjM2NmYyNTM3NDQ=

Clearly it is base64 encoded

cvpbPGS%7Barfgrq_rap0qvat_848n466o%7D

It is URL Encoded

cvpbPGS{arfgrq_rap0qvat_848n466o}

### ROT13

picoCTF{nested_enc0ding_848a466b}

🚩 Final Flag
picoCTF{nested_enc0ding_848a466b}