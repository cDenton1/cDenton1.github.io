---
title: "My Learning During Winter Break"
image: 
tags: [projects, tryhackme]
read_time: "14 min read"
---

My classes started today meaning my winter break is over. During my winter break I have had some ideas for future projects and have messed around with a few different things just to test and learn. I will expand on and dive into all of these in this article. Everything I discuss and go over in here seemed too short to be their own posts, which is why they are all in here. Think of it like a collection of short stories but it is just things that I’ve been doing in my free time for the past few weeks.

---

## Old Windows 8 PC

Ages ago, I had a PC that was shared between my brother and I. It wasn’t used for very much besides playing popular flash games since we were both pretty young and not needing it for school. At some point it had gotten a virus due to my brother downloading what he thought was a free game. From there it sat in our basement collecting dust, but back in September, I had an idea; why not pull it out and clean it up?

It doesn’t have that good of hardware which was to be expected for its age but from what I could remember it ran just fine and getting some of the dust out of it would also help. I don’t have any memory of if it ever got wiped and when asking my parents they didn’t seem to remember either. Plugging it in and booting it up gave me the the answer to that question though, as it required a password to get in. Doing some research I figured out how to access the BIOS settings while it was booting up, and from there I deleted the operating system, wiped anything that was still on the computer, and redownloaded the Windows OS.

Now I have a semi-decent PC running in my room but that was only the easy part. Even once I could get into the PC and use it, I couldn’t do much as it required ethernet or dial-up to gain access to the internet. And without the internet there isn’t much to do on the PC since I had wiped everything on it so it only had whatever came with the redownloaded operating system. I couldn’t do either option due to what I had available to me so I had to figure out something else. From here I had two ideas on how to solve this issue, both required materials and tools that I purchased for my wireless security class in school already, so neither of these required buying anything else.

| **Ideas/Methods** | **Pros** | **Cons** |
| --- | --- | --- |
| Setup a travel router in client mode, then plug an ethernet cord from it to the PC, giving it access to the wi-fi and essentially making it wireless. | Get more use out of hardware that I already have. More control and customization over how the device is connecting to and accessing the internet. | Extra hardware and devices being used, plugged in and connected to the wi-fi. The device and the setup would take up more space and power. Longer time setting up the internet connection for the PC. |
| Plug a wireless USB adapter into the PC itself, giving it the option for wireless connectivity. | Small amount of extra hardware being used. Getting more use out of hardware that I already own. Hardware doesn’t require it’s own constant power source besides being plugged into the PC for it to work. Hardware doesn’t require any, if not very much, extra setup to work. | If I require the tool for future labs then I have no internet access on the PC during that duration. Device adds extra length to the PC making it awkward to store under my desk and is at risk of being easily broken. |

I went with the second one as the pros heavily outweighed everything else: it was easier to setup, get working, and will take up the least amount of space in my already very crowded desk area. Setting it up was simple as it only required plugging in the device, my PC luckily recognized it right away and immediately gave me the option for wi-fi in my network connections.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1735870242799/b09d7bf2-31b5-4f70-8760-b4d32931fac1.png align="center")

If it hadn’t shown me the above, it did come with a small disk that I would have inserted into the disc slot on the PC to download the required drivers for it to work but luckily that wasn’t required.

Now originally this PC had Windows 8 and without being able to login prior to wiping it, the only way I knew that was by a small sticker on the side of it that said Windows 8. I noticed though that after wiping it, it had Windows 10 instead of Windows 8, and due to how I reinstalled the OS, that depended on the recovery partition. I did some more research into this, which is how I learned from a Microsoft Blog post back in July of 2015, Windows 10 was a free upgrade to anyone with Windows 7, 8, or 8.1. So I believe it was upgraded just before we no longer used it as there would be no other way for it to be the OS in the recovery partition.

The PC runs pretty slow which was to be expected but I think moving forward I want to slowly replace hardware in it, maybe even upgrading it to Windows 11. This is most likely a very slim chance due to the fact it was running Windows 8 originally so I don’t even know if it would be able to run it, but we’ll see.

---

## Scripting for Tool Construction Final Project

My final project for my scripting for tool construction class was by far one of the biggest projects and programs I have ever written and worked on. I wanted to write about it here but I noticed a few weeks after finishing and submitting the final working product, it no longer ran properly. Now there is a lot to go over for that project and I want to focus on it in it’s own article, so in this one I’m only going to go over those errors I dealt with and how I fixed them to get it running properly again.

