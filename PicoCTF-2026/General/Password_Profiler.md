# 🏁 picoCTF Writeup: Password Profiler

## 📌 Challenge Overview

Author: Yahaya Meddy

Description
We intercepted a suspicious file from a system, but instead of the password itself, it only contains its SHA-1 hash. Using OSINT techniques, you are provided with personal details about the target. Your task is to leverage this information to generate a custom password list and recover the original password by matching its hash.
Download the following files:
userinfo: Contains the personal details.
hash: Contains the SHA-1 hash of the password.
check_password: Script to test passwords against the hash.

💡 **Hint:** *CUPP is a Python tool for generating custom wordlists from personal data.*

## 🔎 Walkthrough: git clone the mentioned repo in the Hint

``` python
python3 cupp.py -i
 ___________
   cupp.py!                 # Common
      \                     # User
       \   ,__,             # Passwords
        \  (oo)____         # Profiler
           (__)    )\
              ||--|| *      [ Muris Kurgas | j0rgan@remote-exploit.org ]
                            [ Mebus | https://github.com/Mebus/]


[+] Insert the information about the victim to make a dictionary
[+] If you don't know all the info, just hit enter when asked! ;)

> First Name: Alice
> Surname: Johnson
> Nickname: AJ
> Birthdate (DDMMYYYY): 15071990


> Partners) name: Bob
> Partners) nickname:
> Partners) birthdate (DDMMYYYY):


> Child's name: Charlie
> Child's nickname:
> Child's birthdate (DDMMYYYY):


> Pet's name:
> Company name:


> Do you want to add some key words about the victim? Y/[N]:
> Do you want to add special chars at the end of words? Y/[N]:
> Do you want to add some random numbers at the end of words? Y/[N]:
> Leet mode? (i.e. leet = 1337) Y/[N]:

[+] Now making a dictionary...
[+] Sorting list and removing duplicates...
[+] Saving dictionary to alice.txt, counting 5180 words.
> Hyperspeed Print? (Y/n) :
[+] Now load your pistolero with alice.txt and shoot! Good luck!

``` bash
python3 check_password.py
Password found: picoCTF{Aj_15901990}

🚩 Final Flag
picoCTF{Aj_15901990}