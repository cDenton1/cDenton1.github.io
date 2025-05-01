---
layout: default
title: "Infected Networks CTF 2024"
datePublished: Thu Nov 28 2024 03:12:10 GMT+0000 (Coordinated Universal Time)
cuid: cm40qo1d1000d09lj5f4z5c5p
slug: infected-networks-ctf
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1733164794990/124af439-6dad-4c46-8b49-afd443eadedb.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1732815551740/5f7db4f7-6076-4f83-98ea-9f0503e404f0.jpeg
tags: [ctf-writeup, cyber-events]
---

### Introduction

Back on November 2nd, I had the opportunity to compete in the Last of Us themed CTF, Infected Networks, hosted by SAIT, Megabyte SAIT, Women in CyberSecurity (WiCyS) UofC, and The Cybersecurity Club - UCalgary. My teammate and I placed 3rd in the teams category. It was an awesome experience, and as a way to solidify my knowledge I put together a challenge walkthrough on Google Sheets, which I am now transferring and sharing over here with a nicer look (hopefully to be the first of many).

---

### Challenges

* Frozen in Time
    
* Loves Price
    
* Salted Enigma
    
* The Astral Enigma
    
* Quest for the Scholar’s Code
    
* The Nexus of Secrets
    
* Infected Net (not sure if this was the correct name of the challenge)
    
* Intercepted Secrets
    

\* these are not all the challenges, only the ones my teammate and I solved \*

---

### Frozen in Time

Task: Find the exposure of the provided image, Frozen\_In\_Time.JPG, and submit the flag as SAIT\_CTF\_Flag{\[exposure\] seconds}

To complete this challenge was fairly easy, once you downloaded and opened the file, by right-clicking the image and selecting, File Info, you would be given a section revealing info about the device that had taken the photo, labeled, Device Info. Looking for #/sec under there would give you the exposure in seconds and that was the flag.

### Loves Price

Task: Solve the riddle by searching the internet to find the answer which was the flag in the format SAIT\_CTF\_Flag{}.

To complete this challenge, you would use the clues found in it like how it references love-themed malware, mentions email, and that it is something from the past, it would lead you towards the ILOVEYOU virus, which was the answer to the riddle and the challenges flag.

### Salted Enigma

Task: Find the password for a specific user by cracking password hashes from the provided file, Salted\_Enigma.txt, and solve the riddle to know the user you are specifically looking for.

To solve this challenge, right off the bat once you downloaded the text file, you would run it through an application like John the Ripper and crack all the hashes. Then focus on solving the riddle, instead I skipped answering it and just submitted the password for the user ‘hacker1’ as it was the user that stood out amongst the other listed users and it ended up being the correct flag.

### The Astral Enigma

Task: Decode a secret message by using a riddle to figure out the cipher used, then decrypt another secret message found on a poster in a physical location hidden by another riddle.

To complete this challenge, I began with both the riddles provided in the task description, the first pointed to the Caesar cipher with a left shift of 5 and the other riddle pointed towards the bookstore, this is where the other encrypted message was found along with a third riddle. I used the Caesar cipher on the message, NFRFHYKXUJHNFQNXY, and revealed, IAMACTFSPECIALIST. Which was used as the key for decrypting the second message, BHQ FNTL AH LKLDPV SM "/MXBETM\_V1”. The third riddle pointed towards the Vigenère cipher, using an online resource and plugging the key and message in, you were given a subdomain for the CTF website which when visiting revealed the flag for the challenge.

### Quest for the Scholar’s Code

Task: Solve a riddle pointing to a location on SAIT’s campus, submit the flag as SAIT\_CTF\_Flag{\[room #\]}.

Very easily, the riddle pointed to the library, and there were two ways to solve this, 1) physically go to the library and find the room number on the plaque outside of one of the doors, or 2) search SAIT’s website where it is listed on the page discussing library resources.

### The Nexus of Secrets

Task: Solve a riddle pointing to a location on campus, go to the location and get more hints. Watch a specific network, listen for traffic, and look for a website hidden in the packets. Going to the website you found and login to gain access to more info.

Completing this challenge was made easy for us due to technical difficulties, they gave us the URL and skipped the first portion of the challenge. In the case where that wouldn’t have happened, we used netspot originally to find the network, from there we would have most likely used nmap to scan it looking for any open ports revealing the website. Once my groupmate access the website he used “OR 1=1” to bypass the login and find the flag to complete the challenge.

### Infected Net

(not sure if this was the correct name of the challenge)

Task: Solve a riddle to find a specific location on campus and go to the location to gain more information on the task of the challenge. Once in the room it gave us another subdomain for the CTF website, this was a download link link for a PCAP file, Infected\_Net\_campus\_panic\_traffic.pcap. Then you had to find the flag hidden in packets of the PCAP.

To solve the challenge we obviously started with the riddle which led us to room MD023, the room with the copper cage. We went to the site it listed, downloaded the PCAP file and began skimming through it. Not thinking to search for a string hidden in the packet details (which would’ve been the easiest and fastest way), we sorted the packets by size, starting with the largest. It was significantly larger then the rest of the packets and titled secret in the packet info, clicking into it showed the flag in the bytes pane.

### Intercepted Secrets

Task: Search through the provided PCAP file, Email.pcap, looking for an encrypted packet of an email and password, and submit the flag as SAIT\_CTF\_Flag{\[email\] and \[password\]}.

For solving this challenge, unlike the previous one, the flag was not easily readable. Look through the packets that look to include encrypted strings, following those packets need to be another packet stating whether it was authenticated or not, then you will know whether they were right or not. Once you find the strings, either use something like CyberChef or Python to decrypt them and you will have found the info for the flag.

---

### Conclusion

This was my second ever in-person CTF event and my team members first, we had a lot of fun, and I had the opportunity to work on a lot of the skills I already had, and even learn a few new ones. This event led to my teammate and I being offered spots on one of the two cybersecurity teams that represented SAIT at CyberSci. I hope to attend more in-person CTFs as I work to finish school or even work through more online ones in my free time, and as I do I will continue to write more posts here.
