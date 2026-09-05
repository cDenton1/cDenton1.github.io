---
title: "Vault Door Series Writeup - picoCTF"
tags: [writeup, picoctf, challenge-writeup]
read_time: "15 min read"
---

# Vault Door Series Writeup - picoCTF

Foil the schemes of Dr.Evil in this long and well-paced Java reverse engineering challenge series, [picoCTF - Vault Door Series](https://play.picoctf.org/playlists/13?m=81).

## Introduction

Back in May and June, while leading up to CyberSci Nationals, I used picoCTF to prepare and practice for the event. It is one of my all time favourite sites for CTF prep, as its whole focus is CTF styled challenges.

One of the series I had completed early June was the Vault Door series; a collection of Java reverse engineering challenges that grow in difficulty with each level.

In this post, I'm going to walk through how I solved each level and discuss what I learnt from the challenge. As to not directly give away the flags, I will avoid showing them for the most part in this post, however, following my steps should hopefully help in retrieving them.

Any scripts that I wrote for this challenge will most likely be included at each level, but they can also be found [here](https://github.com/cDenton1/Extra-Files/tree/main/pctf-VaultDoorSeries) on my GitHub.

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

# list of characters in the password to their position, converted from Java
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

This level steps it up on the confusion. I found the easiest way for me to tackle it was to take it each for loop at a time, and just ensure I properly understood the operation being applied.

The first 8 characters from the string, `jU5t_a_sna_3lpm18g947_u_4_m9r54f` are copied to an empty list/array, giving us `jU5t_a_s`.

The next 8 characters are filled in reverse, making it `jU5t_a_s1mp13_an`.

The third for loop, takes all the characters at the even indices of the remaining 16 characters, and fills them in reverse. So, like the last steps it's only another 8 characters, but every other one, making the string, `jU5t_a_s1mp13_an4rm4u798`.

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

Another way of solving this is manually, you would convert the decimals, hexadecimals, and octal numbers all separately and then combine them back together with the other characters.

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

The code for this level initially threw me off, but the line `((passBytes[i] ^ 0x55) - myBytes[i])` stood out to me when working through it.

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

Rewriting it to `buffer[i] = chr(myBytes[i] ^ 0x55)` was very important. 

Combining that with a Python script that loops through the given bytes, converts them to characters, stores them in a buffer list, and then combines the list and prints the password was how I solved this level.

## vault-door-7

> This vault uses bit shifts to convert a password string into an array of integers. Hurry, agent, we are running out of time to stop Dr. Evil's nefarious plans!

The source code for this level include a lot of text, but below is what I found useful or important from it:
```
    // Each character can be represented as a byte value using its
    // ASCII encoding. Each byte contains 8 bits, and an int contains
    // 32 bits, so we can "pack" 4 bytes into a single int. Here's an
    // example: if the hex string is "01ab", then those can be
    // represented as the bytes {0x30, 0x31, 0x61, 0x62}. When those
    // bytes are represented as binary, they are:
    //
    // 0x30: 00110000
    // 0x31: 00110001
    // 0x61: 01100001
    // 0x62: 01100010
    //
    // If we put those 4 binary numbers end to end, we end up with 32
    // bits that can be interpreted as an int.
    //
    // 00110000001100010110000101100010 -> 808542562
    //
    // Since 4 chars can be represented as 1 int, the 32 character password can
    // be represented as an array of 8 ints.
    //
    // - Minion #7816
    public int[] passwordToIntArray(String hex) {
        int[] x = new int[8];
        byte[] hexBytes = hex.getBytes();
        for (int i=0; i<8; i++) {
            x[i] = hexBytes[i*4]   << 24
                 | hexBytes[i*4+1] << 16
                 | hexBytes[i*4+2] << 8
                 | hexBytes[i*4+3];
        }
        return x;
    }

    public boolean checkPassword(String password) {
        if (password.length() != 32) {
            return false;
        }
        int[] x = passwordToIntArray(password);
        return x[0] == 1096770097
            && x[1] == 1952395366
            && x[2] == 1600270708
            && x[3] == 1601398833
            && x[4] == 1716808014
            && x[5] == 1734304867
            && x[6] == 942695730
            && x[7] == 942748212;
    }
```

Paying attention to the above, I wrote a script that does the following:
1. Stored all given integers into a list
2. Looped through each one
3. Converted them to binary
4. Removed the `0b`
5. Added 32 bits of padding
6. Split them into 4 bytes
7. Converted each to hex, then to characters
8. Stored into a buffer
9. Combined them into one string, storing it
10. Lastly printing them out

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

Once the script had successfully run, it printed out the password for the level.

## vault-door-8

> Apparently Dr. Evil's minions knew that our agency was making copies of their source code, because they intentionally sabotaged this source code in order to make it harder for our agents to analyze and crack into! The result is a quite mess, but I trust that my best special agent will find a way to solve it.

Below was the provided source code for this final level:

```java
// These pesky special agents keep reverse engineering our source code and then
// breaking into our secret vaults. THIS will teach those sneaky sneaks a
// lesson.
//
// -Minion #0891
import java.util.*; import javax.crypto.Cipher; import javax.crypto.spec.SecretKeySpec;
import java.security.*; class VaultDoor8 {public static void main(String args[]) {
Scanner b = new Scanner(System.in); System.out.print("Enter vault password: ");
String c = b.next(); String f = c.substring(8,c.length()-1); VaultDoor8 a = new VaultDoor8(); if (a.checkPassword(f)) {System.out.println("Access granted."); }
else {System.out.println("Access denied!"); } } public char[] scramble(String password) {/* Scramble a password by transposing pairs of bits. */
char[] a = password.toCharArray(); for (int b=0; b<a.length; b++) {char c = a[b]; c = switchBits(c,1,2); c = switchBits(c,0,3); /* c = switchBits(c,14,3); c = switchBits(c, 2, 0); */ c = switchBits(c,5,6); c = switchBits(c,4,7);
c = switchBits(c,0,1); /* d = switchBits(d, 4, 5); e = switchBits(e, 5, 6); */ c = switchBits(c,3,4); c = switchBits(c,2,5); c = switchBits(c,6,7); a[b] = c; } return a;
} public char switchBits(char c, int p1, int p2) {/* Move the bit in position p1 to position p2, and move the bit
that was in position p2 to position p1. Precondition: p1 < p2 */ char mask1 = (char)(1 << p1);
char mask2 = (char)(1 << p2); /* char mask3 = (char)(1<<p1<<p2); mask1++; mask1--; */ char bit1 = (char)(c & mask1); char bit2 = (char)(c & mask2); /* System.out.println("bit1 " + Integer.toBinaryString(bit1));
System.out.println("bit2 " + Integer.toBinaryString(bit2)); */ char rest = (char)(c & ~(mask1 | mask2)); char shift = (char)(p2 - p1); char result = (char)((bit1<<shift) | (bit2>>shift) | rest); return result;
} public boolean checkPassword(String password) {char[] scrambled = scramble(password); char[] expected = {
0xF4, 0xC0, 0x97, 0xF0, 0x77, 0x97, 0xC0, 0xE4, 0xF0, 0x77, 0xA4, 0xD0, 0xC5, 0x77, 0xF4, 0x86, 0xD0, 0xA5, 0x45, 0x96, 0x27, 0xB5, 0x77, 0xC2, 0xD2, 0x95, 0xA4, 0xF0, 0xD2, 0xD2, 0xC1, 0x95 }; return Arrays.equals(scrambled, expected); } }
```

I had cleaned up the given source code, making it look like the following, which made it much easier to understand and work on.

```java
// These pesky special agents keep reverse engineering our source code and then
// breaking into our secret vaults. THIS will teach those sneaky sneaks a
// lesson.
//
// -Minion #0891
import java.util.*; 
import javax.crypto.Cipher; 
import javax.crypto.spec.SecretKeySpec;
import java.security.*; 

class VaultDoor8 {
    public static void main(String args[]) {
        Scanner b = new Scanner(System.in); 
        System.out.print("Enter vault password: ");
        String c = b.next(); 
        String f = c.substring(8,c.length()-1); 
        VaultDoor8 a = new VaultDoor8(); 
        if (a.checkPassword(f)) {
            System.out.println("Access granted."); 
        }
        else {
            System.out.println("Access denied!"); 
        } 
    } 

    public char[] scramble(String password) {
        /* Scramble a password by transposing pairs of bits. */
        char[] a = password.toCharArray(); 
        for (int b=0; b<a.length; b++) {
            char c = a[b]; c = switchBits(c,1,2); 
            c = switchBits(c,0,3); 
            
            /* c = switchBits(c,14,3); c = switchBits(c, 2, 0); */ 
            c = switchBits(c,5,6); 
            c = switchBits(c,4,7);
            c = switchBits(c,0,1); 
            
            /* d = switchBits(d, 4, 5); e = switchBits(e, 5, 6); */ 
            c = switchBits(c,3,4); 
            c = switchBits(c,2,5); 
            c = switchBits(c,6,7); 
            a[b] = c; 
        } 
        return a;
    } 

    public char switchBits(char c, int p1, int p2) {
        /* Move the bit in position p1 to position p2, and move the bit
        that was in position p2 to position p1. Precondition: p1 < p2 */ 
        char mask1 = (char)(1 << p1);
        char mask2 = (char)(1 << p2); 
        
        /* char mask3 = (char)(1<<p1<<p2); mask1++; mask1--; */ 
        char bit1 = (char)(c & mask1); 
        char bit2 = (char)(c & mask2); 
        
        /* System.out.println("bit1 " + Integer.toBinaryString(bit1));
        System.out.println("bit2 " + Integer.toBinaryString(bit2)); */ 
        char rest = (char)(c & ~(mask1 | mask2)); 
        char shift = (char)(p2 - p1); 
        char result = (char)((bit1<<shift) | (bit2>>shift) | rest); 
        return result;
    } 
    
    public boolean checkPassword(String password) {
        char[] scrambled = scramble(password); 
        char[] expected = { 
            0xF4, 0xC0, 0x97, 0xF0, 0x77, 0x97, 0xC0, 0xE4, 
            0xF0, 0x77, 0xA4, 0xD0, 0xC5, 0x77, 0xF4, 0x86, 
            0xD0, 0xA5, 0x45, 0x96, 0x27, 0xB5, 0x77, 0xC2, 
            0xD2, 0x95, 0xA4, 0xF0, 0xD2, 0xD2, 0xC1, 0x95 
        }; 
        return Arrays.equals(scrambled, expected); 
    } 
}
```

In my notes I hadn't included the above, my solution, or anything else besides a screenshot of the fact I had solved it. 

On my Virtual Machine that I use for pretty much everything though, I did find this script:

```
expected = [
    0xF4, 0xC0, 0x97, 0xF0, 0x77, 0x97, 0xC0, 0xE4,
    0xF0, 0x77, 0xA4, 0xD0, 0xC5, 0x77, 0xF4, 0x86,
    0xD0, 0xA5, 0x45, 0x96, 0x27, 0xB5, 0x77, 0xC2,
    0xD2, 0x95, 0xA4, 0xF0, 0xD2, 0xD2, 0xC1, 0x95
]

def switchBits(c, p1, p2):
    mask1 = 1 << p1
    mask2 = 1 << p2
    bit1 = c & mask1
    bit2 = c & mask2
    rest = c & ~(mask1 | mask2)
    shift = p2 - p1
    result = ((bit1 << shift) | (bit2 >> shift) | rest)
    return result

def scrambleChar(c):
    c = switchBits(c,1,2)
    c = switchBits(c,0,3)
    c = switchBits(c,5,6)
    c = switchBits(c,4,7)
    c = switchBits(c,0,1)
    c = switchBits(c,3,4)
    c = switchBits(c,2,5)
    c = switchBits(c,6,7)
    return c

# Main reverse loop
flag_chars = [""] * 32

for i in range(32):
    found = False
    for candidate in range(32, 127):  # printable ASCII range
        scrambled = scrambleChar(candidate)
        if scrambled == expected[i]:
            flag_chars[i] = chr(candidate)
            # print(f"Found char {i}: {flag_chars[i]}")
            found = True
            break
    if not found:
        print(f"No match for index {i}!")

# Print final flag part
flag_str = ''.join(flag_chars)
print(f"Recovered flag part: {flag_str}")
```

No notes though for how I wrote it, or if this was the final version that had solved it.

Eventually, I would like to go back and take another crack at this final level to see if I would go about it differently, but for now this is how I solved it six months ago.

## Conclusion

I remember at the time really enjoying these levels. I learned Java in high school so going back two years later, having worked with a few different programming languages, it was fun to take a stab at these.

Unrelated to the levels but seeing how my way of note taking has changed as well in six months is interesting. The further I went the less I included, which meant going back was a lot harder for understanding why I chose certain things.

Fun fact, I originally wrote the notes for these levels in a text file on notepad, before eventually moving it over to Obsidian sometime in October. 

These were the challenges that really got me into reverse engineering and had inspired me a little when making my Terminal Hacking level for the Vault 403 CTF last month.

I love picoCTF no matter how simple of a site it is, even though I only heard about it two years ago, it really reminds of my first CTF and what drew me into loving them so much.
