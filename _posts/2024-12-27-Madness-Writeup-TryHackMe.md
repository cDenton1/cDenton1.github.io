---
title: "Madness Writeup - TryHackMe"
datePublished: Fri Dec 27 2024 02:16:39 GMT+0000 (Coordinated Universal Time)
image: assets/images/madnesswriteup.png
tags: [tryhackme, ctf-writeup, tryhackme-walkthrough]
read_time: "9 min read"
---

## Will you be consumed by Madness?

Happy holidays folks! On Christmas eve as a way to wind down for the night I hopped onto TryHackMe to work through one of the challenge rooms. Reading through some chats on the TryHackMe discord server showed me the room Madness, which is one I hadn’t seen before. Now it wasn’t my first option, I originally wanted to do the room Airplane but none of the websites looked to be up and working when I first started it so I moved on to give this room a try. It was really fun but also quite challenging at times, I hope you enjoy my writeup and walkthrough, and if you’re working through it, I hope it’s helpful.

---

## Reconnaissance

As I normally do for these types of challenges, I started off by pinging the machine and running an nmap scan using the command, `nmap -sV -T4 10.10.226.50`. -sV is used for service detection and -T4 is used for timing, it’s fast but maintains reasonable accuracy of the scan. The results from the scan were showing both port 22 for ssh and port 80 for http were open on the machine.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735232372156/e1eb5468-6952-41b3-a367-81ca67b5158d.png align="center")

My next step seeing that port 80 was open was to search the IP address of the machine in my internet browser. Hopefully it would be a site of some sort to put me on a path for this challenge but instead it just successfully brought up the default page for Apache2 Ubuntu, which is still a good sign to see that it’s up.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735232584989/2eb34341-6567-46c3-a1b3-f3a0b700cc36.png align="center")

To look for any possible directories for the site, I ran gobuster using the command, `gobuster dir -u` [`http://10.10.226.50`](http://10.10.226.50) `-w /usr/share/wordlists/dirbuster/directory-list-1.0.txt`. By the time it had hit 76% of the way through the list, it hadn’t found anything besides a few errors, and even once it finished it had found nothing. While I was initially waiting for gobuster to return some results I tried out some common pages like /login, /admin, and /robots.txt, as well as just words that seemed like they would fit the theme of the room since I wasn’t sure what the possible website would be for. Unfortunately I didn’t have any luck with any of them and they all returned 404 page not found.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735233392899/010493dd-c24e-4e6b-bdca-33dcd5a02faf.png align="center")

My next step in looking for and gaining more information was looking into the source code of the Apache site. When initially looking at the site, nothing looked unusual or out of place but that doesn’t mean the source code wasn’t hiding something. I’m glad I looked because a strange comment caught my eye, “They will never find me”, and it revealed a hidden JPG file, “thm.jpg”.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735234042650/5d6e87aa-5fc5-40c0-87b2-e341f61c1062.png align="center")

When trying to open and look at the JPG file in the browser, it didn’t work and instead just showed that it couldn’t be displayed as the file contained some errors.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735234353514/f40c8483-0021-4531-b8f1-e79ab10398b0.png align="center")

To get past this and see what was wrong with the file, I downloaded it to my machine using the command, `wget "`[`http://10.10.226.50/thm.jpg`](http://10.10.226.50/thm.jpg)`" -O thm.jpg`. Next, I ran the command `file thm.jpg` to determine the true file type or verify if the file was corrupted. The output returned 'data', indicating that the file was indeed corrupted. To get a better look at what was possibly wrong with the file I ran the command, `xxd thm.jpg`, which printed out the hexdump of the file. This showed that there was an error in the first few lines of the file since it was coming up with PNG.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735234804453/1fe1a803-5e98-4436-a42b-17351995b52f.png align="center")

To fix this issue, I used a hex editor to change a few of the bytes in the first line of the files’ metadata. This part of the metadata is also known as the file signature or the magic number. Editing the magic number allowed for the software to properly read the file, which in turn fixed the error and actually gave us an image when opening the file; revealing a hidden directory.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735235281535/5b415253-5214-4188-b646-13ba53382a97.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735235290257/c7a282b0-928a-4098-b5e1-12836675c43e.png align="center")

## Active Enumeration

Taking our new found information and going to the hidden directory gives us a very simplistic page that didn’t have much on it besides some text. The text informs us that we need to guess and enter this users’ secret to gain more information about them. Viewing the page source again shows another comment narrowing down what the secret could be, a number between 0 and 99.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735236163282/3ca6fba8-02bf-4f2d-adaa-6d27cac23fe2.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735236136711/fe1dc9af-3a2b-44a4-aa83-a11fc2830a06.png align="center")

From here there are three ways to go about finding this information:

1. You manually go through an enter each number yourself until to find the secret number - this tactic will be extremely slow and time consuming, and it is not recommended.
    
2. You use an application like Burpsuite to create the payload and automate the task for finding the secret number - automation is great and Burpsuite is a great tool, if you aren’t familiar with the tool it might take a little bit more time to figure it out, but once you do it’s a breeze.
    
3. You find or write a simple python program that automates each request and finds the secret number off of comparison - again automation is great but it might take some time to throw something like this together and you might not be sure what to look for when finding what the correct number is.
    

I went with the third option as I enjoy programming and wanted to test my knowledge of writing an automation script for something like this. To find the correct secret number, I compared the length of the response page. I assumed that if the guess was correct, the page would include more information, making it longer than the page stating the guess was wrong. From there, it would look through all options, comparing each one, and once it reached the end, it would print out the number with the longest length: the secret number.