If you’re interested in knowing more about the project stay tuned for my post about it or check out the code on GitHub, [GitHub cDenton1 - Password Analyzer & Manager](https://github.com/cDenton1/passwordAnalyzerManager/tree/main).

### Error 1: Partially Initialized Module

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736141204405/227a249d-6651-4d25-9491-49a6b2aa5e50.png align="center")

I noticed when I went to run the program to test a few small adjusts I made to the code, it was giving me the above output. I was originally really confused because the second function defined in the program is my analyze section as it’s a pretty important part of the overall program. On the right side it mentions a circular import though, this is something I’ve never heard before so I had to do some research.

A circular import is caused when two or more programs import and depend on each other causing a continuous loop. An example of what that could look like it shown below, it’s also basically what my program looked like.

```python
# in the analyzeCode.py
import guiCode

# and in the guiCode.py
import analyzeCode
```

I’m not sure why it wasn’t an issue until after submitting my final but I’m glad it was so it actually gave me time to learn what and how to fix it. Easiest way for me to fix it was by removing the import guiCode from my analyzeCode, thankfully there was only one function that relied on it but it was literally commented ‘NO LONGER IN USE’ above it so it wasn’t much to get rid of it.

```python
# function to get users input * NO LONGER IN USE *
def get_input():
    password = guiCode.input_box.get()              # store user input
    print(f"The password entered: {password}")      # print entered password out
```

### Error 2: Not Exiting the Program Properly

Another issue I noticed when running the program happened actually when I no longer wanted to run it; when closing the program, it would immediately re-open itself. Now this could be caused by a handful of different issues, I had two areas though I wanted to focus my attention on.

**Area 1: Functions Relating to Changing Sections**

```python
# functions to deal with what is showing on the GUI
def a_sect():
    global section 
    section = 1
    change_sect()

def m_sect():
    global section
    section = 2
    change_sect()

def change_sect():
    global section

    # section 1 = analyzer
    if section == 1:
        # showing and hiding elements

    # section 2 = manager
    elif section == 2:
        # showing and hiding elements
```

\* Note, I removed majority of what’s in the function shown at the bottom to avoid large amounts of unnecessary code, any updates will be shown but most of the original logic will not included here

The option for this section is to add flags to prevent recursive calls. There would be a global function that would be set to False, if the section of the program was to change from either the analyzer or the manager, it would set the flag to True until it was done. The code would look something like below.

```python
def change_sect():
    global section, sectionChangeProg

    if sectionChangeProg:
        return
    
    sectionChangeProg = True

    try: 
        # section 1 = analyzer
        if section == 1:
            # showing and hiding elements

        # section 2 = manager
        elif section == 2:
            # showing and hiding elements

    finally:
        sectionChangeProg = False
```

**Area 2: Functions Relating to the Master Password**

```python
def masterPass():
    global manager
    
    # create page to input master password
    manager = tk.Toplevel()
    manager.wm_title("Enter Master Password")
    manager.geometry("300x150")
    manager.configure(bg="gray")

    # input box for password
    masterPass_input = tk.Entry(manager, width=35)
    masterPass_input.place(x=15, y=50)

    # submit button to analyze users input
    masterPass_submit = tk.Button(manager, text="Enter", command=lambda: returnMasterPass(masterPass_input))
    # masterPass_submit = tk.Button(manager, text="Enter", command=manageCode.check_input)
    masterPass_submit.place(x=250, y=48)

    # label to request user to enter their master password
    enter_label = tk.Label(manager, text="Enter your Master Password:", background="gray")
    enter_label.place(x=15, y=25)

    # label to request user to enter their master password
    note_label = tk.Label(manager, text="*WARNING: First time entering your master password\n will set the password, keep track of it!*", background="gray")
    note_label.place(x=5, y=90)

    manager.wait_window()  # Wait for the password window to close
    return mastPassCheck
```

The option for this area of the code was to add a function inside the main function handling the master password; it would close the window and call the other function for checking the master password. This would be of instead of calling to a separate function to call the other function, it would skip the extra step. The only edits needed for the code shown above, are shown below; add the new function and change the command of the submit button.

```python
    def close_manager():
        global mastPassCheck
        mastPassCheck = manageCode.check_input(masterPass_input)
        manager.destroy()

    masterPass_submit = tk.Button(manager, text="Enter", command=close_manager)
    masterPass_submit.place(x=250, y=48)
```

Very anti-climatic, neither of these areas were the issue and it was actually also caused by a circular import error in my code. It was easily fixed by removing an import line from one of the other python scripts which wasn’t necessary for the code anyways.

