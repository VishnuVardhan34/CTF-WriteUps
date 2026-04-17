# 🏁 picoCTF Writeup: bytemancy 1

## 📌 Challenge Overview

Author: LT 'syreal' Jones

Description
Can you conjure the right bytes? The program's source code can be downloaded here. Additional details will be available after launching your challenge instance.

💡 **Hint:** *No copy-pasta, please - use Python!*

## 🔎 Step 1: Analyze the code given

The code reveals that the server is expecting the character 'e' 1751 times so, we must craft the python output and pass that to the server input

```bash
python3 -c "print('e'*1751)" | nc foggy-cliff.picoctf.net 64694

```bash
Result:
⊹──────[ BYTEMANCY-1 ]──────⊹
☍⟐☉⟊☽☈⟁⧋⟡☍⟐☉⟊☽☈⟁⧋⟡☍⟐☉⟊☽☈⟁⧋⟡☍⟐

Send me ASCII DECIMAL 101 1751 times, side-by-side, no space.

☍⟐☉⟊☽☈⟁⧋⟡☍⟐☉⟊☽☈⟁⧋⟡☍⟐☉⟊☽☈⟁⧋⟡☍⟐
⊹─────────────⟡─────────────⊹
==> picoCTF{h0w_m4ny_e's???_7dbc095c}

🚩 Final Flag
picoCTF{h0w_m4ny_e's???_7dbc095c}