#CTF Write-up 5 (FORENSICS)
date: 12/02/2026

##Challenge Description
I need to Hack potter...
I captured professor quirrell network activity though capturing the packets. Help me to solve it.

##Analysis and Approach
I started by examining the challenge.pcap file using tcpdump, a command line network traffic analyzer.
initial command : tcpdump -r challenge.pcap
I tried reading the packet content as ASCII text using tcpdump -A, and while I could see some HTML code, it was really messy and hard to figure out exactly what was going on. : tcpdump -A -r challenge.pcap
Upon more research came accross wireshark
Wireshark allowed us to visually filter HTTP requests and responses, which helped pinpoint critical traffic patterns.
That’s when I found a request for http://127.0.0.1:5000/static/text/prophecy.txt.

I thought prophecy.txt was the flag, but it turned out to be a dead end—the file kept giving an error (404) and wasn't actually in the capture.

Since the text file was missing, I assumed the flag might be hidden in the images.
I used Wireshark’s automated extraction tool. and navigatedto: File > Export Objects > HTTP...
I saved all the image files to a local directory:
snape.jpg
harry.jpg
ron.jpg
hermoine.jpg
dumbledore.jpg

Then I just ran the strings command on all of them.
I actually found the second part of the flag first in hermoine.jpg. : w1tch3s_$nd_w1z4rds}
I kept checking the others and found the first part hidden in ron.jpg. : ACM{w3_4r3_cyb3r

##Tools/Commands Used
Wireshark, strings, curl, net-tools, tshark

##The Final Flag
ACM{w3_4r3_cyb3rw1tch3s_$nd_w1z4rds}


