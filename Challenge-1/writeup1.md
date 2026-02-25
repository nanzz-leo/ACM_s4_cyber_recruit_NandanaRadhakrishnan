#CTF Write-up 1 (OSINT)
date: 12/02/2026

##Challenge Description
Restaurant:So last time I went to this place with my friend i ate in a restaurant which was opened in 1980. We ate chicken biryani in the ground floor of that restaurant. I don’t remember the price, can you find out?
Flag format: ACM{}

##Analysis and Approach

what is OSINT -> Open Source Intelligence
exitftool? -> writing and editing metadata -> any imp info received ? -> No 
Honestly, I wasted a bit of time at the start thinking this was a forensics challenge. I saw the city.webp file and immediately threw the usual tools at it—exiftool, strings, and binwalk—expecting a hidden flag or comment.

When nothing came up, I took a step back and realized it was pure OSINT.

The Approach:

Location ID: I tossed the image into Google Lens, which instantly identified the landmark as Charminar in Hyderabad.

Narrowing it down: The challenge description mentioned the year 1980, so I searched for famous restaurants near Charminar established around that time. Hotel Shadab came up as the top result.

Finding the Flag: I looked up Hotel Shadab on Google Maps and sorted the photos/reviews by "Newest."

The Win: I found a menu uploaded just yesterday by a user named "Mani Ratnam." The flag was the price listed for the Chicken Biryani.
tried strings city.webp | grep -i "ACM" but no flage found

trying binwalk -> didnt work as expected

=> put the image in google lens to see which place this is -> search results showed charminar hyderabad build in 1591

searched what os int was properly , typed into google as to what restaurants opened in charminar during 1980 , found shadab restaurant
checked the place with latest in it , found a menu by mani ratnam , one day ago, and saw the chicken biryani price and found the flag.

##Tools/Commands Used
exiftool, strings,binwalk,steghide (yet none was needed for this challenge)

##The Final Flag
ACM{260}





