---
title: "Decrypter"
tags: [python, projects, hidden-1, hidden-2, hidden-3]
read_time: "__ min read"
---

# Decrypter

Decrypter is a modular command-line decrypting tool offering various techniques and the option of applying multiple methods step-by-step. Some techniques even include other options for brute forcing, digit shifting, and character substitution. Feel free to check out the repository if you're interested, [https://github.com/cDenton1/Decrypter](https://github.com/cDenton1/Decrypter).

Since returning from CyberSci Nationals I had a few different ideas for some projects to work on. I will be honest I was expecting this one to take me at least a month, since I normally only work on these projects in the evening after work. However, due to some unforeseen circumstances in my life, I had a lot of free time this weekend and was able to finish it.

In this post I'm going to talk in-depth about the tool, and the future of it.

## Inspiration and Project Plan

For those who have read any of my CTF or cybersecurity event related posts, you might know I am a BIG fan of cryptography and encryption related challenges; especially when I get to use the tool CyberChef. I love the type of problem solving and thought process it requires, and the variety of tools and methods out there really expand on the possibilities.

Now CyberChef is an amazing tool, it offers so much, but sometimes I find it can feel like too much. I wanted to condense Decrypter down to what I use the most in CTFs and combine some of the techniques with extra options for solving problems I've had to figure out in the past. 

For example, at Nationals, we were given binary but instead of 0/1's, it was frowny and smiley faced emojis. In Decrypter I wanted to build in the option for character substitution in case the user encountered something similar.

Making the tool with the main focus of it being modular also meant that other users could make their own mods to add, or limit what modules they use to keep it small.

## Creation Process and Code

There is simply one main python script consisting of less than 200 lines of code. This file includes the logic for dynamic importing of modules, calling the modules, the menu pages, and the handling of different command arguments. I wanted this file to focus on how the tool would be used and leave the more technical aspects to each module's own script.

### main

The main function has three really important aspects, and I would say handles the bulk of everything.

The first part consists of parsing the command arguments and dealing with any command options that may have been used. Currently, the program has four options:

| Options      | Description                                                     |
|--------------|-----------------------------------------------------------------|
| -f           | File input, read from a given file                              |
| -o           | File output, write steps and decrypted strings to a dated file  |
| -m           | List modules and info                                           |
| -h           | Help menu                                                       |

In between all of that it also calls the two functions that import the modules and creates the list used for the selection menu.

Once any options have been set or dealt with, and the modules have been imported, the script moves into a while loop that runs as long as the users input doesn't equal 'e'. There are multiple if statements in this while loop for handling the options that are output, and handling the users input. 

For the option menu I setup a page system: it prints five modules from the list and uses n/p for moving to the next page or back to the previous one. Along with that, it tracks the page number and only prints 'n' if it's not the last page, and only prints 'p' if it's not the first.

Each module prints along with the placement number of where it is in the list, plus one. Meaning five modules are listed 1-5 instead of 0-4. The placement number is what is used to select the method. When an option is selected, it calls the function, callMod.

### callMod

This function starts with a try for calling the module, 

