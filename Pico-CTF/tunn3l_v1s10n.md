tunn3l v1s10n – picoCTF 2021 Writeup

Challenge Overview
We are given a corrupted file named tunn3l_v1s10n. The goal is to recover the flag from what appears to be a damaged BMP image. 

Initial Analysis
Running file tunn3l_v1s10n returns data, indicating an unknown format. However, inspecting the file with xxd or a hex editor reveals the magic bytes 42 4D, which identify it as a BMP (Bitmap) file. 

Despite this, the image fails to open — a sign of corrupted headers. 

Fixing the BMP Header
BMP files have two main headers:

File Header (14 bytes)
DIB Header (typically 40 bytes, 0x28 in hex) 
Using a hex editor like HexEd.it, we inspect and correct the following:

Field	Incorrect Value	Correct Value	Offset
DIB Header Size	BA D0 00 00 (53434)	28 00 00 00 (40)	0x0E
Pixel Array Offset	BA D0 00 00 (53434)	36 00 00 00 (54)	0x0A
Image Height	32 01 00 00 (306)	52 03 00 00 (850)	0x16

Why 850?
Total file size: 0x2C268E = 2,893,454 bytes
Header size: 54 bytes → Data size = 2,893,400 bytes
Width = 1134 pixels, 3 bytes per pixel → Height = 2,893,400 / (1134 × 3) = 850 

Result
After applying these fixes and saving the file as tunn3l_v1s10n.bmp, the image opens successfully and displays the flag. 

Flag
picoCTF{qu1t3_a_v13w_2020}