---
title: "CyberSci Nationals 2025"
tags: [cyber-events, hidden-1, hidden-2, hidden-3]
read_time: "__ min read"
---

# CyberSci Nationals 2025

After months of preparation, I finally got to put all of my skills to the test at CyberSci Nationals 2025 this past weekend, and what an experience it was. In this post I want to talk about as much as possible from this amazing four day event; sharing my overall experience in Ottawa, my solutions and thought processes in solving the challenges, and everything I learnt.

Massive thank you to the CyberSci organizers, the team behind it all, and everyone involved in putting together such an event. As someone who is just very anxious in general, everyone made this trip extremely memorable and easy. Every single person I spoke to was so kind and welcoming, and everyone was very open to chatting and getting to know one another.

Congrats to everyone who competed and the winners, there was a lot of strong competition coming from all across Canada, and some of the final scores were very close. I'm extremely proud of how my team and I did for being some of the youngest people there, and for only just meeting each other for the first time on Friday. 

**Juniors Script Kiddies**: Paul Lee, Sam Taplin, Jonathan Lok, Daniel Steele, and myself.

---

![CyberSci Logo](/assets/images/cybersciPic1.png "CyberSci Logo")

## What is CyberSci?

CyberSci is a security competition with regional events hosted across Canada for teams of university and college students interested in cybersecurity to come together and compete in. Winners from each region then get to move on and compete in the Canadian nationals, and the top teams from there get to represent Canada on the international stage. Their mission is to help create jobs for students, and improve the cyber workforce within Canada. 

---

## Defence Competition - Saturday June 14

Each team was given a subnet with four vulnerable services and an attack bot. Each service was ran in a docker container and users had full sudo permissions, but no access to the attack machine. We were all given one hour to analyze the services and begin fixing vulnerabilites, after that we had four hours of competition time. 

Once the competition time started, every two minutes, what was called a _tick_ would happen, and the bot would send a mix of benign and malicious requests to each service. The goal was to cause the malicious requests to fail, while letting the benign requests execute successfully. When a malicious request failed, the team would earn points, but if a benign request failed or the service was down at the time of the tick, you would lose points.

### Preparation

I had never competed in a competition like this or attempted challenges similar, so I was pretty nervous leading up to it. We were given some info about it a couple days prior, which were pretty handy when it came to planning and preparing. 

I had put together some notes ahead of time as an easy reminder for myself, incase I was blanking on useful or important commands during the competition. One of our teammates also setup Tulip for all of us to access, which came in clutch.

Our initial hour was spent ensuring Tulip was working, setting up Git repos on each machine for faster and easier reverting, and the beginning stages of analyzing the services.

### Comptition

...

---

## CTF/Jeopardy Competition - Sunday June 15

In comparison to the defence competition, this is the more common type of competition most people are used to seeing. We were given eight hours to complete as many challenges as possible from a wide variety of categories. 

Each challenge was initially worth 500 points but as more people completed them, the worth of the challenge would decrease. The CTF didn't require much prep day of, unlike the defence competition. This meant our team spent more time discussing who was focusing where.

### Challenge Writeups

There were a lot of challenges and I had worked on quite a few during the eight hour competition; solving a couple on my own, and working with my teammates to help solve a handful more. My main focus ended up being on the cryptography, OSINT, and hardware challenges.

| Challenge              | Category         | Points |
|------------------------|------------------|--------|
| Rigged Ballot Location | OSINT            | 296    |
| 256                    | Crypto           | 100    |
| dot dot dot            | Crypto           | 427    |
| staged                 | Crypto           | 207    |
| Badge 1                | Badge (Hardware) | 100    |
| Badge 2                | Badge (Hardware) | 100    |

#### Rigged Ballot Location - OSINT 296 points

For this challenge, were given the file, **BallotRiggers.jpg**, the image shown below. We were tasked with finding out who owned the compound, and using that information to get to the flag.

![](/assets/images/cybersci-nat25/BallotRiggers.jpg)

I initially started off by moving the file into my Kali Linux VM where I could easily run it through a handful of different tools and commands; including **zsteg**, **steghide**, and **exiftool**. 

