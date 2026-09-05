---
title: "Cipher's Secret Message Writeup - TryHackMe"
tags: [tryhackme, writeup, challenge-writeup]
read_time: "3 min read"
---

# Cipher's Secret Message Writeup - TryHackMe

> Sharpen your cryptography skills by analyzing code to get the flag.

It is almost the end of January of the new year and I've been working hard! I wanted to start the year off right, and what better way than lot's of practicing. 

I've been tackling a ton of TryHackMe rooms, working my way through the **Cyber Security 101** path, along with some challenge rooms, and even competing at the Western Regionals of the **NCC Mastercard CTF series** back on the 17th of this month.

In this post I'm going to cover one of the TryHackMe challenge rooms I completed recently and walkthrough my solve process. It's not a super long room, but was a ton of fun and good practice for some cryptography and better understanding code.

## The Challenge

> One of the Ciphers' secret messages was recovered from an old system alongside the encryption algorithm, but we are unable to decode it.
>
> Order: Can you help void to decode the message?
>
> Message : a_up4qr_kaiaf0_bujktaz_qm_su4ux_cpbq_ETZ_rhrudm

Then we are also provided with the following encryption algorithm:
```python
from secret import FLAG

def enc(plaintext):
    return "".join(
        chr((ord(c) - (base := ord('A') if c.isupper() else ord('a')) + i) % 26 + base) 
        if c.isalpha() else c
        for i, c in enumerate(plaintext)
    )

with open("message.txt", "w") as f:
    f.write(enc(FLAG))
```

It's a simple room, figure out the required decryption algorithm needed to decipher the message from the given code.

## Breaking it Down

Something in the code I had never seen before was the use of the operator `:=`. This is a walrus operator, it can be used to to assign a value to a variable and use it in the same line.

In this case, it's assigning a value to the variable `base` in the same line as it's encrypting each character.

It's a position-based Caesar shift, each shift increases by 1 for each character position.

A breakdown of the encryption algorithm goes as follows:

1. It uses the `enumerate` function to iterate through each character of the given string as the variable `c` while keeping count with the variable `i`
2. It checks if the character is alphabetic with the function `isalpha`
3. If it isn't, it returns it unchanged - meaning numbers and symbols stay unchanged
4. Otherwise, it continues with the following steps
5. It checks if `c` is uppercase using the `isupper` function
6. If it is, the variable `base` is assigned as `ord('A')` otherwise it's assigned as `ord('a')` - keeping the letters uppercase or lowercase through encryption
7. It subtracts `base` from `ord(c)`
8. It adds `i`
9. Then it does `%` (modulo) 26
10. Lastly adds `base`

Each character of the given string goes through this, is joined together to make one string and returned.

## Solution

For decrypting the message I wrote the following script. Once I had an understanding of how the encryption process worked, going back and reverting the steps to decrypt the message made it very easy.

```python
message = 'a_up4qr_kaiaf0_bujktaz_qm_su4ux_cpbq_ETZ_rhrudm'
index = 0
decrypted = []

for c in message:
	if c.isalpha():
		base = ord('A') if c.isupper() else ord('a')
		decrypted.append(chr(((ord(c) - base - index) % 26 + base)))
	else:
		decrypted.append(c)
	index += 1
	
print("".join(decrypted))
```

Note, in my script I used `index` to mirror the variable `i` from the original code.

Running the script printed out the deciphered message. Which has been omitted to avoid spoilers.
```zsh
─$ python3 solve.py
[ ~ message ~ ]
```

## Conclusion

Very simple challenge that had taken me no more than 30 minutes to figure out and solve but again, great practice for cryptography and understanding code. 

I have been pushing myself to try harder challenges more often, but sometimes coming back to the smaller challenges is still good practice and reinforces the basics.
