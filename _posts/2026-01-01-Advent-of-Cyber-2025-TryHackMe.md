---
title: "Advent of Cyber 2025 - TryHackMe"
tags: [tryhackme, ctf-writeup, writeup]
read_time: "7 min read"
---

# Advent of Cyber 2025 - TryHackMe

Happy holidays, and happy new year! 

To finish off the year, I completed TryHackMe's Advent of Cyber 2025 event, as well as attempted one of the five side-quest rooms available.

In this post, I'm going to share my overall thoughts on this year's event, highlighting any notable favourites from the daily challenges and share my experience. 

I am keeping this post quite a bit shorter than I normally would, as it's been a busy holiday season and with classes starting in less than a week, I'm wrapping up some small projects and making sure I'm well rested before the new semester.

## What is Advent of Cyber?

It's a free, beginner-friendly, cybersecurity challenge, hosted on TryHackMe during the month of December. 

Every day leading up to Christmas, a new cybersecurity challenge is released following the story of saving Christmas, finding McSkidy, and stopping King Malhare.

There are a ton of topics covered throughout the month, including analysis, exploitation, forensics, cryptography, and so much more. 

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

Similar to the [Huntress CTF 2025](https://cdenton1.github.io/2025/11/01/Huntress-CTF-2025.html), this challenge got me out of bed every morning and made me start the day with something productive, especially once I was done with my classes and finished my exams.

### Notable Favourites

When I first started writing this post, I came to the realization that I loved almost every single challenge released this year, and if I could, I would write about each one but I want to keep this on the shorter end. 

I've done my best to narrow it down to a small handful and condense my thoughts, but just know my notes from this event are very lengthy.

#### Day 1 - Linux CLI

> 1 Shell Bells - Explore the Linux command-line interface and use it to unveil Christmas mysteries.

Even though it was the first day and pretty simple, it really reminded me of the earlier OverTheWire Bandit levels, and I really liked the addition of challenge flags that could only be "submitted" through completing the necessary tasks on the provided machine.

This level also included extra material that needed to be completed for accessing the first side quest level, **The Great Disappearing Act**. If you didn't read properly (like myself originally), there is a lot of stuff that needs to be done to get the required passkey to access it.

#### Day 2 & 12 - Phishing

> 2 Merry Clickmas - Learn how to use the Social-Engineer Toolkit to send phishing emails.

This was my first time hearing about and using the Social-Engineer Toolkit (SET), which was really interesting and super cool to mess around with a little and learn how to configure one of the many things it offers.

I've never created phishing emails or simulations, so being on the backend of that and watching the logs come in for it was really neat.

> 12 Phismas Greetings - Learn how to spot phishing emails from Malhare's Eggsploit Bunnies sent to TBFC users.

I know most people probably don't enjoy tasks centered around spotting phishing emails and stuff like that, but I honestly don't mind because I think it's very important and relevant whether you're in cybersecurity or not.

This room defined a handful of phishing email classifications, and then gave you six examples that you had to classify as either phishing or spam, and then further define the type of phishing.

#### Day 3 & 15 - Splunk Basics and Web Attack Forensics

> 3 Did you SIEM? - Learn how to ingest and parse custom log data using Splunk.

I want to work with SIEMS more, and I enjoy sifting through and piecing together the information to figure out what's going on. 

But I also find they can be a little complex and overwhelming, and thankfully I have experienced Splunk slightly in the past so this felt less daunting and was really fun to work with again.

I got a little stuck on the path traversal section of the challenge, but eventually I figured it out and it was really cool working through it.

> 15 Drone Alone - Explore web attack forensics using Splunk.

Continued using Splunk similar to day 3, however, this time centered around identifying the steps an attacker had taken to execute commands. 

Shorter room compared to the basics, but a great opportunity to build on the skills that one introduced.

#### Day 17 - CyberChef

> 17 Hoperation Save McSkidy - The story continues, and the elves mount a rescue and will try to breach the Quantum Fortress's defenses and free McSkidy.

I absolutely love cryptography related challenges and any opportunity to use CyberChef, so this one definitely made my favourites list. It also introduced me to a new tool, [CrackStation](https://crackstation.net/), a free, online, password hash cracker.

This style of challenge really reminded me of the [picoCTF Vault Door Series](https://cdenton1.github.io/2025/12/22/Vault-Door-Series-Writeup-picoCTF.html), but less complex and instead of reverse engineering, it's cryptography. I loved the style of challenges and levels within it.

## Side Quests

For anyone who has never competed in an Advent of Cyber event, alongside the main, beginner-friendly daily tasks, there are sometimes also side quests available for those who want to take on an even harder challenge.

Having learnt a lot from CTFs and about cybersecurity in-general throughout 2025, I wanted to take a swing at the first of five side quests available this year.

### The Great Disappearing Act 

Now I was able to retrieve the required passkey from the day 1 challenge after quite a bit of digging and lots of re-reading info, before moving on to find the first flag of the actual room on my own.

From there I was able to gain access to the Asylum security site, but I wasn't able to get much further and successfully retrieve the last two flags before the competition ended. 

That being said, once it wrapped I went back and followed a walkthrough to see what I was missing.

This challenge combined a ton of skills together to make a really fun but difficult challenge, reminding me that there are always new things to learn.

Goal for next year, beat one of the side quests for Advent of Cyber 2026.

## Conclusion

I had a ton of fun with this event, it introduced me to some new concepts and topics, and gave me the opportunity to practice my skills.

It reminded me why I love learning about cybersecurity. It encouraged me to work on more projects and spend less of my time doom-scrolling or rotting in bed.

As much as the holidays were a perfect chance for a break from the stress of studies and a constant list of deadlines, having this daily competition leading up to Christmas honestly kept me from crashing as soon as my exams were over.

I would highly recommend competing next year if you have the chance, no more than 30 minutes a day and lots of fun!

![Advent of Cyber Certificate](/assets/images/thm-aoc-cert.png "Advent of Cyber Certificate")
