---
title: "CyberSci Regionals 2024"
datePublished: Sun Dec 01 2024 02:24:19 GMT+0000 (Coordinated Universal Time)
cuid: cm44za1o900040ajr63x7dsfu
slug: cybersci-regionals-2024
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1733020375316/8b9ae1ac-214d-4f90-b218-28640c1d10d1.jpeg
tags: [cyber-events]
---

### Introduction

This past weekend on November 23rd, I had the opportunity to represent SAIT on one of their two teams attending CyberSci Regionals here in Calgary this year. Our team, Payload Pirates, was able to secure 3rd place by the end of the four hour event, it was extremely difficult but I had a lot of fun and learnt a lot from the experience. I’m extremely proud of our team and how we performed, I couldn’t have asked for a better group to work along side and compete with. Congrats to the other SAIT team as well and to everyone else who had the opportunity to compete, everyone did amazing.

I was originally considering doing a writeup after the event but I think I’d have more to say if I write from a less technical view and more of a reflection on my performance and experience in the event instead. I worked hard during those four hours but I know I have a lot I can work on myself and a lot more to learn still. This experience was eye opening and I’m extremely thankful that I got to take part in it.

---

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733019614213/4c60d91f-8e83-4f84-a370-c0b39570bcc5.png align="center")

### What is CyberSci?

Before I dive into my experience in the event, for those of you who may not be familiar with what it is, let me quickly explain it for you. CyberSci is a security competition hosted across Canada in a handful of different regions for teams of university and college students interested in cybersecurity to come together and compete in. The challenges are a mix of pretty much anything and everything cybersecurity related. Winners from each region then get to move on and compete in the Canadian nationals, and the top teams from there get to represent Canada on the international stage.

---

### Preparation

