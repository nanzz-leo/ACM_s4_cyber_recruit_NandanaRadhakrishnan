#CTF Write-up 2 (OSINT)
date: 12/02/2026

##Challenge Description
Wand: 
I found the this wand. I don’t know whose wand is so i need to find it and also the flag is inside this image. Go on, you may find the hint.

##Analysis and Approach

I started by checking the file metadata since the challenge hinted that the data was hidden there. exiftool gave me a clue: "Full name of the wielder without spaces in small letters."

I thought of the Elder Wand as I saw the image. I tried "albusdumbledore" and "harryjamespotter," but nothing worked. I eventually Googled Dumbledore’s full name and it was  Albus Percival Wulfric Brian Dumbledore then as it was all small wuth no spaces , i got the massive string: albuspercivalwulfricbriandumbledore.

At first, I tried submitting that as the flag in the ACM{...} format, but it failed. That’s when I realized the name wasn't the flag—it was a password.

I installed steghide and ran the extraction command with the long name as the passphrase:
steghide extract -sf wand.jpeg -p albuspercivalwulfricbriandumbledore

It extracted a file called hide.txt. I just cat'ed that file, and the real flag was inside.

##Tools/Commands Used
exiftool,steghide ,cat

##The Final Flag
potter{1_570l3_7h47_3ld3r_w4nd}




