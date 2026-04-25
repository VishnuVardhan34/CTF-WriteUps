# Forensics Git 1

## Challenge Overview
Author: LT 'syreal' Jones

Description
Can you find the flag in this disk image?
Download the disk image here.

**Hints**
*How can you checkout the files of a previous commit?*

## Walkthrough
```
pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF$ gunzip disk.img.gz
gzip: disk.img already exists; do you wish to overwrite (y or n)? y

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF$ mmls disk.img
DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  000:000   0000002048   0000616447   0000614400   Linux (0x83)
003:  000:001   0000616448   0001140735   0000524288   Linux Swap / Solaris x86 (0x82)
004:  000:002   0001140736   0002097151   0000956416   Linux (0x83)

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF$ mkdir extracted_files

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF$ tsk_recover -e -o 0001140736 disk.img extracted_files
cd Files Recovered: 5609

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF$ cd extracted_files

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/extracted_files$ ls
bin  etc  home  lib  root  sbin  usr  var

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/extracted_files$ cd home

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/extracted_files/home$ ls
ctf-player

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/extracted_files/home$ cd ctf-player

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/extracted_files/home/ctf-player$ ls
Code

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/extracted_files/home/ctf-player$ cd Code

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/extracted_files/home/ctf-player/Code$ ls
secrets

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/extracted_files/home/ctf-player/Code$ cd secrets

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/extracted_files/home/ctf-player/Code/secrets$ ls

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/extracted_files/home/ctf-player/Code/secrets$ ls -la
total 0
drwxrwxrwx 1 pepsi-man pepsi-man 512 Apr 25 11:06 .
drwxrwxrwx 1 pepsi-man pepsi-man 512 Apr 25 11:06 ..
drwxrwxrwx 1 pepsi-man pepsi-man 512 Apr 25 11:06 .git

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/extracted_files/home/ctf-player/Code/secrets$ git log
commit 5fb8194539c770a830b8ba089a50778c07072b03 (HEAD -> master)
Author: ctf-player <ctf-player@example.com>
Date:   Wed Nov 19 09:20:05 2025 +0000

    Remove flag

commit 177789af0b300e043ea8f54ea57d6cee352291ae
Author: ctf-player <ctf-player@example.com>
Date:   Wed Nov 19 09:20:05 2025 +0000

    Add flag

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/extracted_files/home/ctf-player/Code/secrets$ git checkout 177789af0b300e043ea8f54ea57d6cee352291ae
Note: switching to '177789af0b300e043ea8f54ea57d6cee352291ae'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 177789a Add flag

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/extracted_files/home/ctf-player/Code/secrets$ ls
flag.txt

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/extracted_files/home/ctf-player/Code/secrets$ cat flag.txt
picoCTF{g17_r3m3mb3r5_d4ddf904}
```

## Final Flag:
```
picoCTF{g17_r3m3mb3r5_d4ddf904} 
```
