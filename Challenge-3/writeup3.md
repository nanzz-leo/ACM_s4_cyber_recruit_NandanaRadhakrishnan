#CTF Write-up 3 (MISC)
date: 12/02/2026

##Challenge Description
Letter from Hogwarts: I got the letter from hogwarts but it was password encrypted help me to find, lets try until we get the password the password for the pdf. In that pdf u can get the flag.

##Analysis and Approach
Reading the challenge I realised that I needed John the Ripper to crack the password hash. The core version of John doesn't support PDF cracking, so I had to download and compile the Jumbo version.
For using John the Ripper Jumbo I cloned the repository from GitHub and compiled the Jumbo version of John to enable support for PDF cracking.
commands used(took help from llm like chatgpt and also google) :
git clone https://github.com/openwall/john.git
cd john/src
./configure
make -s clean && make -sj4 

With the tools ready, I needed to extract the hash from the password protected PDF. I used pdf2john to extract the hash from the encrypted file. 
command used : python3 pdf2john.py Letter_from_Hogwarts.pdf > hash.txt

This saved the encrypted hash of the pdf file in hash.txt

Then as I searched web I found about the RockYou wordlist used commonly in CTF challenges and downloaded directly with this command:
wget https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt

Then I ran the rockyou wordlist:  ./john --wordlist=rockyou.txt --rules hash.txt

This command started cracking the password using the RockYou wordlist and John’s built-in rules to modify passwords

Then next command was to show the password : ./john --show hash.txt
the output was : 
?:bud121clam725
1 password hash cracked, 0 left

I tried this password to open the file but it didn't work so I used qpdf to decrypt the PDF file and extract the same contents.
ran the following command: qpdf --password=bud121clam725 --decrypt ~/Letter_from_Hogwarts.pdf unlocked.pdf

Then I opend unlocked.pdf witht he newly found command : evince ~/unlocked.pdf , this opened the PDF file and I received the letter with the flag in it.
At first I made mistakes in typing the flag , then it took me some time to figure out that its all '0' and not 'o'. 

##Tools/Commands Used
John the Ripper (Jumbo version), qpdf, pdf2john, RockYou wordlist, evince

##The Final Flag
potter{w3lc0m3_70_h0gw4r75}

