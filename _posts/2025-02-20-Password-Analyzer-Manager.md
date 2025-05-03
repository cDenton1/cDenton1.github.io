---
title: "Password Analyzer & Manager"
image: assets/images/passwordanalyzermanager.png
tags: python, python-programming, password-manager, python-projects, password-security
read_time: "9 min read"
---

My final project for my Scripting for Tool Construction class was by far one of this biggest projects and programs I have ever written and I wanted to go in depth about it, sharing more here. If you are interested in downloading and using it for yourself after reading about it, check out my GitHub for the majority of the files and some info regarding setting it up, [https://github.com/cDenton1/Password-Analyzer-Manager](https://github.com/cDenton1/Password-Analyzer-Manager).

---

## Scripting for Tool Construction

This class was focused on teaching us Python and applying what we were learning to creating cybersecurity tools. There were eight different labs that focused on a handful of different topics and incorporated knowledge we had learnt in some of our previous classes like networking. On top of the labs we had also worked on one massive project all semester long; we had to come up with a cybersecurity related tool and work on it on our own time, checking in with our instructor every few weeks.

My initial project idea was just a password analyzer. The user would enter a password, it would check it against a handful of criteria, and then print out a percentage score regarding its strength. Realizing how simple this idea alone was, I changed it to combine it with a password manager. After the user checks the strength of a password they would then have the option to save the password in the application along with the name of the service they’re using it for. I really liked how the project turned out, since submitting the final product and finishing the class though I have continued to make tweaks to it, constantly improving small details making it even better.

My cybersecurity reasons behind going with this idea was that in our world’s current reality, websites don’t properly check the strength of a users password. They check for very minimal things, they don’t check to an extent that should be, and sometimes they even include strength score meters that focus on components they deem strong but actually aren't. My program offers actual feedback on the password and a strength score that aligns with what makes a password strong. Built alongside a local password manager that is also password protected and uses an encrypted data file.

---

## Diving Into The Code

The program is made up of three main Python files, analyzeCode.py, guiCode.py, and manageCode.py. Each file is focused on one of the three main components of the project: the analyzing criteria, the generated user interface (GUI), and the password managing. There are a handful of other files as well, and I will touch on those ones later but for now I want to break down these main three first.

### Password Analyzer

This is the first thing you see when opening the program. You are met with an input box for the password you want to check, a large space for the results to output, the option to save the password at the bottom, and the option to export the output results.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1738608944120/11361529-7016-428c-93cd-ed13230cb8f6.png align="center")

When you enter a password and hit analyze, after a few seconds, the password along with the results regarding each of the ten criteria checks are output to the screen. Each check includes a score out of ten, which are then added together and displayed at the bottom. The password shown above for example had a score of 67%. The ten criteria checks include:

1. Length of the password
    
2. Variation in lowercase letters, uppercase letters, special characters, and numbers
    
3. The use of common substitutions of letters (ex. 3 is used instead of e)
    
4. The use of dictionary words
    
5. The use of names
    
6. The use of places
    
7. The use of dates
    
8. The use of numerical or alphabetical strings (ex. 12345 or abcde)
    
9. Comparison to rockyou.txt
    
10. Comparison to previous input
    

The logic for the first two criteria are very simple compared to the rest of the program, the password length is checked and ran through some if statements deciding the score. Then each character is checked and counted towards being either an uppercase or lowercase letter, number, or special character. Those tallies are also checked through some if statements determining the score in that section.

After the second criteria, common substitutions are made to the password and that string is stored separately to also be checked against. Since common substitutions are starting to become pretty popular in passwords I thought it would be important to be considered in the strength but it doesn’t have as much of an effect on the score compared to the original string.

Criteria 3-8 rely on a class for the comparing before they determine the score, each file is read through and stored in a list, it then runs through some if statements. For a word in the list to be checked against the password they have to be greater than a length of 4 or 5 depending on which criteria check it is, and shorter than or equal to the length of the password. For each word that matches, it takes off 1 point from that sections score or 0.5 if it only matches with the substitution string.

The last two criteria sections don’t use the class mentioned above because rockyou.txt is a massive file and also because the previous input file isn’t a regular plaintext file; so neither work with the premade class, making it easier to just have their own comparison logic in each of their own functions.

