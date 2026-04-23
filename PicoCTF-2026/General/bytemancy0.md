# 🏁 picoCTF Writeup: bytemancy 0

## 📌 Challenge Overview

Author: LT 'syreal' Jones

Description
Can you conjure the right bytes? The program's source code can be downloaded here.
Additional details will be available after launching your challenge instance.

💡 **Hint:** *Solving this with a one-liner will help with the next challenge in this series*

## 🔎 Step 1: Analyze the code given

The code reveals that the server is expecting the character 'e' 3 times so, we must craft the python output and pass that to the server input

```bash
python3 -c "print('e'*3)" | nc foggy-cliff.picoctf.net 64694

```bash
Result:
⊹──────[ BYTEMANCY-1 ]──────⊹
☍⟐☉⟊☽☈⟁⧋⟡☍⟐☉⟊☽☈⟁⧋⟡☍⟐☉⟊☽☈⟁⧋⟡☍⟐

Send me ASCII DECIMAL 101 1751 times, side-by-side, no space.

☍⟐☉⟊☽☈⟁⧋⟡☍⟐☉⟊☽☈⟁⧋⟡☍⟐☉⟊☽☈⟁⧋⟡☍⟐
⊹─────────────⟡─────────────⊹
==> picoCTF{pr1n74813_ch4r5_2f7a75e5}

🚩 Final Flag
 picoCTF{pr1n74813_ch4r5_2f7a75e5}