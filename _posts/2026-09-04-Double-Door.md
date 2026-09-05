---
title: "Double Door Writeup - Crackmes.one"
tags: [crackmes, ctf-writeup]
read_time: "6 min read"
---

# Double Door Writeup - Crackmes.one

I'm back in school this week and during my computer architecture class someone asked about **Ghidra**; reminding me of the tool and how I have wanted to practice reverse engineering more recently.

So with that reminder, some motivation to do things I enjoy before school gets heavy, and hearing about the site Crackmes.one for the first time: I attempted my first **crackme** challenge!

## What is a crackme?

While watching the YouTube video, [The Fun Way To Learn Reverse Engineering](https://youtu.be/u-ahOATO62U?si=-jUF39XdD4xV__8l) by CyberFlow, they mentioned the site [Crackmes.one](https://crackmes.one/) as a site to learn and practice reverse engineering challenges.

"[crackmes] was created as a place for reverse engineers to upload their creations and help newcomers learn this discipline." - crackmes FAQ. Users can download crackmes, upload writeups for them, and even submit their own for other users to attempt to "crack".

The whole focus is reverse engineering: challenges are written in a wide variety of languages, for various platforms, and can even extend to include different anti-debugging techniques for a harder challenge.

The site really reminds me of [CryptoHack](https://cryptohack.org/) in a way but instead of the main focus being cryptography, this sites main focus is reverse engineering, which gives it the chance to have a lot of variety and practice in one area.

## Let's Get Cracking

The first challenge I decided to attempt was [Double Door](https://crackmes.one/crackme/6a9281f948cda5a2aaa3dbf3) by the user chaltu. 

| Language | Upload     | Platform | Difficulty |
|----------|------------|----------|------------|
| C/C++    | 2026-08-29 | Windows  | 1.2        |

**Description:**

> A beginner-friendly crackme written in C. It simulates a simple terminal login prompt with a 3-attempt limit and a fake loading animation. Your goal is to bypass the security check and get the "ACCESS GRANTED" message. 
> 
> 1. **Primary Goal:** Reverse engineer the binary to find the main password required to unlock the system 
> 2. **Bonus Goal:** Find the hidden backdoor string left behind by the developer.
> 
> Good luck, and happy reversing! 

I used a Linux machine for my analysis and then I also threw the challenge file into a Windows VM to run the executable when I was ready to test my findings. Yes, the file should technically be safe to run on my host (part of the site rules), but I like to run unknown files on an isolated VM out of habit.

First thing I did was run the command `strings` on the file. This will print any human-readable strings out and sometimes can reveal some really useful info. 

And in a lot of cases for reverse engineering challenges, it actually will immediately reveal what you're looking for:
```
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/
Y3JhY2ttZTIwMjQ=
hack
        CRACKME CHALLENGE            
    Find the correct password!       
Checking credentials
You have %d attempts to crack the password.
[Attempt %d/%d] Enter password: 
 ACCESS GRANTED! 
 You cracked the password!
 Welcome to the secret system!
 Your cracker ID: CR4CK3R_%d
 Running on Windows
 ACCESS DENIED! 
Wrong password. Try again.
 SECURITY ALERT! 
Too many failed attempts. System locked!
Please contact system administrator.
  Your IP has been logged.
 Exiting now...
 Congratulations! You've successfully cracked the system!
Press Enter to exit...
```

This one particular chunk shows a lot of what the output looks like, and towards the top there are two strings listed that really stick out. 

One looks to be encoded with base64 and the other seems to stick out as it doesn't really make sense with the rest of the "output" strings.

However, I want to actually practice reverse engineering and to not finish this challenge within seconds, so I still threw the executable into `Ghidra` for a more in-depth look.

Once the executable was imported in and analyzed, my first check was the **Symbol Tree** pane on the left-hand side, specifically under **Functions**:

![](/assets/images/crackmes/double-door-1.png "Symbol Tree Functions")

Here is where I spotted the function, `check_password()`, which is made up of the following decompiled code:

![](/assets/images/crackmes/double-door-2.png "Decompiled Function check_password")

Here again I spotted one of the two strings from the results earlier, `Y3JhY2ttZTIwMjQ=`. However, now I can confirm what it's encoded with: base64 and it decodes to `crackme2024`.

Looking at this function we can break down how it works:

- The function is called from somewhere else in the program and includes the parameter `*param_1`, this is presumably the password but I still want to confirm
- The encoded string is stored as a string pointer in `local_10`
- It's then decoded from base64 and stored in the variable `local_48`
- From there using `strcmp` (string compare), `param_1` is compared to `local_48` and the result is stored in the int variable `iVar1`
- `strcmp` returns 0 if the strings match and 1 if they don't
- Using an if statement `iVar1` is checked if it equals 0 and stores a 1 in the undefined variable `uVar2` if it is
- Else it looks for if the string `hack` is anywhere in `param_1`, storing the results in the char variable `pcVar3`
- If it doesn't, it sets `uVar2` to 0, else it sets it to 1
- Lastly it returns `uVar2`, again presumably to the main function as the check for whether we submitted either the main password or the hidden backdoor string

From this and the findings earlier, I believe `hack` is the backdoor string or part of it and the encoded string is the main password.

From here I hunted down the main function to confirm my suspicions. Once found, it did:

![](/assets/images/crackmes/double-door-3.png "Symbol Tree Functions")

This is only part of the function but we can see some very important pieces. `fgets` is used to store the user input, which goes through some checks before eventually being sent to the `check_password()` function. 

The result from that is stored in the variable `uVar3` and is checked for whether it equals 0 (wrong password) or else (1, right password).

My last step was actually entering the passwords into the executable to 100% confirm my findings above. First the main password: 

![](/assets/images/crackmes/double-door-6.png "Symbol Tree Functions")

Then the hidden backdoor string:

![](/assets/images/crackmes/double-door-7.png "Symbol Tree Functions")

And that's it, I successfully cracked this challenge!

## Conclusion

I quite enjoyed this challenge for as simple as it was. I thought it was great Ghidra practice, actually diving into the decompiled code in what can be quite a complicated application if you have never used it before, really teaches you a lot.

And it's a great reminder that even though sometimes tools like `strings` can get you what you are looking for very quickly, you don't learn much about how it works or why something is. 

For example, I could have taken that encoded string and threw it into something like CyberChef. From there I can try a ton of different decoding options aimlessly until I found the one, but looking at that decompiled code revealed it without me needing to guess.

Not all challenges are as easy obviously, and even for me this wasn't challenging enough but it got me to practice reading C code and using Ghidra again.
