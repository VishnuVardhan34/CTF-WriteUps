# 🏁 picoCTF Writeup: Printer Shares

## 📌 Challenge Overview

Author: Janice He

Description
Oops! Someone accidentally sent an important file to a network printer—can you retrieve it from the print server?
Additional details will be available after launching your challenge instance.

💡 **Hint:** *smbclient and smbutil are good tools*

## 🔎 Step 1: Analyze the connection that has been provided and folooe through

```bash
 smbclient -L \\mysterious-sea.picoctf.net -p 61828 -N

        Sharename       Type      Comment
        ---------       ----      -------
        shares          Disk      Public Share With Guests
        IPC$            IPC       IPC Service (Samba 4.19.5-Ubuntu)
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to mysterious-sea.picoctf.net failed (Error NT_STATUS_CONNECTION_REFUSED)
Unable to connect with SMB1 -- no workgroup available

## 🔎 Step 2: Connect to the public Shares
smbclient -L //mysterious-sez.picoctf.net/shares -p 61828 -N

```bash
smb: \> ls
  .                                   D        0  Sat Mar  7 01:55:46 2026
  ..                                  D        0  Sat Mar  7 01:55:46 2026
  dummy.txt                           N     1142  Thu Feb  5 02:52:17 2026
  flag.txt                            N       37  Sat Mar  7 01:55:46 2026


🚩 Final Flag
picoCTF{5mb_pr1nter_5h4re5_78a2840d}