### Password Manager

At the top of the GUI next to the analyzer header, there is a button that switches over to the manager section, clicking it will prompt you for the Master Password to access saved passwords and a blank output screen so can’t bypass the authentication.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1740066262193/f23e4257-4d47-44a3-a5b7-694f30662990.png align="center")

If it’s your first time using the manager section, whatever password you enter will be set as your master password and a popup message will let you know that. Once it’s set if you try to access it later and you enter the wrong password, an error message will popup letting you know it’s incorrect, and kick you back to the analyzer section. If you enter the right password however, it will welcome you back and update the output section to include any saved passwords, except your master password. The email password shown above is just an example of what the output would look like.

Now if you were to hit the ‘Save Password’ button shown at the bottom of the analyzer section screen, you would get a popup requesting for the service the password is used for. After entering the service, it would appear in the manager section. For example the test password shown above would look something like this:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1740067312751/86bbd462-29de-4d16-97c2-9462b5f64550.png align="center")

Currently, I’m trying to figure out and change this output so that the passwords are all stars here but if you were to hit a button for example, a timed popup would appear with the password so that it’s not constantly shown in plaintext like this whenever you are in the manager section.

### Generated User Interface

The main visual components are controlled by a program file dedicated only to the GUI, some of the popups are controlled via the other sections I just previously discussed but the majority is from this file. If you were to download and use this product yourself, this is the file you would run to start and use it.

It’s not a massive file, and the code is pretty simple. There are a few functions that handle the master password popup and the logic behind storing and checking it, and there are also a couple of functions that handle the changing between the analyzer and manager sections. Other than that, the majority of the code is used for creating the window and canvases, and adding each widget and their respective functions.

There isn’t much to say about this section; the most complex part is knowing when and how to hide or show each widget as needed. Besides that, everything is pretty simple to pick up and understand. This program file, as well as the other two, is filled with a handful of comments to ensure anyone wanting to use or make adjustments to any of the code can easily do so. All the screenshots previously shown and discussed are pretty much everything you will see when using this project. I tried to stick with a very simple design for those who aren’t super tech-savvy, which I think I did well in.

### Extra Program Files

Besides the three main files discussed above, I had to create a small handful of other files that were used to adjust the text files I used for the data (i.e. the text files that were checked and compared in each of the criteria checks).

Most of the files that I was using, I had pulled from GitHub repositories that just had a bunch of long lists of different categories and topics. These files also either included more than what I was looking for and/or were CSV files instead of text files, so I made a few different extra Python files that read through those original files and wrote what I was looking for to a new text file. I had also made another one that generated the text file that listed the dates.

These files aren’t necessary for the running of the program but they were useful and very helpful in the overall creation of it, if you are curious at all about them I did include them in an extra files section of the main repository on GitHub, [Extra Files](https://github.com/cDenton1/Password-Analyzer-Manager/tree/main/extraFiles).

---

## Future Updates and Adjustments

Obviously as time goes on, what makes a password strong will change and this project will either become obsolete or it will be updated to check to the standards at the time. I’m hoping that maybe this is something I will keep up with, especially as I continue to learn. Some of the ideas I already have include:

1. Like mentioned earlier, change the passwords from constantly being plaintext in the password manager to being hidden by a button. When the button is pressed, a popup containing the password would appear so the user could copy and paste it as needed. This popup could maybe even be timed so that it doesn’t stay open for too long on the users screen.
    
2. Locking up the encrypted data file so that if by chance it’s stolen by somebody who has gained unauthorized access to the users machine, they can’t read it through the password manager or by attempting to read it through a text editor.
    
3. Updating the check criteria to either add more or so they more align with what currently makes a password strong. An example of this could be the length check, initially when making this, twelve was the minimum recommended length but even now and as time goes on, it will get longer.
    

---

## Conclusion

This was a great opportunity for me to learn more about password security, encryption methods, and application designing and planning. I’m really happy with how the project turned out, and even though I already have a handful of ideas for how to improve it, I think where it’s at currently is good. I spent four months planning it out and programming it, and I’m really proud of the finished product.

If you’re interested in checking it out and trying it for yourself, check out my GitHub to download the necessary files, [Password Analyzer & Manger](https://github.com/cDenton1/Password-Analyzer-Manager).
