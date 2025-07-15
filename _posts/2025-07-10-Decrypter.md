---
title: "Decrypter"
tags: [python, projects]
read_time: "7 min read"
---

# Decrypter

Decrypter is a modular command-line decrypting tool offering various techniques and the option of applying multiple methods step-by-step. Some techniques even include other options for brute forcing, digit shifting, and character substitution. Feel free to check out the repository if you're interested, [https://github.com/cDenton1/Decrypter](https://github.com/cDenton1/Decrypter).

Since returning from CyberSci Nationals I had a few different ideas for some projects to work on. I will be honest I was expecting this one to take me at least a month, since I normally only work on these projects in the evening after work. However, due to some unforeseen circumstances in my life, I had a lot of free time this past weekend and was able to finish it.

In this post I'm going to talk in-depth about the tool, and the future of it.

* * *

## Inspiration and Project Plan

For those who have read any of my CTF or cybersecurity event related posts, you might know I am a BIG fan of cryptography and encryption related challenges; especially when I get to use the tool CyberChef. I love the type of problem solving and thought process it requires, and the variety of tools and methods out there really expand on the possibilities.

Now CyberChef is an amazing tool, it offers so much, but sometimes I find it can feel like too much. I wanted to condense Decrypter down to what I use the most in CTFs and combine some of the techniques with extra options for solving problems I've had to figure out in the past. 

Making the tool with the main focus of it being modular also meant that other users could make their own mods to add, or limit what modules they use to keep it small. This would also mean expanding the tool would be much easier.

* * *

## Code

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

In between all of that it also calls the two functions that import the modules and creates the list used for the selection menu: **loadMods** and **dOptList**.

Once any options have been set or dealt with, and the modules have been imported, the script moves into a while loop that runs as long as the user's input doesn't equal 'e'. There are multiple if statements in this while loop for handling the options that are output, and handling the users input. 

For the option menu I setup a page system: 
- Prints at most five modules from the list
- Uses n/p for moving to the next page or back to the previous one
- Tracks the page number and only prints 'n' if it's not the last page, and only prints 'p' if it's not the first.

Each module prints along with the placement number of where it is in the list, plus one. Meaning five modules are listed 1-5 instead of 0-4. The placement number is what is used to select the method. When an option is selected, it calls the function, **callMod**.

### callMod

This function starts with a try except for calling the module, it maps the chosen module from a list made while importing the modules and only returns false if there is an error.

Once the module is called and handled, callMod asks the user if they want to continue with the new string, continue but revert back a step, or exit. As long as the user doesn't select exit, it returns to the main function and returns to the while loop previously explained.

### loadMods

This is the function that handles the dynamic loading of the modules. It checks the filenames and contents of all the files in the subfolder, /modules.

For the file to be imported, the filename must start with 'mod' and end with '.py' Then for it to actually be used and listed as an available module, it must include a callable 'conv' function.

### dOptList

After the modules are imported, the option list is created. This is done by reading a list of the successfully imported modules, appending a new list with item position in the list plus one, and the name of the module minus 'mod' at the beginning.

### Extra

Before any of the above mentioned functions, there are another three sections that aren't necessary for the code but definitely useful. 

#### Option Menus

Instead of using the default help menu that is available with the argparse module, I created my own script for it. I will eventually revamp it or move over to the default but I like the touch of character it gives right now. 

On top of that, I made a menu that lists all of the modules I've written for the program, along with a brief description for each. This just makes it easier for users to see what's available and how they work if they are unfamiliar with the method.

#### ANSI Mapping

For some of the printing for the option selections, I wanted them to stand out more in the terminal. Instead of retyping the ANSI codes constantly throughout my code, I predefined them at the top for easier, repeated use. This includes, red, blue, and the reset escape code.

#### OS Check

To ensure proper path check of the modules subfolder, there is an if statement that checks for posix or else, and creates the necessary path to the folder. This is to ensure no errors with importing the modules. As of now, I've only tested with Windows and Linux, and it works but I can't guarantee other operating systems.

* * *

## Decryption Methods

1. Base64 - Decode Base64 strings
2. ROT13 - Caesar Cipher; includes options for using a key, digit shifting, and brute forcing
3. Binary - Decode Binary strings; including options for strings not made of 0/1's
4. Hex - Decode Hex encoded strings
5. Hexdump - Decode strings from Hexdumps
6. URL Decode - Decode URI/URL percent encodings to raw values
7. Morse Code - Decode strings of morse code
8. XOR - Decipher a string with a known key or brute force
9. AtBash - Substitution cipher that reverses the alphabet
10. Octal - Decodes octal numbers to decimal and converts from ASCII

* * *

## Future of Decrypter

At the time of this post being made, technically I have released two versions on GitHub:

- v1.0.0 - The first official working release
- v1.0.1 - Included some minor bug fixes

There aren't that big of differences between the two versions, but my plan for the next release will hopefully add quite a lot more functionality to the tool.

### Method Detect

Similar to 'Magic' on CyberChef, *detect* will print the entropy of the string and test a few of the available techniques. It will also print out any results that seem either human readable or align with the typical layout/formula of another method (i.e. Base64).

### Method Search

Instead of a user needing to flip through multiple pages to find something they are looking for, *search* would allow the user to find the option number quicker. 

With only ten modules currently available, this isn't the most necessary but with the hope the tool continues to expand, adding this will improve the future usage.

* * *

## Conclusion

Decrypter is a modular command-line decrypting tool offering various techniques and the option of applying multiple methods step-by-step. The tool combines what I find to be commonly used in CTFs along with extra additions to create a simple yet expandable tool.

Building this tool was a great learning experience and something I genuinely see myself using in the future and continuing to work on. It included some things I had never used before and gave me the opportunity to better understand decryption methods and algorithms I had never written.

If you're interested in trying it out for yourself, check it out on GitHub, [https://github.com/cDenton1/Decrypter](https://github.com/cDenton1/Decrypter).
