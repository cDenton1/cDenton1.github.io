---
title: "Snyk Fetch The Flag CTF 2026 - Writeup"
tags: [ctf-writeup]
read_time: "_ min read"
---

# Snyk Fetch The Flag CTF 2026 - Writeup

Snyk returned again to host their annual **Fetch The Flag CTF** challenge from Febraury 12th-13th. 

For the busy week I had, I did take a small opportunity to work through a couple challenges, and I'm going to share a walk through of them here.

I do wish I had the time to work through more but I'm still grateful to have had the chance to spend even just a couple hours working on the few that I did.

## What is Snyk? 

...

## What is Fetch The Flag? 

...

## Challenges

I mainly focused on the crypto and forensics categories, solving 3 challenges and finding a total of 12 flags.

### Void Step 

Forensics - Easy

> You're a SOC analyst at Sfax-Tech.
>
> Your team lead rushes in: "A client server triggered multiple alerts.
>
> The system isolated it and saved the traffic." She hands you a USB with a PCAP file. Find out what happened.
>
> Time is critical.

For this challenge we were given a traffic capture to analyze to answer the following 10 questions. 

1. How many decoy hosts are randomized in this reconnaissance evasion technique? - `11`

For this question I applied the filter `tcp.flags.syn == 1 && tcp.flags.ack == 0`. 

Within the filtered results I noticed a section of 11 light green colour coded packets, which stood out in contrast to the grey of everything else.

Each of those packets were from different IP addresses but all sending to the same destination.

I did also note down that IP down as the possible victim IP address.

![image]

2. What is the real attacker IP address? - `192.168.1.23`

Based off the last question, I went into endpoints to see which IP had the highest amount of packets.

![image]

3. How many open ports did the attacker find? - `4`

For this one I applied the filter `ip.dst == 192.168.1.23 && tcp.flags.syn == 1 && tcp.flags.ack == 1 && ip.src == 192.168.1.27` - including the real attacker IP address and the IP address that I thought might possibly belong to the victim.

I noticed within the filter results that it listed four ports, repeated them a few times, and then only listing port 5000 over and over again. 

![image]

4. What web enumeration tool did the attacker use? - `gobuster`

I applied the filter `ip.src == 192.168.1.23 && http`, then I began sifting through the remaining packets, looking at the Hypertext Transfer Protocol section of each packet.

Within packet 25278, I spotted `gobuster` listed next to User-Agent.

![image]

5. What is the first endpoint discovered by the attacker?

6. What was the first file extension tested during the enumeration?

7. What is the full vulnerable request parameter used in the attack?

8. What is the username discovered by the attacker?

9. What authentication-related file did the attacker attempt to access?

10. What time (HH:MM:SS,MS) did the attacker start brute forcing for SSH?

### Ready - Smile - Action

Crypto - Medium

> A configuration export was recovered from a secure service.
>
> The values appear unrelated, but may hide something valuable.
>
> Can you reconstruct the original message?

...

### Did You Read The FAQ?

Misc - Easy

> Make sure to read the FAQ!

This one was extremely easy to find, as all you needed to do was go to the FAQs page of the CTF site.

Right at the top, underneath the first header, there was the flag - `flag{let_the_games_begin}`

## Conclusion

...
