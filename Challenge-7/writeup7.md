#CTF Write-up 7 (CRYPTO)
date: 12/02/2026

##Challenge Description
Shattered Trust
The file you’re given holds the rules of the game. If you can’t unlock it, you’re not ready for what’s inside.

##Analysis and Approach

1. the description part 
I entered a directory Shatterd_Trust, and cd description to the folder, though the folder itself was read-only (dr-xr-xr-x). Inside, I found description.txt.enc and a very large password.txt.
I tried to read password.txt and saw it was a massive string of hex numbers: 56 6d 30 77.... Converting these to ASCII revealed a string starting with Vm0w, which is actually for multiple layers of Base64 encoding.
I wrote a Python script to recursively decode the string. After 32 layers of Base64 decoding, I finally found the plaintext key: lollipop.

Python Script:
python3 -c 'print(bytes.fromhex(open("password.txt", "r").read().strip()).decode())'

python3 -c '
import base64


with open("password.txt", "r") as f:
    data = bytes.fromhex(f.read().strip()).decode()

for i in range(1, 51):
    try:
        data = base64.b64decode(data).decode()
        print(f"Layer {i}: {data[:50]}...")
    except Exception:
        print(f"\nFinal Result found at layer {i-1}:")
        print(data)
        break
'

I found the key lolipop at layer 32
Using the key lollipop, I used OpenSSL to decrypt the description file: openssl enc -d -aes-256-cbc -pbkdf2 -in description.txt.enc -out description.txt -k lollipop

This file explained that the final flag is split into 4 parts in the format flag{part1_part2_part3_part4}.

2. Stage1
I moved into the stage1 directory and found cipher.bin and server.py.
I checked server.py and saw it was using AES-CBC with a hardcoded key:
b"\xd3C\x08'\x0f\xd5\xa7\x0b\xbf\x9e\xae\xc5\xa8|\x83w"
The script had a decrypt function, but it only returned True or False based on whether the padding was correct.

I tried to run a decryption script but ran into a ModuleNotFoundError because Crypto (PyCryptodome) wasn't installed. I fixed this with: pip install pycryptodome

Since I had write permissions in this folder, I edited server.py to actually print the decrypted data instead of just checking the padding.
final code:
" from Crypto.Cipher import AES
from Crypto.Util.Padding import unpad

key = b"\xd3C\x08'\x0f\xd5\xa7\x0b\xbf\x9e\xae\xc5\xa8|\x83w"

with open("cipher.bin", "rb") as f:
    ciphertext = f.read()

iv = ciphertext[:16]
data = ciphertext[16:]

cipher = AES.new(key, AES.MODE_CBC, iv)
decrypted = unpad(cipher.decrypt(data), 16)
print(decrypted.decode()) "

Running the modified script gave me the first part of the flag.
Found Part 1: oracle_master 

3. Stage2


##Tools/Commands Used
python3, pycryptodome, openssl

##The Final Flag
	flag{oracle_master_weak_primes_fail_b64f4ff6cee4a82a49835a41a5fabac8cfbd3fe25561d3ab3769beea9c1d71cd_time_is_the_key}