Now the program is back to working like normal! If you want to know more about what it is and does, stay tuned for my post about it or check out the code on GitHub, [GitHub cDenton1 - Password Analyzer & Manager](https://github.com/cDenton1/passwordAnalyzerManager/tree/main).

---

## Advent of Cyber 2024

I originally started by posting articles for the first four days but I stopped after the fourth because on top of the holiday season, working, and finishing up final projects and exams, I didn’t have a ton of time. I did end up deleting the four articles I did put out but prior to that I wanted to highlight my favourite things I got to work on and practice and the new things I learnt.

### **Day 1: Maybe SOC-mas music, he thought, doesn't come from a store?**

The learning objectives from day one included: Learning how to investigate malicious link files, learning about OPSEC and OPSEC mistakes, and understanding how to track and attribute digital identities in cyber investigations.

I enjoyed getting to practice my researching skills, looking for small clues that could possibly lots of info. Using something like a YouTube to MP3 converter website, then checking out the file info in our terminals, before searching the internet for Mayor Malwares GitHub to read through a PowerShell script was pretty neat and gave me the chance to work on skills I already had but in a more realistic way. This task was pretty simple but also taught some big lessons in just general internet safety.

### **Day 2: One man's false positive is another man's potpourri.**

Day two focused on: Log analysis, making the decision between True Positives or False Positives, and helping Wareville’s SOC team differentiate TPs from FPs.

This was actually a great day for initially learning about ELK and using it in a real-world scenario. The day of this challenge was actually around the time I was given a lab in class where I had to use that same application, so getting to experience the interface prior was really nice. This task also introduced CyberChef which is one of my favourite tools and I love getting to work and practice which was awesome.

### **Day 3: Even if I wanted to go, their vulnerabilities wouldn't allow it.**

The learning objectives from the day three challenge included: Learning about Log analysis and tools like ELK, learning about KQL (Kibana Query Language), and how to use it when using ELK, and learning about RCE (Remote Code Execution), and how it can be done via unsafe/unrestricted file uploads.

This was sort of a continuation from yesterday with the log analysis but it also included some new red team tasks and a website to explore which was a nice add-on. I enjoyed getting a lot of explanation as I read through the room and I enjoyed the questions having very little which made me really think for myself and put these new found skills to the test.

### **Day 4: I’m all atomic inside!**

The learning objectives for day four were: Learning how to identify malicious techniques using the MITRE ATT&CK framework, learning about how to use Atomic Red Team tests to conduct attack simulations, and understanding how to create alerting and detection rules from the attack tests.

This had some more overlap to what I had been learning in one of my classes at the time so it was a great opportunity to see more of that outside of the labs and the classroom. It was also a great chance to learn about and get familiar with some common programs used in industry.

### Non Article Days

**Day 20: If you utter as so much as one packet…**

The task for this day was in an area I’ve probably had the most amount of practice in, investigating network traffic with Wireshark. Along with this I learned some new things like, identifying indicators of compromise and understanding how C2 servers operate and communicate with compromised systems. This task was pretty quick, gave me the opportunity to practice skills I already had while also teaching some more, and I got to use CyberChef again.

**Day 23: You wanna know what happens to your hashes?**

Th task for this day was related to hashes, something I’ve also spent some time learning in school and love practicing. This day did teach me some new tools related to hash cracking that I hadn’t used before which was cool to tryout for the first time. It was pretty simple day as I’ve learnt about hash cracking before but was a great chance to also learn a little more.

---

## Upcoming Semester

To not completely set my personal projects and hobbies on the back burner this upcoming semester and become very school/work orientated, I’ve set a few goals for myself:

1. Finish this semester with no lower than a 3.7 GPA - I would like to keep my classes as my main focus without them taking over and I’m currently sitting at a 3.76 going into this semester.
    
2. Get my drivers license before the end of the semester - Driving has been a small fear of mine which has been stopping me from going through with this but I’m looking to signup for driving lessons to achieve this goal.
    
3. Write and post at least one article to the blog every month - I don’t want to completely disappear while in school, so this will push me to either study with online resources, compete in more competitions, or spend some time completing other projects.
    

---

## Conclusion

Winter break was a great chance for me to relax and focus on some other projects besides school. I had the time to actually setup and troubleshoot an old PC, problem solve issues for hardware and for a final project I wrote, and complete online festive training. To anyone who has started school this week or starts the following one, best of luck to you with your studies! I find January is a hard month for me to want to do anything as it feels like it’s always dark and cold, but I’m going into this semester (and year) optimistic. Have a good rest of your week and take it easy!
