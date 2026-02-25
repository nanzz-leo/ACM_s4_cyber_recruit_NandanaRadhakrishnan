#CTF Write-up 4 (MISC)
date: 12/02/2026

##Challenge Description
This is for u know who
Someone from the Hogwarts send this file given below to the voldemort.I don’t know who it is and what is the information in it. What ever the information is but it will definitely harm harry so find the person who send it. The flag lies there.


##Analysis and Approach
I unzipped the file and extracted the contents. Then did ls and only saw one file even though , ls -l showed that there were 4 files.
Then I did ls -la and found a hidden directory named '.message to voldemort' and inside it there was a file called 'top secret.png'

I ran strings on the image and saw a "PK" header, indicating a ZIP file was hidden inside.
I extracted it using unzip 'top secret.png'

The extraction gave me a folder called why always my nose, containing a file called important.txt
the file had the content: Lord Voldemort for more security from these students i secured it in deathly hallows cloak.

Then when I did ls -la , I saw a directory called .secret
then did cd '.secret' and then ls 

Then i found a horcrux.jpg file and did strings in it but it was really large , then I did exiftool in it and found a line that said 
Artist                          : cG90dGVyezdoMTVfMTVfRDAxMHIzNV9VbThyMWRnM30=

This looked like a base64 encoded format to me and I used the command : echo "cG90dGVyezdoMTVfMTVfRDAxMHIzNV9VbThyMWRnM30=" | base64 --decode

Doing this decoded this string and gave me the final flag

##Tools/Commands Used
unzip, ls, ls -la, cd, strings, steghide, echo, base64, strings

##The Final Flag
potter{7h15_15_D010r35_Um8r1dg3}