To prepare for this event I had been practicing more on websites like [TryHackMe](https://tryhackme.com/r/dashboard) and [picoCTF](https://www.picoctf.org/), meeting with my professors for some advice, and meeting with my teammates to discuss everyone’s skillset. It was a great chance to get other viewpoints on the competition from people who have taught me and been in the industry. Speaking with teammates was a great chance to see how everyone was preparing for the event themselves and what resources they were going to make sure they had ready on hand for the day of the event. And TryHackMe and picoCTF was great for practicing skills that I needed a refresher in and learning a little more.

### The Beginning

I find I always start off these kinds of events fairly nervous, not because I feel like I lack the skills but more so because I don’t know what everyone else’s skills or skill levels are. I’m fairly confident in what I know and I also know where I lack the practice or knowledge, but going into these events I almost always automatically assume everyone is better than me at everything. And unfortunately CyberSci was no different, thankfully though I showed up around an hour early and had the opportunity to chat with my teammates, other competitors, the volunteers and even the sponsors. This helped me destress a little before the event and gave me the opportunity to remind myself that I’m there to do my best, we’re all students there to learn more, and to stop comparing myself to everyone before the event even begins.

### The Challenges

The challenges I started with and focused on for the most part were the cryptography and the forensics challenges, I’ve noticed those have been my go to challenges recently when it comes to CTFs; whether in-person or online.

The one I spent most of my time on was the challenge, **Leaked and Loaded**. We were provided a text file of binary with the challenge description stating that this was someone’s password/passcode that they encrypted before sending over in an email. There were no hints to point to what encryption was possibly used, only the text file. The first step was very easy, convert the binary to ascii so it was a string of characters. Every time I would try a combination of two or three different decryption techniques on that string, I felt like I had gone too far and then back tracked to the original string of characters, essentially starting over. To make sure I wasn’t missing something obvious I asked a few of the volunteers and sponsors for their input and we were all in agreement that the flag would be obvious when it was right. This challenge was one I kept coming back to throughout the four hours as I didn’t want it to be the only challenge I attempted during the event but I didn’t want to give up on it either since I felt like I was so close. Afterwards, one of the sponsors shared the solution with me and I couldn’t believe how close I had gotten to solving it yet just barely missing it.

<details data-node-type="hn-details-summary"><summary>Leaked and Loaded Solution</summary><div data-type="detailsContent">Convert the original binary to ASCII, <strong>ErcmE3yzD2KmQCWqDBbyhrqzh2qzEd1wfB59</strong>, then use ROT13 and set the amount to 21, <strong>ZmxhZ3tuY2FhLXRlYWwtcmluc2luZy1raW59</strong>, and then lastly convert it from Base64 revealing the flag, <strong>flag{ncaa-teal-rinsing-kin}</strong>.</div></details>

Another way I had seen this challenge get solved was from a UofA CompSci student, Hasan Khan, who had done a writeup on the challenges along with their teammates, [https://github.com/osu/cybersci-2024](https://github.com/osu/cybersci-2024). It was almost like a mix of going backwards and forwards, no [i](https://github.com/osu/cybersci-2024)dea if this even makes sense but I’ll try my best to explain. Reading into it actually helped me a lot and gave me possible strategies with these kinds of challenges moving forward, it was actually really clever and a smart way of going about solving it, that I will probably use in the future.

<details data-node-type="hn-details-summary"><summary>Hasan Khan’s Solution Approach</summary><div data-type="detailsContent">First they started by converting the original binary to ASCII, then using ROT13, <strong>RepzR3lmQ2XzDPJdQOoluedmu2dmRq1jsO59</strong>. They said the string looked like it could possibly be Base64, so after that they converted ‘flag{}’ to Base64, <strong>ZmxhZ3t9,</strong> to give themselves an idea of what to look for when converting the other string of characters. They used online tools like <a target="_self" rel="noopener noreferrer nofollow" href="https://gchq.github.io/CyberChef/#oeol=CR" style="pointer-events: none">CyberChef</a> for the first few steps and then <a target="_self" rel="noopener noreferrer nofollow" href="https://www.dcode.fr/rot-cipher" style="pointer-events: none">dcode’s ROT Cipher</a> till they found the string with the first few characters that matched the encrypted version of ‘flag{}’, <strong>ZmxhZ3tuY2FhLXRlYWwtcmluc2luZy1raW59</strong>. Afterwards they converted it from Base64 to get the flag and they found it.</div></details>

One of the other challenges I remember explicitly and spent a lot of time trying was, **Data is the New Currency 1/2** from the forensics category. A couple of my other teammates focused on the first challenge of the pair, while another one of my teammates and I gave the second one a try. In this challenge we were tasked with looking through a PCAP file to find a specific persons occupation, along with this challenge there was a hint stating that there were two forms of encryption/cryptography used, AES and RSA. Keeping this in my mind for the challenge, I decided to quickly skim some lists of people I found in larger packets of the file incase there were any sort of hints to the answer before I moved on. The second strategy I had was focusing on emails, a lot of the packets included emails between a lot of different people and with the hint of encryption, I thought there might be a chance that one of the emails might be from or to the specific person we were looking for. I tried decrypting a few but none seemed to come close to being human readable and I didn’t have any luck looking for what might have been a key. At one point I asked another one of the sponsors if I was even on the right track for what my idea was and unfortunately I wasn’t. No one from any region had the solved it by the end of the four hours and I also didn’t have a chance to speak with anyone else about that challenge to hear the solution or what other people were trying.

I attempted more than just those two challenges, but those were the challenges that I have been thinking about quite a lot since the end of the competition. I have even gone back to them a couple times since then to look at more or have even led me to trying similar challenges in those areas on some online resources.

Another challenge I have been reading some writeups on and have skimmed over myself since then was one of the reverse engineering challenges, **Vector Veil**. I didn’t attempt this challenge during the competition as I was more confident in the other challenges I was working on but as someone who loved learning assembly programming in school, I wanted to read up on the challenge after the event. Reading over the writeups has taught me quite a lot more about assembly then what I already knew and has shown me some really interesting tools that I hope to get a chance to look over more once I am done my finals. And if I can find the file out there somewhere, maybe then I will even attempt this challenge myself.

### Networking

After the four hours of hacking we had the opportunity to chat with everyone at the event some more, this was a great chance to see how everyone else felt once we hit the end; their strategies, the challenges they attempted, where they may have gotten stuck, and just overall how they felt about the event. I found myself talking with the other SAIT team and the UofC teams since most of us had competed against one another in the Infected Networks CTF back in early November and some of us were attending the same program just in different semesters. It was awesome to hear some different viewpoints on everything and just get the chance to chat.

---

### Conclusion

The competition was great, and even though I felt like I had just been struggling for four hours, I honestly felt like I learnt quite a lot by the end of the event. It was great getting to meet so many other really cool and talented students in the field and I couldn’t be more thankful for the chance to be apart of something like that. Like I’ve said previously this has been kind of like a tipping point for me to be doing even more, I mean this was what finally pushed me to start posting about what I was learning somewhere besides you know a short post here or there on LinkedIn. I love learning about technology and the security aspects, so experiences like this are really what continue to cement those feelings even more.

![SAIT Students](https://cdn.hashnode.com/res/hashnode/image/upload/v1733019798876/c5f1791e-ed2c-4a20-91a2-9175b26fcb6e.jpeg align="center")
