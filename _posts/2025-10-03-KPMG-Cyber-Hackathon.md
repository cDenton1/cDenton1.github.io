---
title: "KPMG Cyber Hackathon 2025"
tags: [cyber-events]
read_time: "4 min read"
---

# KPMG Cyber Hackathon 2025

Long time no see blog! Just know you haven't been forgotten, there is a lot going on currently. One of which concluded today, the KPMG Cyber Hackathon. My first ever Hackathon, which I competed in with a friend of mine, Caleb Lerch.

In this post I'm going to share my overall experience, what I learnt from it, and even talk about my team's preparation for the event. 

## What is the KPMG Cyber Hackathon?

This is an event that has run in Calgary for three years now, hosted by KPMG. It's a capture the flag challenge, but a little different compared to what you would see at a typical CTF event.

For this challenge, there are multiple machines set up with flags hidden in only a few of them, and you are tasked with finding as many as possible within the alotted time (in our case three hours).

And this challenge is known to be pretty hard when it comes to finding more than just the first one.

## Event Preparation

To prepare for the event and practice alongside one another some more, we had dedicated roughly 4-4.5hrs a couple weeks prior to work through a couple boxes on HackTheBox. 

That was a great way of getting a feel for working together in this type of event. We have competed in CTFs together before, but being that this event is slightly different, and I haven't really done a whole lot similar besides some TryHackMe rooms, this was useful prep.

Each of us had also worked on some other things on our own, which for me was useful for keeping myself in a certain mindset that I like to be in for these types of things.

## Competition 

This morning the event started bright and early at 8am. I got there a little before then to chat with some friends and have adequate time to set up. I caught up with some classmates and some alumni who I have met previously but never really spoken to. 

Once the time started, for probably close to the first half hour, we spent troubleshooting network connection issues. We would get on, and within a couple seconds or minutes we would be kicked off again. Eventually both of us were good with it.

Caleb focused heavily on the first flag, whereas I started on one machine before moving over to another that I had higher hopes for. This worked out as my first machine of choice, actually didn't have a flag.

That being said, I focused for too long on the wrong part of the vulnerable application. I had found a related CVE for the version the machine was using, except a slight difference in configuration meant it wasn't exploitable.

What I did miss until very close to the end, was attempting the default admin login credentials or similar, which was needed for retrieving the second flag.

Caleb did find the first flag for our team through enumeration on one of the machines, which tied us for first along with everyone else. 

The time flew by in this competition, and before I knew it, it was over. My biggest takeaway honestly had to do with my time blindness, especially in short competitions like this, every second counts.

With practice though, challenges like these will get easier, and eventually faster.

## Conclusion

This was a great event, I really enjoyed it even though I missed out on some of the stuff that had taken place afterwards. I hope to compete in more events like it, it's nice to try something else in between all of the typical CTF challenges you see.

And overall, it was a good opportunity to work on these kinds of skills and see where my weak spots were. I will definitely continue practicing on TryHackMe and HackTheBox, hopefully next year we can get that second flag.
