```python
pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF$ gunzip disk.img.gz

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF$ file disk.img
disk.img: DOS/MBR boot sector; partition 1 : ID=0x83, active, start-CHS (0x2,0,33), end-CHS (0x263,8,56), startsector 2048, 614400 sectors; partition 2 : ID=0x82, start-CHS (0x263,8,57), end-CHS (0x3ff,15,63), startsector 616448, 524288 sectors; partition 3 : ID=0x83, start-CHS (0x3ff,15,63), end-CHS (0x3ff,15,63), startsector 1140736, 956416 sectors

epsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF$ exiftool disk.img
ExifTool Version Number         : 13.50
File Name                       : disk.img
Directory                       : .
File Size                       : 1074 MB
File Modification Date/Time     : 2026:04:24 17:44:33+05:30
File Access Date/Time           : 2026:04:24 17:45:22+05:30
File Inode Change Date/Time     : 2026:04:24 17:45:41+05:30
File Permissions                : -rwxrwxrwx
Error                           : Unknown file type

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF$ xxd disk.img | head
00000000: 33c0 fa8e d88e d0bc 007c 89e6 0657 8ec0  3........|...W..
00000010: fbfc bf00 06b9 0001 f3a5 ea1f 0600 0052  ...............R
00000020: 52b4 41bb aa55 31c9 30f6 f9cd 1372 1381  R.A..U1.0....r..
00000030: fb55 aa75 0dd1 e973 0966 c706 8d06 b442  .U.u...s.f.....B
00000040: eb15 5ab4 08cd 1383 e13f 510f b6c6 40f7  ..Z......?Q...@.
00000050: e152 5066 31c0 6699 e866 00e8 3501 4d69  .RPf1.f..f..5.Mi
00000060: 7373 696e 6720 6f70 6572 6174 696e 6720  ssing operating
00000070: 7379 7374 656d 2e0d 0a66 6066 31d2 bb00  system...f`f1...
00000080: 7c66 5266 5006 536a 016a 1089 e666 f736  |fRfP.Sj.j...f.6
00000090: f47b c0e4 0688 e188 c592 f636 f87b 88c6  .{.........6.{..

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF$ fdisk -l disk.img
Disk disk.img: 1 GiB, 1073741824 bytes, 2097152 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x610b63c2

Device     Boot   Start     End Sectors  Size Id Type
disk.img1  *       2048  616447  614400  300M 83 Linux
disk.img2        616448 1140735  524288  256M 82 Linux swap / Solaris
disk.img3       1140736 2097151  956416  467M 83 Linux

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF$ mkdir ~/mnt3

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF$ sudo mount -o loop,offset=583008256 disk.img ~/mnt3
mount: /home/pepsi-man/mnt3: wrong fs type, bad option, bad superblock on /dev/loop0, missing codepage or helper program, or other error.
       dmesg(1) may have more information after failed mount system call.

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF$ file -s disk.img
disk.img: DOS/MBR boot sector; partition 1 : ID=0x83, active, start-CHS (0x2,0,33), end-CHS (0x263,8,56), startsector 2048, 614400 sectors; partition 2 : ID=0x82, start-CHS (0x263,8,57), end-CHS (0x3ff,15,63), startsector 616448, 524288 sectors; partition 3 : ID=0x83, start-CHS (0x3ff,15,63), end-CHS (0x3ff,15,63), startsector 1140736, 956416 sectors

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF$ sudo losetup -Pf disk.img
ls /dev/loop0*
/dev/loop0  /dev/loop0p1  /dev/loop0p2  /dev/loop0p3

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF$ sudo file -s /dev/loop0p3
/dev/loop0p3: Linux rev 1.0 ext4 filesystem data, UUID=7a00e9da-98f8-4f0f-b257-95edf422d902 (extents) (64bit) (large files) (huge files)

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF$ sudo mount /dev/loop0p3 ~/mnt3

pepsi-man@CoachPotato:/mnt/d/ctf/.dump/Pico-CTF$ cd ~/mnt3

pepsi-man@CoachPotato:~/mnt3$ ls
bin  boot  dev  etc  home  lib  lost+found  media  mnt  opt  proc  root  run  sbin  srv  swap  sys  tmp  usr  var

pepsi-man@CoachPotato:~/mnt3$ ls -la
total 45
drwxr-xr-x 22 root      root       1024 Nov 19 14:08 .
drwx------ 18 pepsi-man pepsi-man  4096 Apr 24 17:51 ..
drwxr-xr-x  2 root      root       4096 Nov 19 14:09 bin
drwxr-xr-x  2 root      root       1024 Nov 12 01:31 boot
drwxr-xr-x  2 root      root       1024 Nov 12 01:31 dev
drwxr-xr-x 30 root      root       3072 Nov 19 14:09 etc
drwxr-xr-x  3 root      root       1024 Nov 12 01:31 home
drwxr-xr-x 10 root      root       1024 Nov 12 01:31 lib
drwx------  2 root      root      12288 Nov 12 01:31 lost+found
drwxr-xr-x  5 root      root       1024 Nov 12 01:31 media
drwxr-xr-x  2 root      root       1024 Nov 12 01:31 mnt
drwxr-xr-x  2 root      root       1024 Nov 12 01:31 opt
drwxr-xr-x  2 root      root       1024 Nov 12 01:31 proc
drwx------  2 root      root       1024 Nov 19 14:09 root
drwxr-xr-x  2 root      root       1024 Nov 12 01:31 run
drwxr-xr-x  2 root      root       5120 Nov 12 01:31 sbin
drwxr-xr-x  2 root      root       1024 Nov 12 01:31 srv
drwxr-xr-x  2 root      root       1024 Nov 19 14:08 swap
drwxr-xr-x  2 root      root       1024 Nov 12 01:31 sys
drwxrwxrwt  2 root      root       1024 Nov 12 01:31 tmp
drwxr-xr-x  8 root      root       1024 Nov 12 01:31 usr
drwxr-xr-x 11 root      root       1024 Nov 19 14:08 var

pepsi-man@CoachPotato:~/mnt3$ cd tmp

pepsi-man@CoachPotato:~/mnt3/tmp$ ls

pepsi-man@CoachPotato:~/mnt3/tmp$ ls -la
total 2
drwxrwxrwt  2 root root 1024 Nov 12 01:31 .
drwxr-xr-x 22 root root 1024 Nov 19 14:08 ..

pepsi-man@CoachPotato:~/mnt3/tmp$ cd home
cd: no such file or directory: home

pepsi-man@CoachPotato:~/mnt3/tmp$ ls

pepsi-man@CoachPotato:~/mnt3/tmp$ cd ..

pepsi-man@CoachPotato:~/mnt3$ cd home

pepsi-man@CoachPotato:~/mnt3/home$ ls
ctf-player

pepsi-man@CoachPotato:~/mnt3/home$ cd ctf-player

pepsi-man@CoachPotato:~/mnt3/home/ctf-player$ ls
Code

pepsi-man@CoachPotato:~/mnt3/home/ctf-player$ cd Code

pepsi-man@CoachPotato:~/mnt3/home/ctf-player/Code$ ls
secrets

pepsi-man@CoachPotato:~/mnt3/home/ctf-player/Code$ cd secrets

pepsi-man@CoachPotato:~/mnt3/home/ctf-player/Code/secrets$ ls
note.txt

pepsi-man@CoachPotato:~/mnt3/home/ctf-player/Code/secrets$ cat note.txt
The picoCTF flag format is 'picoCTF{}' where there is some leetspeak phrase in between the curly braces

pepsi-man@CoachPotato:~/mnt3/home/ctf-player/Code/secrets$ git cd ../
cdgit: 'cd' is not a git command. See 'git --help'.

The most similar command is
        add

pepsi-man@CoachPotato:~/mnt3/home/ctf-player/Code/secrets$ cd ..

pepsi-man@CoachPotato:~/mnt3/home/ctf-player/Code$ ls
secrets

pepsi-man@CoachPotato:~/mnt3/home/ctf-player/Code$ ls -la
total 3
drwxr-sr-x 3 pepsi-man pepsi-man 1024 Nov 19 14:19 .
drwxr-sr-x 3 pepsi-man pepsi-man 1024 Nov 19 14:19 ..
drwxr-sr-x 3 pepsi-man pepsi-man 1024 Nov 19 14:19 secrets

pepsi-man@CoachPotato:~/mnt3/home/ctf-player/Code$ cd secrets

pepsi-man@CoachPotato:~/mnt3/home/ctf-player/Code/secrets$ ;s
s: command not found

pepsi-man@CoachPotato:~/mnt3/home/ctf-player/Code/secrets$ ls
note.txt

pepsi-man@CoachPotato:~/mnt3/home/ctf-player/Code/secrets$ ls -la
total 4
drwxr-sr-x 3 pepsi-man pepsi-man 1024 Nov 19 14:19 .
drwxr-sr-x 3 pepsi-man pepsi-man 1024 Nov 19 14:19 ..
drwxr-sr-x 8 pepsi-man pepsi-man 1024 Nov 19 14:19 .git
-rw-r--r-- 1 pepsi-man pepsi-man  104 Nov 19 14:19 note.txt

pepsi-man@CoachPotato:~/mnt3/home/ctf-player/Code/secrets$ git log --online
fatal: unrecognized argument: --online

pepsi-man@CoachPotato:~/mnt3/home/ctf-player/Code/secrets$ git log --oneline
327681b (HEAD -> master) Wrap this phrase in the flag format: g17_1n_7h3_d15k_041217d8
```

Final Flag:
picoCTF{g17_1n_7h3_d15k_041217d8}
