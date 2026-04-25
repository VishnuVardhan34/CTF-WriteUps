# Challenge: Forensics git 2

## Description:
Author: LT 'syreal' Jones

Description
The agents interrupted the perpetrator's disk deletion routine. Can you recover this git repo?
Download the disk image here.

**Hints:**
*We think the deletion was interrupted before any git objects were touched*

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

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF$ tsk_recover -e -o 0001140736 disk.img exc_files
Files Recovered: 5629

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF$ cd exc_files

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/exc_files$ ls
bin  etc  home  lib  root  sbin  usr  var

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/exc_files$ cd home

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/exc_files/home$ ls
ctf-player

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/exc_files/home$ cd ctf-player

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/exc_files/home/ctf-player$ ls
Code

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/exc_files/home/ctf-player$ cd Code

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/exc_files/home/ctf-player/Code$ cd killer-chat-app

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/exc_files/home/ctf-player/Code/killer-chat-app$ ls
client  logs  server

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/exc_files/home/ctf-player/Code/killer-chat-app$ git status
fatal: not a git repository (or any parent up to mount point /mnt)
Stopping at filesystem boundary (GIT_DISCOVERY_ACROSS_FILESYSTEM not set).

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/exc_files/home/ctf-player/Code/killer-chat-app$ ls -la
total 0
drwxrwxrwx 1 pepsi-man pepsi-man 512 Apr 25 20:09 .
drwxrwxrwx 1 pepsi-man pepsi-man 512 Apr 25 20:09 ..
-rwxrwxrwx 1 pepsi-man pepsi-man  25 Apr 25 20:09 client
drwxrwxrwx 1 pepsi-man pepsi-man 512 Apr 25 20:09 .git
drwxrwxrwx 1 pepsi-man pepsi-man 512 Apr 25 20:09 logs
-rwxrwxrwx 1 pepsi-man pepsi-man  25 Apr 25 20:09 server

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/exc_files/home/ctf-player/Code/killer-chat-app$ ls -la .git
total 0
drwxrwxrwx 1 pepsi-man pepsi-man 512 Apr 25 20:09 .
drwxrwxrwx 1 pepsi-man pepsi-man 512 Apr 25 20:09 ..
-rwxrwxrwx 1 pepsi-man pepsi-man  20 Apr 25 20:09 COMMIT_EDITMSG
-rwxrwxrwx 1 pepsi-man pepsi-man 150 Apr 25 20:09 config
-rwxrwxrwx 1 pepsi-man pepsi-man  73 Apr 25 20:09 description
-rwxrwxrwx 1 pepsi-man pepsi-man  23 Apr 25 20:09 HEAD
drwxrwxrwx 1 pepsi-man pepsi-man 512 Apr 25 20:09 hooks
-rwxrwxrwx 1 pepsi-man pepsi-man 478 Apr 25 20:09 index
drwxrwxrwx 1 pepsi-man pepsi-man 512 Apr 25 20:09 info
drwxrwxrwx 1 pepsi-man pepsi-man 512 Apr 25 20:09 logs
drwxrwxrwx 1 pepsi-man pepsi-man 512 Apr 25 20:09 objects

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/exc_files/home/ctf-player/Code/killer-chat-app$ git head
git: 'head' is not a git command. See 'git --help'.

The most similar command is
        help

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/exc_files/home/ctf-player/Code/killer-chat-app$ git Snow HEAD
git: 'Snow' is not a git command. See 'git --help'.

The most similar command is
        show

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/exc_files/home/ctf-player/Code/killer-chat-app$ git Show HEAD
git: 'Show' is not a git command. See 'git --help'.

The most similar command is
        show

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/exc_files/home/ctf-player/Code/killer-chat-app$ git show HEAD
fatal: not a git repository (or any parent up to mount point /mnt)
Stopping at filesystem boundary (GIT_DISCOVERY_ACROSS_FILESYSTEM not set).

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/exc_files/home/ctf-player/Code/killer-chat-app$ mkdir .git/refs/heads
mkdir: cannot create directory ‘.git/refs/heads’: No such file or directory

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/exc_files/home/ctf-player/Code/killer-chat-app$ mkdir -p .git/refs/heads

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/exc_files/home/ctf-player/Code/killer-chat-app$ mkdir -p .git/refs/tags

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/exc_files/home/ctf-player/Code/killer-chat-app$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   client
        new file:   logs/1.txt
        new file:   logs/2.txt
        new file:   logs/4.txt
        new file:   server

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   logs/1.txt
        modified:   logs/2.txt
        modified:   logs/4.txt


pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/exc_files/home/ctf-player/Code/killer-chat-app$ find .git/objects/ -type f | grep -v "pack" | awk -F '/' '{print $(NF-1)$NF}' | xargs -I {} sh -c 'if [ "$(git cat-file -t {})" = "commit" ]; then echo "--- Commit: {} ---"; git cat-file -p {}; fi'
--- Commit: 01533f718556a0e59f1467dae4fa462eed82c2a1 ---
tree c931ae0868411e5f23656a2436e78a4c4699e18c
parent 2151ef0ccc15aed1ab88e1afdc7484aaeff211c4
author ctf-player <ctf-player@example.com> 1763549240 +0000
committer ctf-player <ctf-player@example.com> 1763549240 +0000

Add random chat log
--- Commit: 2151ef0ccc15aed1ab88e1afdc7484aaeff211c4 ---
tree 6bf83de540f7d12cc3b683a83d69432e03d84509
parent e80b38b3322a5ba32ac07076ef5eeb4a59449875
author ctf-player <ctf-player@example.com> 1763549240 +0000
committer ctf-player <ctf-player@example.com> 1763549240 +0000

Remove secret hideout log
--- Commit: 26b809e0c41d8421f1126ed3a4eb06ad66e6d90a ---
tree 201c707b43219a63c1d3499b29c7d539af079861
parent 2c0a9b2b15dce92f800393d5030c7454efc278ae
author ctf-player <ctf-player@example.com> 1763549240 +0000
committer ctf-player <ctf-player@example.com> 1763549240 +0000

Add video game chat log
--- Commit: 2c0a9b2b15dce92f800393d5030c7454efc278ae ---
tree 5eb896e3ccd51175f66480cdb247fc45f3e8ac2d
author ctf-player <ctf-player@example.com> 1763549240 +0000
committer ctf-player <ctf-player@example.com> 1763549240 +0000

Add netcat scripts
--- Commit: 5827632e046a80a1e0d7b4fc5c7800dd539baeaf ---
tree 6bf83de540f7d12cc3b683a83d69432e03d84509
parent 26b809e0c41d8421f1126ed3a4eb06ad66e6d90a
author ctf-player <ctf-player@example.com> 1763549240 +0000
committer ctf-player <ctf-player@example.com> 1763549240 +0000

Add TV show chat log
--- Commit: e80b38b3322a5ba32ac07076ef5eeb4a59449875 ---
tree ead27e2bd5a0fc22868ffb629a768f82dfcda11c
parent 5827632e046a80a1e0d7b4fc5c7800dd539baeaf
author ctf-player <ctf-player@example.com> 1763549240 +0000
committer ctf-player <ctf-player@example.com> 1763549240 +0000

Add secret hideout chat log

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/exc_files/home/ctf-player/Code/killer-chat-app$ git ls-tree -r e80b38b3322a5ba32ac07076ef5eeb4a59449875
100755 blob d7b4a371ebd23e682ffebc7ec355690fdc94fbd1    client
100644 blob aa1cc01687b4ec94faf9916c3fc6efd83f23b816    logs/1.txt
100644 blob f150f0b963ab3ee95ba5656212abd76d7f2fed2e    logs/2.txt
100644 blob 7178644433e7cb6da3adf028f1c80d382a18e7b6    logs/3.txt
100755 blob 71fd2fafcd5ebd62fbf857769c92a91225ab3954    server

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF/exc_files/home/ctf-player/Code/killer-chat-app$ git show e80b38b3322a5ba32ac07076ef5eeb4a5944987
commit e80b38b3322a5ba32ac07076ef5eeb4a59449875
Author: ctf-player <ctf-player@example.com>
Date:   Wed Nov 19 10:47:20 2025 +0000

    Add secret hideout chat log

diff --git a/logs/3.txt b/logs/3.txt
new file mode 100644
index 0000000..7178644
--- /dev/null
+++ b/logs/3.txt
@@ -0,0 +1,3 @@
+Rex: Meet at the old arcade basement for the secret hideout.
+Jay: Ask Rusty at the door and use password picoCTF{g17_r35cu3_16ac6bf3}.
+Rex: Bring the decoder map so we can plan the route.
```

### Final Flag:
 picoCTF{g17_r35cu3_16ac6bf3}

