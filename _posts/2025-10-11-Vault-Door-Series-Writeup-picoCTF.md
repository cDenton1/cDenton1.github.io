---
title: "Vault Door Series Writeup- picoCTF"
tags: [picoctf, ctf-writeup]
read_time: "__ min read"
---

# Vault Door Series Writeup - picoCTF

Foil the schemes of Dr.Evil in this long and well-paced Java reverse engineering challenge series, [picoCTF - Vault Door Series](https://play.picoctf.org/playlists/13?m=81).

## Introduction

Back in May and June, while leading up to CyberSci Nationals, I used picoCTF to prepare and practice for the event. It is one of my all time favourite sites for CTF prep, as it's whole focus is CTF styled challenges.

One of the series I had completed early June was the Vault Door series; a collection of Java reverse engineering challenges that grow in difficulty with each level.

In this post, I'm going to walkthrough how I solved each level and discuss what I learnt from the challenge. As to not directly give away the flags, I will not be showing them anywhere in this post, however, following my steps should hopefully help in retrieving them.

Any scripts that I wrote on this challenge will most likely be included at each level, but they can also be found [here](https://github.com/cDenton1/Extra-Files/tree/main/pctf-VaultDoorSeries) on my GitHub.

## vault-door-training

This is the easiest one by far, the whole point of this level is to get an idea what the levels look like, show what exactly you are looking for, and get some lore for the overall story.

Download the provided source code, and either open it in any file editor or use something like `cat` to output the contents into your terminal.

Then you can find the flag shown under the comment section stating that the password is below.

## vault-door-1

For this level you will see that the password is broken up into a complicated and out of order array that is 32 characters long. 

I copied the long list of character array checks and converted them from Java into Python, and then combined them all into one string.

```py
password = [''] * 32    #create a list of 32 empty characters

# list of chracters in the password to their position, converted from Java
password[0]  = 'd'
password[29] = '9'
password[4]  = 'r'
password[2]  = '5'
password[23] = 'r'
password[3]  = 'c'
password[17] = '4'
password[1]  = '3'
password[7]  = 'b'
password[10] = '_'
password[5]  = '4'
password[9]  = '3'
password[11] = 't'
password[15] = 'c'
password[8]  = 'l'
password[12] = 'H'
password[20] = 'c'
password[14] = '_'
password[6]  = 'm'
password[24] = '5'
password[18] = 'r'
password[13] = '3'
password[19] = '4'
password[21] = 'T'
password[16] = 'H'
password[27] = '5'
password[30] = '2'
password[25] = '_'
password[22] = '3'
password[28] = '0'
password[26] = '7'
password[31] = 'e'

final_pass = ''.join(password)    # combined each character in the list
print(final_pass)                 # printed the descrambled password
```

Above is the script I wrote for solving this challenge, running it will output the flag contents.

## vault-door-3

This isn't a typo, picoCTF doesn't list a second challenge, it jumps to 3 instead.

## vault-door-4

...

## vault-door-5

...

## vault-door-6

...

## vault-door-7

...

## vault-door-8

...

## Conlusion