```python
import requests

url = "http://10.10.226.50/th1s_1s_h1dd3n/?"     # base URL
numbers = list(range(100))                       # generate numbers from 0-99
longest_length = 0                               # set longest length to 0
secret_number = None                             # set secret number to none

for number in numbers:                           # iterate through the numbers and send as payload
    params = {"secret": number}                  # create the parameters for the secret key
    response = requests.get(url, params=params)  # use GET and create the request with the URL and parameters

    print(f"Trying number: {number}")            # print to the terminal what number is being tested

    # check if the response indicates the correct number
    if len(response.text) > longest_length:      # check response length for the longest length
        longest_length = len(response.text)      # if bigger than the longest, set it as the new longest
        secret_number = number                   # set the secret number

if secret_number is not None:                             # check if there is secret number
    print(f"Secret number found: {secret_number}")        # print the found secret number
else: 
    print("Secret number not found in the range 0-99.")   # print that no secret number was found
```

P.S. if you’re interested in seeing more scripts I make or find, I’ve started a GitHub repository where I store and keep track of these file and the challenges they were used in, [GitHub - cDenton1/extra\_ctf\_files](https://github.com/cDenton1/extra_ctf_files/tree/main).

After running the program and troubleshooting a few things (mainly spelling errors), on my final run after going through each number it printed out that the secret number that was found was 73.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735237847378/ea791a8d-201f-4df0-8ec9-f6596bac1010.png align="center")

Using the number that was found, I went back to the browser and adjusted the URL to include ?secret=73 at the end which gave me a new output along with a jumbled string of letters, y2RPJ4QaPF!B. It also hinted at the string most likely being the password of the user since they said, “I won’t tell you who I am!”.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735238045493/7afbdc91-d3e0-4271-900e-c4b6f2ca55c3.png align="center")

After this point I wasn’t sure where to go, running the string through different techniques on CyberChef gave me no information. I was really stuck at this point, so I did some research to try and find out other things I could try or look for with the information I had available to me, and this was when I learned about steganography. At the time of me attempting this challenge I had heard almost nothing about this, so with the little bit of new information I just gained, I gave it a try on the image found earlier. Running the command, `steghide extract -sf thm.jpg`, it asked for a passphrase, I entered the string I just found and it seemed to work as it gave me a file with the extracted data, hidden.txt. Running the command, `cat hidden.txt` showed the contents of the file, revealing the username, wbxre.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735238904757/7ec40b96-e3d4-4ac9-a0ae-c7e6542cd9ce.png align="center")

At the bottom of the file it read, “I didn’t say I would make it easy for you!”, which hinted that we most likely still needed to find more information. With that in mind, I put the string I had just found into CyberChef and tested out different techniques again, this time revealing the username to actually be, joker.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735239419536/d5b0d081-691a-4b0d-9fb5-704fc2f21609.png align="center")

Assuming the passphrase I used earlier was only meant for that one section of the challenge, I decided to try using the tool steghide again, but on the image in the challenge's task on the TryHackMe website. I got stuck when it asked for the passphrase but hitting enter without any input worked successfully and gave me another file. Running `cat password.txt`, revealed what was presumably the password for SSH.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735239684971/b0643dff-67dd-4908-ba98-dc6cb04586de.png align="center")

## Exploitation

From there I attempted to connect to the machine by SSH using the username, joker, and the password I had just found, and I was successful in gaining access into the machine. The first command I ran once I was in was ls, this revealed the first file I was looking for. The second I found the file, running cat on it was quick and easy for showing its contents and getting the first flag.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735240507260/2bae03d8-4ed6-4dfa-8c75-0ae565855d0d.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735240586428/57b6f8aa-2d72-4a7c-8896-ebaf5151f582.png align="center")

## Post-Exploitation

From there it was time to escalate my privileges to root, for this I ran the command, `find / -perm -4000 2>/dev/null`, which listed out files with the setuid permission bit set. The setuid bit set is often used for programs that need to perform tasks that require higher privileges, and is important for identifying potential vulnerabilities in a system.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735241264803/2388b634-959b-457f-b552-ef386d50523e.png align="center")

The results from the scan seemed a little suspicious and doing some research I found that screen-4.5.0 had a local privilege escalation exploit outlined on [https://www.exploit-db.com/exploits/41154](https://www.exploit-db.com/exploits/41154). I copied the code from the site into a file on the machine, and then ran `chmod +x exploit`. This turned the file into an executable which I could run using `./exploit`.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735241397415/c8f2b158-5695-4c0c-b80e-798e84298bec.png align="center")

It had some warnings when ran but entering the command, whoami, showed it to be successful as I was now the root sure. From there, doing some searching around revealed where the file storing the final flag of the challenge was and I was able to cat the file, getting its contents.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735265957685/17dac5fa-d98d-4395-9104-c8c6cd381f06.png align="center")

---

## Conclusion

That was a challenge I really enjoyed. It had taken me about two hours to complete, which isn't very long when you consider that I was writing some of the notes I used in my write-up and doing extra research. For my first time learning about steganography, I found it really interesting and will definitely spend some time during my winter break learning about it. I hadn't done a challenge like this in a while, so it was nice to have some time the other day to do one. There was surprisingly a lot more new information than I expected, but it's a good reminder to myself that I still have a lot to learn. This was a good challenge for expanding and testing my knowledge. If you're looking for an easier challenge, I would highly recommend this room, [https://tryhackme.com/r/room/madness](https://tryhackme.com/r/room/madness).
