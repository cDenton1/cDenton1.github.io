---
title: "Advent of Cyber 2025 - TryHackMe"
tags: [tryhackme, ctf-writeup]
read_time: "__ min read"
---

# Advent of Cyber 2025 - TryHackMe

Happy holidays, and happy new year! To finish off the year, I completed TryHackMe's Advent of Cyber 2025 event, as well attempted one of the five side-quest rooms available.

In this post, I'm going to share my overall thoughts on this years event, highlighting any notiable favourites or least favourites, and towards the end, share my walkthrough for the **The Great Disappearing Act** side quest.

## What is Advent of Cyber?

It's a free, beginner-friendly, cybersecurity challenge, hosted on TryHackMe during the month of December. 

Every day leading up to Christmas, a new cybersecurity challenge is released following the story of saving Christmas, finding McSkidy, and stopping King Malhare.

There are a ton of topics covered thoughout the month, including analysis, exploitation, forensics, cryptography, and so much more. 

Along with that, each room completed earns you a ticket entered into a raffle to win some prizes at the end of the competition.

## Daily Tasks

| Day | Challenge | Topic |
| --- | --------- | ----- |
| 0   | Advent of Cyber Prep Track | Event Prep |
| 1   | Shell Bells | Linux CLI |
| 2   | Merry Clickmas | Phishing |
| 3   | Did you SIEM? | Splunk Basics |
| 4   | old sAInt nick | AI in Security |
| 5   | Santa's Little IDOR | IDOR |
| 6   | Egg-xecutable | Malware Analysis |
| 7   | Scan-ta Clause | Network Discovery |
| 8   | Sched-yule conflict | Prompt Injection |
| 9   | A Cracking Christmas | Passwords |
| 10  | Tinsel Triage | SOC Alert Triage |
| 11  | Merry XSSMas | XSS |
| 12  | Phishmas Greetings | Phishing |
| 13  | YARA mean one! | YARA Rules |
| 14  | DoorDasher's Demise | Containers |
| 15  | Drone Alone | Web Attack Forensics |
| 16  | Registry Furensics | Forensics |
| 17  | Hoperation Save McSkidy | CyberChef |
| 18  | The Egg Shell File | Obfuscation |
| 19  | Claus for Concern | ICS/Modbus |
| 20  | Toy to The World | Race Conditions |
| 21  | Malhare.exe | Malware Analysis |
| 22  | Command & Carol | C2 Detection |
| 23  | S3cret Santa | AWS Security |
| 24  | Hoperation Eggsploit | Exploitation with cURL |

I was a little late to the event and didn't start till the 8th but once I caught up around the 15th, I would do the new challenge every day, first thing in the morning. 

Similar to the [Huntress CTF 2025](https://cdenton1.github.io/2025/11/01/Huntress-CTF-2025.html), this challenge got me out of bed every morning and made me start the day with something productive, especially once I was done my classes and finished with my exams.

### Notable Favourites

When I was first started writing this post, I came to the realization that I loved almost every single challenge released this year, and if I could, I would write about each one. 

I've done my best to narrow it done to a small handful and condense my thoughts, but just know my notes from this event are very lengthy.

#### Day 1 - Linux CLI

> 1. Shell Bells - Explore the Linux command-line interface and use it to unveil Christmas mysteries.

Even though it was the first day and pretty simple, it really reminded me of the earlier OverTheWire Bandit levels, and I really liked the addition of challenge flags that could only be "submitted" through completing the necessary tasks on the provided machine.

This level also included extra material that needed to be completed for accessing the first side quest level, **The Great Disappearing Act**, and if you didn't read properly (like myself originally), there is a lot of stuff that needs to be done to get the required pass key.

#### Day 2 & 12 - Phishing

> 2. Merry Clickmas - Learn how to use the Social-Engineer Toolkit to send phishing emails.

This was my first time hearing about and using the Social-Engineer Toolkit (SET), which was really interesting and super cool to mess around with a little and learn how to configure one of the many things it offers.

I've never created phishing emails or simulations, so being on the backend of that and watching the logs come in for it was really neat.

> 12. Phismas Greetings - Learn how to spot phishing emails from Malhare's Eggsploit Bunnies sent to TBFC users.

I know most people probably don't enjoy tasks centered around spotting phishing emails and stuff like that, but I honestly don't mind because I think it's very important and relevant whether you're in cybersecurity or not.

This room defined a handful of phishing email classifications, and then gave you six examples that you had to classify as either phishing or spam, and then further define the type of phishing.

#### Day 3 & 15 - Splunk Basics and Web Attack Forensics

> 3. Did you SIEM? - Learn how to ingest and parse custom log data using Splunk.

I want to work with SIEMS more, and I enjoy sifting through and peicing together the information to figure out what's going on. But I also find they can be a little complex and overwhelming, and thankfully I have experienced Splunk slightly in the past so this felt less daunting and was really fun to work with again.

I got a little stuck on the path traversal section of the challenge, but eventually I figured it out and it was really cool working through it.

> 15. Drone Alone - Explore web attack forensics using Splunk.

Continued using Splunk similar to day 3, however, this time centered around identifying the steps an attacker had taken to execute commands. 

Shorter room compared to the basics, but a great opportunity to build on the skills that one introduced.

#### Day 17 - CyberChef

> 17. Hoperation Save McSkidy - The story continues, and the elves mount a rescue and will try to breach the Quantum Fortress's defenses and free McSkidy.

I absolutely love cryptography related challenges and any opportunity to use CyberChef, so this one definitely made my favourites list. It also introduced me to a new tool, [CrackStation](https://crackstation.net/), a free, online, password hash cracker.

This style of challenge really reminded me of the [picoCTF Vault Door Series](https://cdenton1.github.io/2025/12/22/Vault-Door-Series-Writeup-picoCTF.html), but less complex and instead of reverse engineering, it's cryptography. I loved the style of challenges and levels within it.

### Least Favourites

Unfortunately, I heavily disliked day 4 and 8, which were both AI focused. I think the content itself was interesting, and useful to know and learn, but found the practical portion extremely slow and frustrating. 

Due to the fact that these challenges have to be completed in the AttackBox, they take forever to load, eating away at my only time to access the AttackBox. 

Otherwise, I also didn't really care for these days, besides the time it had taken to find 3 total flags between the two days, they weren't super memorable to me.

## Side Quest

...

### The Great Disappearing Act

...

## Conclusion

...
