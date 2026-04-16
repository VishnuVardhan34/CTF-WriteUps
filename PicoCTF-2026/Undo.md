# Challenge: Undo

Description
Can you reverse a series of Linux text transformations to recover the original flag?
Additional details will be available after launching your challenge instance.

Hint:
For text translation and character replacement, see tr command documentation.

Walkthrough
After connecting to the server we will recieve the text "KTJxNW85NjQ1LWZhMDFnQHplMHNmYTRlRy1nazNnLXRhMWZlcmlyRShTR1BicHZj" with a Hint to rev with base64.

echo ‘KTJxNW85NjQ1LWZhMDFnQHplMHNmYTRlRy1nazNnLXRhMWZlcmlyRChTR1BicHZj’ | base64 -d

Result:  )2q5o9645-fa01g@ze0sfa4eG-gk3g-ta1ferirE(SGPbpvc

Now reverse it

echo ‘)2q5o9645-fa01g@ze0sfa4eG-gk3g-ta1ferirE(SGPbpvc’ | rev

result: cvpbPGS(Eriref1at-g3kg-Ge4afs0ez@g10af-5469o5q2)

It is Rot13 Cipher so decrypt it and get the flag and don;t forget to change () -> {} (i.e fot that use the tr command, i did it manually)

Final Flag:
picoCTF{Revers1ng_t3xt_Tr4nsf0rm@t10ns_5469b5d2}
