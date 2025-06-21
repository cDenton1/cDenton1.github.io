---
title: "CyberSci Nationals 2025"
tags: [cyber-events, ctf-writeup]
read_time: "17 min read"
---

# CyberSci Nationals 2025

After months of preparation, I finally got to put all of my skills to the test at CyberSci Nationals 2025 this past weekend, and what an experience it was. In this post I want to talk about as much as possible from this amazing four day event; sharing my overall experience in Ottawa, my solutions and thought processes in solving the challenges, and everything I learnt.

Massive thank you to the CyberSci organizers, the team behind it all, and everyone involved in putting together such an event. As someone who is just very anxious in general, everyone made this trip extremely memorable and easy. Every single person I spoke to was so kind and welcoming, and everyone was very open to chatting and getting to know one another.

Congrats to everyone who competed and the winners, there was a lot of strong competition coming from all across Canada, and some of the final scores were very close. I'm extremely proud of how my team and I did for being some of the youngest people there, and for only just meeting each other for the first time on Friday. 

**Juniors Script Kiddies**: Paul Lee, Sam Taplin, Jonathan Lok, Daniel Steele, and myself.

<details>
    <summary><b>Article Content</b></summary>
    <ul>
        <li>What is CyberSci</li>
        <li>Defence Competition</li>
            <ul>
                <li>Preparation</li> 
                <li>Competition</li>
                <li>Results</li>
            </ul>
        <li>CTF/Jeopardy Competition</li>
            <ul>
                <li>Challenge Writeups</li>
                <li>Results</li>
            </ul>
        <li>Overall Experience</li>
        <li>Conclusion</li>
    </ul>
</details>

---

![CyberSci Logo](/assets/images/cybersciPic1.png "CyberSci Logo")

## What is CyberSci?

CyberSci is a security competition with regional events hosted across Canada for teams of university and college students interested in cybersecurity to come together and compete in. Winners from each region then get to move on and compete in the Canadian nationals, and the top teams from there get to represent Canada on the international stage. Their mission is to help create jobs for students, and improve the cyber workforce within Canada. 

---

## Defence Competition

Each team was given the following:

- Subnet with four vulnerable services and an attack bot
- Each service was ran in a docker container
- Users had full sudo permissions but no access to the attack machine
- One hour to analyze the services and begin fixing vulnerabilities
- Four hours of competition time 

Once the competition time started, every two minutes, what was called a _tick_ would happen, and the bot would send a mix of benign and malicious requests to each service. The goal was to cause the malicious requests to fail, while letting the benign requests execute successfully. 

When a malicious request failed, the team would earn points, but if a benign request failed or the service was down at the time of the tick, you would lose points.

### Preparation

