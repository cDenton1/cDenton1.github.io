---
title: "Vault Door Series Writeup- picoCTF"
tags: [picoctf, hidden-1, hidden-2, hidden-3]
read_time: "__ min read"
---

# Vault Door Series Writeup - picoCTF

Foil the schemes of Dr.Evil in this long and well-paced Java reverse engineering challenge series, [picoCTF - Vault Door Series](https://play.picoctf.org/playlists/13?m=81).

## Introduction

Back in May and June, while leading up to CyberSci Nationals, I used picoCTF to prepare and practice for the event. It is one of my all time favourite sites for CTF prep, as it's whole focus is CTF styled challenges.

One of the series I had completed early June was the Vault Door series; a collection of Java reverse engineering challenges that grow in difficulty with each level.

In this post, I'm going to walkthrough how I solved each level and discuss what I learnt from the challenge. As to not directly give away the flags, I will avoid showing them for the most part in this post, however, following my steps should hopefully help in retrieving them.

Any scripts that I wrote on this challenge will most likely be included at each level, but they can also be found [here](https://github.com/cDenton1/Extra-Files/tree/main/pctf-VaultDoorSeries) on my GitHub.

## vault-door-training

> Your mission is to enter Dr. Evil's laboratory and retrieve the blueprints for his Doomsday Project. The laboratory is protected by a series of locked vault doors. Each door is controlled by a computer and requires a password to open. Unfortunately, our undercover agents have not been able to obtain the secret passwords for the vault doors, but one of our junior agents obtained the source code for each vault's computer! You will need to read the source code for each level to figure out what the password is for that vault door. As a warmup, we have created a replica vault in our training facility.

This is the easiest one by far, the whole point of this level is to get an idea what the levels look like, show what exactly you are looking for, and get some lore for the overall story.

Download the provided source code, and either open it in any file editor or use something like `cat` to output the contents into your terminal.

Then you can find the flag shown under the comment section stating that the password is below.

## vault-door-1

> This vault uses some complicated arrays! I hope you can make sense of it, special agent.

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

> This vault uses for-loops and byte arrays.

This isn't a typo, picoCTF doesn't list a second level, it jumps to three instead.

This level steps it up on the confusion. I found the easiest way for me to tackle it was take it each for loop at a time, and just ensure I properly understood the operation being applied.

The first 8 characters from the string, `jU5t_a_sna_3lpm18g947_u_4_m9r54f` are copied to an empty list/array, giving us `jU5t_a_s`.

The next 8 characters are filled in reverse, making it `jU5t_a_s1mp13_an`.

The third for loop, takes all the characters at the even indices of the remainging 16 characters, and fills them in reverse. So, like the last steps it's only another 8 characters, but every other one, making the string, `jU5t_a_s1mp13_an4rm4u798`.

Lastly, the remaining characters at the odd indices are filled into their corresponding indices, kind of like filling in the gaps. This makes the string, `jU5t_a_s1mp13_an4gr4m_4_u_79958f`.

Below is a Python script I wrote to do all the above work for me, I just converted the Java and added an extra line to print the final string.

```
def checkPw(pw):
    
    if len(pw) != 32:              # checks the password length is 32
        return False               # returns false if not
    buffer = [""] * 32             # creates a buffer list with 32 empty strings
    
    for i in range(8):             # copies the first 8 characters of the input to the buffer
        buffer[i] = pw[i]
    
    for i in range(8, 16):         # buffer 8 to 16 is filled with the reverse of pw 8 to 16
        buffer[i] = pw[23 - i]
        
    for i in range(16, 32, 2):     # the even indices of buffer 16 to 32 is filled with the reverse of pw 16 to 32
        buffer[i] = pw[46 - i]
    
    for i in range(31, 16, -2):    # the odd indices of buffer 17 to 32 is filled with the corresponding pw i
        buffer[i] = pw[i]
        
    final_pw = ''.join(buffer)     # combined the characters in the buffer list
    print(final_pw)                # prints the final descrambled password
    
checkPw("jU5t_a_sna_3lpm18g947_u_4_m9r54f")
```

## vault-door-4

> This vault uses ASCII encoding for the password.

The source code for this level has an array with decimals, hexadecimals, octal numbers, and literal characters.

For this I actually already had a script made for converting ascii output like this from another command to readable text.

```
import sys

def parse_token(token):
    token = token.strip()
    
    if token.startswith("0x"):
        return chr(int(token, 16))
    elif token.startswith("0"):
        return chr(int(token, 8))
    elif token.startswith("'") and token.endswith("'"):
        return token[1]
    else: 
        return chr(int(token))

def ascii_to_text(data):
    tokens = data.split()
    chars = [parse_token(token) for token in tokens]
    return ''.join(chars)
    
if __name__ == "__main__":
    input_data = sys.stdin.read()
    results = ascii_to_text(input_data)
    print(results)
```

Saving the above script as `asciiText.py` and copying the array items into a text file called `vd4Bytes.txt`, I could run the command `cat vd4Bytes.txt | python asciiText.py` to output the password.

Another way of solving this is manually, you would convert the decimals, hexadecimals, and octal numbers all seperately and then combine them back together with the other characters.

## vault-door-5

> In the last challenge, you mastered octal (base 8), decimal (base 10), and hexadecimal (base 16) numbers, but this vault door uses a different change of base as well as URL encoding!

For this level, I wrote another Python script that reverses the steps from the Java source code, decoded from base64, and then URL decoded before printing.

```
import urllib.parse
import base64
    
encoded_string = "JTYzJTMwJTZlJTc2JTMzJTcyJTc0JTMxJTZlJTY3JTVmJTY2JTcyJTMwJTZkJTVmJTYyJTYxJTM1JTY1JTVmJTM2JTM0JTVmJTM4JTM0JTY2JTY0JTM1JTMwJTM5JTM1"
print(encoded_string)

s1Decode_string = base64.b64decode(encoded_string)
s2Decode_string = urllib.parse.unquote(s1Decode_string)

print(s2Decode_string)
```

However, this one can also be completely done in CyberChef without the need for an extra script.

`[encoded string] > From Base64 > URL Decode > [plaintext password]`

## vault-door-6

> This vault uses an XOR encryption scheme.

...

```
myBytes = [0x3b, 0x65, 0x21, 0xa, 0x38, 0x0, 0x36, 0x1d, 0xa, 0x3d, 0x61, 0x27, 0x11, 0x66, 0x27, 0xa, 0x21, 0x1d, 0x61, 0x3b, 0xa, 0x2d, 0x65, 0x27, 0xa, 0x66, 0x36, 0x30, 0x67, 0x6c, 0x64, 0x6c]
buffer = [""] * 32

for i in range (32):
    buffer[i] = chr(myBytes[i] ^ 0x55)
    
pw = ''.join(buffer)

if len(pw) != 32:
    print("ERROR:" + pw)
else:
    print(pw)
```

## vault-door-7

> This vault uses bit shifts to convert a password string into an array of integers. Hurry, agent, we are running out of time to stop Dr. Evil's nefarious plans!

...

```
ogPass = [1096770097, 1952395366, 1600270708, 1601398833, 1716808014, 1734304867, 942695730, 942748212]
thePass = [""] * 8

for i in range (8):
    binPass = bin(ogPass[i])
    
    binStr = binPass[2:].zfill(32)
    
    byte1 = binStr[0:8]
    byte2 = binStr[8:16]
    byte3 = binStr[16:24]
    byte4 = binStr[24:32]

    hex1 = hex(int(byte1, 2))
    hex2 = hex(int(byte2, 2))
    hex3 = hex(int(byte3, 2))
    hex4 = hex(int(byte4, 2))
    
    char1 = chr(int(hex1, 16))
    char2 = chr(int(hex2, 16))
    char3 = chr(int(hex3, 16))
    char4 = chr(int(hex4, 16))
    
    buffer = [char1, char2, char3, char4]
    thePass[i] = ''.join(buffer)

pw = ''.join(thePass)
print(pw)
```

## vault-door-8

...

## Conlusion
