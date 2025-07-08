---
title: "Decrypter"
tags: [python, projects]
read_time: "__ min read"
---

# Decrypter

Decrypter is a modular command-line decrypting tool offering various techniques and the option of applying multiple methods step-by-step. Some techniques even include other options for brute forcing, digit shifting, and character substitution. Feel free to check out the repository if you're interested, [https://github.com/cDenton1/Decrypter](https://github.com/cDenton1/Decrypter).

Since returning from CyberSci Nationals I had a few different ideas for some projects to work on. I will be honest I was expecting this one to take me at least a month, since I normally only work on these projects in the evening after work. However, due to some unforeseen circumstances in my life, I had a lot of free time this weekend and was able to finish it.

In this post I'm going to talk in-depth about the tool, and the future of it.

## Inspiration and Project Plan

For those who have read any of my CTF or cybersecurity event related posts, you might know I am a BIG fan of cryptography and encryption related challenges; especially when I get to use the tool CyberChef. I love the type of problem solving and thought process it requires, and the variety of tools and methods out there really expand on the possibilities.

Now CyberChef is an amazing tool, it offers so much, but sometimes I find it can feel like too much. I wanted to condense Decrypter down to what I use the most in CTFs and combine some of the techniques with extra options for solving problems I've had to figure out in the past. 

For example, at Nationals, we were given binary but instead of 0/1's, it was frowny and smiley faced emojis. In Decrypter I wanted to build in the option for character substitution in case the user encountered something similar.

Making the tool with the main focus of it being modular also meant that other users could make their own mods to add, or only include a couple of the ones I created, instead of each one.