- zsteg returned with nothing
- exiftool gave me a lot of information regarding the image, but nothing useful for the challenge
- steghide got stuck since I didn't have a password

From there I had moved over to Google where I could reverse image search the picture, and a few film review sites and wiki pages had come up regarding the 1985 film, _Commando_. I had sifted through a couple before my teammate and I both came across the site, [Filming Locations of Commando - MovieLoci.com](https://www.movieloci.com/4300-Commando?from=8&count=8&sortby=9&sortdirection=0)

It included the uncropped version of the picture we were provided, geographical information, and the name of who owned the compound in the film, _Arius_. 

With our new found information, I wasn't entirely sure of our next steps. My teammate assumed this couldn't be it as it seemed too easy, so I went back to my VM to give steghide another try. This time using the characters name as the passphrase, we were successfully able to retrive this challenges flag.

```bash
$ steghide extract -sf BallotRiggers.jpg
Enter passphrase:
wrote extracted data to "Flag.txt"

$ cat Flag.txt
CybersciNats{R1gged_B4llot_Stor4ge_290948}
```

#### 256 - Crypto 100 points

For this challenge, we were given two files: a python script which contained the code shown below, and a text file with three values.

```py
import math
from Crypto.Util.number import getPrime
from secret import FLAG

BITS = 256
PRIMES = 16

primes = [getPrime(BITS // PRIMES) for _ in range(PRIMES)]

n = math.prod(primes)
phi = math.prod(p - 1 for p in primes)
e = 65537

m = int.from_bytes(FLAG, 'big')
c = pow(m, e, n)

print(f'{n = }')
print(f'{e = }')
print(f'{c = }')
```
```
n = 796619421721763408110066621894301379640702094358332972179336180714381814791
e = 65537
c = 760460476332603195975870956320663031030142509238270316470240866540546100772
```

Right off the bat I recognized this challenge to be similar to a typical RSA encryption puzzle but with a bit of a twist; it used 16 smaller primes to form 'n'. Due to this, we were able to brute force all the primes, and convert everything in a python script. 

```
from Crypto.Util.number import long_to_bytes, inverse
from sympy import primerange

n = 796619421721763408110066621894301379640702094358332972179336180714381814791
e = 65537
c = 760460476332603195975870956320663031030142509238270316470240866540546100772

primes = []                           # empty list for primes
remaining = n                         # store n for calculating primes

for p in primerange(2**15, 2**16):    # loop through all 16 bit primes
    while remaining % p == 0:         # check if p divides remaining
        primes.append(p)              # p is one of the primes
        remaining //= p               # divides remaining (n) by p
        if len(primes) == 16:
            break                     # stop once it finds all 16
    if len(primes) == 16:
        break                         # stop once it finds all 16

phi = 1                               # calculate phi, used for computing d
for p in primes:                      # for RSA, n = p₁*p₂*...*pₙ is the product of (p - 1) for each p
    phi *= (p - 1)

d = inverse(e, phi)                   # caclulate private key, d

m = pow(c, d, n)                      # decrypt, the reverse of c = pow(m, e, n) from the encrypt script
flag = long_to_bytes(m)               # convert the flag from an integer into the og string
print(flag)
```

Running the above script gave us the flag.

```bash
$ python sol256.py
b'cybersci{reest4bl1sh_on_4096}'
```

#### dot dot dot - Crypto 427 points

For this challenge, we were given a text file that included the string shown below. 

```
-.-.-.---.....-....-.-....--....----...-.........--..--.-......--.....-.-.-----..-.....-...-..--..-.-----..-..-----..-....
```

The challenge description confirms that it's morse code without spaces, and gave us some other hints to narrow down the decoding process: it's only letters a-z and includes _cybersci_.

Sam and I tackled this one together because it  didn't seem like a lot till we realized just how many different possibilities of combinations of letters there are. Our initial step, since we were given the clue that the string 'CYBERSCI' was in it, was confirm whether it was at the beginning or end of the string and work from there.

```
-.-. -.-- -... . .-. ... -.-. .. .--....----...-.........--..--.-......--.....-.-.-----..-.....-...-..--..-.-----..-..-----..-....
 C    Y    B   E  R   S   C   I
```

From there, I had tried writing a script to make the process quicker but I kept running into the issue where it would string together mainly 'e' and 't' since they are simply '.' and '-'. This caused the output to mainly be flooded with long strings of e and t, with a few different letters changed at the end.

For our next idea, we both gave the string to a different AI model to see if it could narrow down at least the first few letters. ChatGPT originally couldn't find anything that made sense and kept trying to give me something other than letters a-z. Claude gave Sam the first word as 'WHO' but then turned into gibberish the more it went.

We weren't sure if we wanted to trust the next word as 'WHO', but since there really wasn't much of another direction to go. We stuck through with it, and it did really benefit us since it was indeed right.

```
-.-. -.-- -... . .-. ... -.-. .. .-- .... --- -...-.........--..--.-......--.....-.-.-----..-.....-...-..--..-.-----..-..-----..-....
 C    Y    B   E  R   S   C   I   W   H    O
```

We did want to try another script, but we weren't sure we could automate it entirely and it not take the rest of the competititon time to run. We knew if we could speed up the process even just a little bit, it would make it 10x easier.

I wrote a script that took a list of common dictionary words, filtered out any that were less than three characters long, converted them to morse code without spaces, and compared it to the morse code we were given. This gave a list of 1073 matching words, including 'PROGRAMMERS', which at the time I hadn't realized was actually the last word in the string. 

Sam put together a script that would convert the morse code, string the letters together, compare it to a list of dictionary words, and then repeat. It was a little slow, but was actually pretty accurate and helped point us in a good direction. 

While Sam had that running, I was manually converting and trying different strings with a couple different morse code tools I found online:

- [Morse Code Translator](https://morsecode.world/international/translator.html), a very simple morse code translator that I could easily keep track of the string with.
- [Morse Code - dCode](https://www.dcode.fr/morse-code), a translator with a no spaces and a "brute force" option (one of my favourite sites for any decryption challenge).
- [UnMorse Code Solver - CacheSleuth](https://www.cachesleuth.com/unmorse.html), a decoder to step through it character by character and give you every possible combination.

We went back a forward with our mix of tools, and each time we were confident in the next word, we could confirm with each other, and then shorten the string we were working with. Eventually, after us going through this process a few times, we got to the final string, 'CYBERSCI WHO NEEDS SPACES WHEN YOU HAVE PROGRAMMERS'.

```
-.-. -.-- -... . .-. ... -.-. .. .-- .... --- -. . . -.. ... ... .--. .- -.-. . ... .-- .... . -. -.-- --- ..- .... .- ...- . .--. .-. --- --. .-. .- -- -- . .-. ...
 C    Y    B   E  R   S   C   I   W   H    O  N  E E  D   S   S   P   A   C   E  S   W   H   E N   Y    O   U   H   A   V   E  P    R   O   G   R  A  M  M  E  R   S
```

#### staged - Crypto 207 points

The provided files for this challenge included, **cipher.txt**, a text file which was 44 lines of just under 4500 smiling and frowning emojis. 

This was the first challenge that both Sam and I had opened once the competition started, and he immediatly had an idea so I swapped over to another challenge while he began to work on this one. It had a few steps along the way and was passed around quite a bit amongst our team.

**Step 1**: He had noticed the emojis were only smiling and frowning, so he converted them to binary: 🙁 = 0 and 🙂 = 1. The emojis converted to binary became:
```
0100001101100001011001010111001101100001011100100010000001110111011010010111010001101000001000000110000100100000011000100110100101110100001000000110111101100110001000000111001101110000011010010110001101100101001000000110011001110010011011110110110100100000011101000110100001100101001000000110001101101000011001010110011000001010001100100110111000110110001100100011011000110001001101110011001100110110001101010011011000110100001100100110111000110010001100000011011001110011001101100111001000110010001100000011001001101110001101110110111000110110001110010011011100110000001100100110111000110000011011100011010000111000001100110011010000110111001100110011010000111001001101000011000100110100001110000011001001101111001101000111000100110011001101000011010000110111001101100011001100110100001100010011001001110011001101110011100000110011001100110011010000110101001101110011011100110101001100010011011001110001001101000011000100110101001100010011010000110001001101110011011100110100001101010011011100110111001101000111000000110011001110010011010100110110001101010011011100110100001101010011010100110000001101000011000100110100011100110011011100110101001101110011011100110110001101110011010100111001001101000011011100110011001101000011010000110110001100100111001100110101001101110011010100110001001101100011100000110101001110000011011001101111001101000011010000110100001101100011001100110100001101110011000100110100011100010011001001101111001101010110111000110111001100000011011100111000001100110011100100110010011100110011010000110011001101110011100100110100011100110011010000111000001100110011001000110100011100110011011100110111001101110011001100110010011011110011010000110101001101000011100100110011001100100011001001101111001100110011011000110100011011100011011100110001001100100110111100110101001110010011011100110101001101100011011000110100001110010011010001110010001101010011001100110111001100100011011001110000001101010011001100110110001101100011001100111000001101010011001000110101001101000011011000110100001100110011100000110011001101000011010000110011001100110011011000110100011100010011011000110111001101000011000100110100001100010011010000110001001101000011000100110011011100010011001101110001
```

**Step 2**: From there he converted the binary and got the following:
```
Caesar with a bit of spice from the chef
2n62617365642n206s6r202n7n69702n0n4834734941482o4q344763412s78334577516q4151417745774p3956574550414s757767594734462s575168586o444634714q2o5n7078392s43794s48324s77732o4549322o364n712o597566494r53726p536638525464383443364q67414141413q3q
```

At this point, he had gotten stuck, so he put everything he had found so far into Discord so another teammate could work on it from where he left off. 

I had given it a try from here but everything I was getting was a dead end. Paul decided to give it a go and he was able to get **Steps 3 and 4**:
```
*based* on *zip*
H4sIAH+M4GcA/x3EwQmAQAwEwL9VWEPAOuwgYG4F/WQhXkDF4qM+Zpx9/CyOH2Ows+EI2+6Jq+YufINSrlSf8RTd84C6MgAAAA==
```

And from there it turned into:
```
not not and and or
bxcdsrbhz5oe^ui2o^uid^o2yu^nOd|
```

I'm not entirely sure what Paul had done to get the above steps, I could see similarities between some of what I was getting and the output of step 3, but nothing extremely close. 

I gave it another try from this point since when I had initially seen the results of step 4, I thought I had an idea where to go but it wasn't entirely working out. Returning to it later I had much better luck. I used the follwing script to decrypt the **5th and Final Step** of the challenge and retrive the flag.
```py
ciphertext = b"bxcdsrbhz5oe^ui2o^uid^o2yu^nOd|"              # encrypted data as bytes

def is_readable(s):                                          # checks if the string consists entirely of printable ASCII
    return all(32 <= c < 127 for c in s)                     # helps filter out nonsense

for key in range(256):                                       # loops through every possible byte key
    plaintext = bytes([c ^ key for c in ciphertext])         # XOR every byte with key and creates a new byte object
    if is_readable(plaintext):                               # checks if plaintext is readable
        print(f"Key {key}: {plaintext.decode('ascii')}")     # prints key and plaintext
```

Another more simple option for the above code would be to throw the string into [CyberChef](https://gchq.github.io/CyberChef/#recipe=XOR_Brute_Force(1,100,0,'Standard',false,true,false,'')&input=YnhjZHNyYmh6NW9lXnVpMm9edWlkXm8yeXVebk9kfA) and use the XOR Brute Force operation. But running the script gave us a list of plaintext strings, which did include the flag and the corresponding key:
```bash
$ python stagedA3.py 
Key 1: cybersci{4nd_th3n_the_n3xt_oNe}
```

#### Badge 1 - Badge 100 points

...

#### Badge 2 - Badge 100 points

...

---

## Overall Experience

...

---

## Conclusion

...