We were given some info about it a couple days prior, which came in handy for planning and preparing. I had put together some notes ahead of time as a reminder for myself in case I forgot something useful. One of our teammates also setup [Tulip](https://github.com/OpenAttackDefenseTools/tulip) for all of us to access.

Our initial hour was spent ensuring Tulip was working, setting up Git repos on each machine for faster and easier reverting, and the beginning stages of analyzing the services.

### Competition

Once the time started, we all began working through one of the four services, letting each other know which ones. 

#### Voter Registry

A couple of us started on this service and it's where I found a python script called **documentscanner.py**. 

- The script dealt with reading files that were uploaded, like PDFs and images, but there wasn't a proper check for the file types.
- I attempted to add a check that would look at the extension and magic number of the file.
- Unfortunately, benign requests began to fail after pushing my changes to the service, so we reverted them back.

By the time we had noticed that, I had moved onto another service and I didn't get a chance to go back and try dealing with that service again. As much as I would have loved to take another crack at it, that  taught me a valuable lesson of just how important it is to watch the scoreboard during these types of challenges. Everything takes effect so quickly and you don't want to miss if something is going wrong.

#### Candidate Registry

The service I swapped over to from there was a service for **Candidate Registry**. Here I mainly helped Sam implement some patches for a SQL injection vulnerability that he found. 

- There were two lines in a file that he had spotted while sifting through its contents.
- I had taken the fixes he wrote, replaced the necessary lines in the code, and restarted the service.
- The patch was successful, and we watched the score this time to ensure we didn't miss anything.

#### Real Time Election

This was the last service I had taken a look at and worked on.; it had a lot to look at. I attempted two patches, one being successful and one that didn't affect our score.

The first one, was in a typescript file that wasn't properly checking information regarding duplicate user info. There seemed to be a lot of vulnerabilities similar to that throughout multiple services, making this patch somewhat expected.

The second one was similar to the vulnerability found above but in an SQL file, I attempted to modify what I thought was wrong, but our score didn't change. Time was up not long after so I didn't get much time to explore it any further.

### Results

By the end of the four hours, we had scored 574 points, and finished 7th out of nine teams. This was my first time doing a challenge like this, I learnt some valuable lessons during it and got to experience a defensive side in a challenge, which I found insightful.

---

## CTF/Jeopardy Competition

In comparison to the defence competition, this is the more common type of competition most people are used to seeing. We were given eight hours to complete as many challenges as possible from a wide variety of categories. 

Each challenge was initially worth 500 points but as more people completed them, the worth of the challenge would decrease. The CTF didn't require much prep day of, unlike the defence competition. This meant our team spent more time discussing who was focusing where.

### Challenge Writeups

There were a lot of challenges and I had worked on quite a few during the eight hour competition; solving a couple on my own, and working with my teammates to help solve a handful more. My main focus ended up being on the cryptography, OSINT, and hardware challenges.

| Challenge                                                            | Category         | Points |
|----------------------------------------------------------------------|------------------|--------|
| [Rigged Ballot Location](#rigged-ballot-location---osint-296-points) | OSINT            | 296    |
| [256](#256---crypto-100-points)                                      | Crypto           | 100    |
| [dot dot dot](#dot-dot-dot---crypto-427-points)                      | Crypto           | 427    |
| [staged](#staged---crypto-207-points)                                | Crypto           | 207    |
| [Badge 1](#badge-12---badge-100-points-each)                         | Badge (Hardware) | 100    |
| [Badge 2](#badge-12---badge-100-points-each)                         | Badge (Hardware) | 100    |

#### Rigged Ballot Location - OSINT 296 points

For this challenge, we were given the file, **BallotRiggers.jpg**, the image shown below. We were tasked with finding out who owned the compound, and using that information to get to the flag.

![](/assets/images/cybersci-nat25/BallotRiggers.jpg)

I initially started off by moving the file into my Kali Linux VM where I could easily run it through a handful of different tools and commands; including **zsteg**, **steghide**, and **exiftool**. 

- zsteg returned with nothing
- exiftool gave me a lot of information regarding the image, but nothing useful for the challenge
- steghide got stuck since I didn't have a password

From there I had moved over to Google where I could reverse image search the picture, and a few film review sites and wiki pages had come up regarding the 1985 film, _Commando_. I had sifted through a couple before my teammate and I both came across the site, [Filming Locations of Commando - MovieLoci.com](https://www.movieloci.com/4300-Commando?from=8&count=8&sortby=9&sortdirection=0)

It included the uncropped version of the picture we were provided, geographical information, and the name of who owned the compound in the film, _Arius_. 

With our new found information, I wasn't entirely sure of our next steps. My teammate assumed this couldn't be it as it seemed too easy, so I went back to my VM to give steghide another try. This time using the characters name as the passphrase, we were successfully able to retrieve this challenges flag.

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
for p in primes:
    phi *= (p - 1)

d = inverse(e, phi)                   # calculate private key, d

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

Sam and I tackled this one together because it  didn't seem like a lot till we realized just how many different possibilities of combinations of letters there are. Our initial step, since we were given the clue that the string 'CYBERSCI' was in it, was to confirm whether it was at the beginning or end of the string and work from there.

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

We did want to try another script, but we weren't sure we could automate it entirely and not take the rest of the competition time to run. We knew if we could speed up the process even just a little bit, it would make it 10x easier.

I wrote a script that took a list of common dictionary words, filtered out any that were less than three characters long, converted them to morse code without spaces, and compared it to the morse code we were given. This gave a list of 1073 matching words, including 'PROGRAMMERS', which at the time I hadn't realized was actually the last word in the string. 

Sam put together a script that would convert the morse code, string the letters together, compare it to a list of dictionary words, and then repeat. It was a little slow, but was actually pretty accurate and helped point us in a good direction. 

While Sam had that running, I was manually converting and trying different strings with a couple different morse code tools I found online:

- [Morse Code Translator](https://morsecode.world/international/translator.html), a very simple morse code translator that I could easily keep track of the string with.
- [Morse Code - dCode](https://www.dcode.fr/morse-code), a translator with a 'no spaces' and a 'brute force' option (one of my favourite sites for any decryption challenge).
- [UnMorse Code Solver - CacheSleuth](https://www.cachesleuth.com/unmorse.html), a decoder to step through it character by character and give you every possible combination.

We went back and forth with our mix of tools, and each time we were confident in the next word, we could confirm with each other, and then shorten the string we were working with. Eventually, after going through this process a few times, we got to the final string, 'CYBERSCI WHO NEEDS SPACES WHEN YOU HAVE PROGRAMMERS'.

```
-.-. -.-- -... . .-. ... -.-. .. .-- .... --- -. . . -.. ... ... .--. .- -.-. . ... .-- .... . -. -.-- --- ..- .... .- ...- . .--. .-. --- --. .-. .- -- -- . .-. ...
 C    Y    B   E  R   S   C   I   W   H    O  N  E E  D   S   S   P   A   C   E  S   W   H   E N   Y    O   U   H   A   V   E  P    R   O   G   R  A  M  M  E  R   S
```

#### staged - Crypto 207 points

The provided files for this challenge included, **cipher.txt**, a text file which was 44 lines of just under 4500 smiling and frowning emojis. 

This was the first challenge that both Sam and I had opened once the competition started, and he immediately had an idea so I swapped over to another challenge while he began to work on this one. It had a few steps along the way and was passed around quite a bit amongst our team.

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

I gave it another try from this point since when I had initially seen the results of step 4, I thought I had an idea where to go but it wasn't entirely working out. Returning to it later I had much better luck. I used the following script to decrypt the **5th and Final Step** of the challenge and retrieve the flag.
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

#### Badge 1/2 - Badge 100 points each

These challenges and two others required a physical badge that had:

- A screen
- Four buttons
- On/off switch
- Usb-c port
- A battery

Huge shoutout to everyone who worked on making them and worked hard to make sure they were ready the day of; they were super cool to mess around with. 

Jonathan and Sam initially looked over it, but from the vague challenge description and the fact they couldn't see/access much on it via their laptops, they moved onto another challenge. 

To take a break from the crypto challenges, I decided to take a look at it. There were a couple different sections within the device, one being a poll and another being like an admin portal. 

I immediately figured out how to access the admin section; it requested an eight button pattern and on a whim I entered the beginning of the Konami code (up up down down left right left right), which worked. I got in successfully, but I wasn't seeing much on the device itself. Even with the device plugged into my laptop, it wasn't being recognized whatsoever. I wasn't ready to give up on this one though because I could feel I was extremely close to something.

I passed the device back to Jonathan and told him I would find commands for him to try on his laptop while it was plugged in. The commands were something along the lines of the following.

```bash
dmesg | grep tty             # look for the device
screen /dev/ttyUSB0 115200   # replace with the device found
```

The commands above worked and Jonathan was able to begin seeing output in his terminal, with stuff we couldn't see originally: completing the poll gave us the flag for **Badge 1** and entering the admin section gave us the flag for **Badge 2**.

### Results

By the end of the eight hours, we had scored 2743 points, and finished 5th out of nine teams. Unlike regionals, I actually scored some points this time around and when I got stuck on challenges, I found it really nice to have people to bounce ideas off of which in-turn helped us finish a lot more challenges. 

---

## Overall Experience

In the end, our final score was 5511 points, and we finished 6th out of nine teams. To some this may seem low and not something to celebrate, but I'm very proud of how we did. I could see huge improvements in myself compared to my regionals performance, in both my soft and technical skills.

I'm a very anxious person and I find it gets worse when travelling or spending lots of time around people I've never met before. That meant, throughout the entire event, I would have to face those fears as I've never been to Ottawa before, and I was the only student attending from my school. 

And guess what, I did it. I went for five days so I could explore the city a little and settle in before we started everything Friday night. After that, I spent four days meeting a ton of new people and just getting to know everyone. Throughout the competition, I got to see what people loved to do, and after each day, I got to discuss with and hear from people about how they solved the challenges.

---

## Conclusion

Simply, if you ever get to experience something like this, DO IT. The opportunity to learn from and collaborate with so many different people was amazing. I had an absolute blast the entire trip, everyone was so kind and friendly, and I felt like I truly learnt a lot from the entire experience. Again, a huge thank you to everyone at CyberSci, everyone who helped organize the event, build the challenges, and all of the sponsors; without any of you guys, this wouldn't be possible.

![](/assets/images/cybersci-nat25/cybersciNat25-everyone2.jpg)
