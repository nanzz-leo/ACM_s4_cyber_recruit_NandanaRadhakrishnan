#CTF Write-up 6 (REV+CRYPTO+OSINT)
date: 25/02/2026

##Challenge Description
Compiled While Intoxicated:
Someone once said, “This word was created by someone on morphine, cocaine, and alcohol at the same time.”
Absurd origin story? Maybe. But the word exists — and it’s hiding somewhere. A suspicious obsession with hex and some strange bases can be observed.
Follow the encodings instead of fighting them. The trail will eventually point you to something that exists outside this file.
Flag format: ACM{}

##Analysis and Approach

At first glance, this looked like a standard Java reverse engineering challenge since we were given a rock.class file.
I started with the usual tools:
strings rock.class AND javap -c rock.class

I found a strange string m0rph111n3 and 2 hex strings : 2d581b041a545 and 25e1c571e511f

The challenge description mentioned an obsession with hex and strange bases, so I combined both hex parts into: 2d581b041a54525e1c571e511f
 
I realised that m0rph111n3 looked like some key: then I wrote a small Python script to XOR the decoded hex bytes with this string as a repeating key.

code:

data = bytes.fromhex("2d581b041a54525e1c571e511f")
key = b"m0rph111n3"

result = bytes([data[i] ^ key[i % len(key)] for i in range(len(data))])
print(result)

The result: @hitrecordsam

Searching for @hitrecordsam led to an Instagram profile. 
Then searching: @hitrecordsam word was created by someone on morphine, cocaine, and alcohol , gave me a video link where Hochwasserschutzanlage Dovenfleet , was the sign on the german street word

then finally Hochwasserschutzanlage was the flag
##Tools/Commands Used
strings, javap, Python (for XOR decoding), Instagram

##The Final Flag
ACM{Hochwasserschutzanlage}



