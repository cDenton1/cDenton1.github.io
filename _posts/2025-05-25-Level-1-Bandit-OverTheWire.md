---
title: "Level 1 - OverTheWire Bandit"
tags: [overthewire, otw-bandit, hidden-1, hidden-2]
read_time: "1 min read"
---

# Level 1 - OverTheWire Bandit

To know more about OverTheWire, check out my post, [Bandit Writeup - OverTheWire](https://cdenton1.github.io/2025/05/19/Bandit-Writeup-OverTheWire.html). For these walkthroughs, I am using a Linux OS.

---

Previous Level: [Level 0 - SSH Login](https://cdenton1.github.io/2025/05/25/Level-0-Bandit-OverTheWire.html)

---

## Level Goal

The password for the next level is stored in a file called readme located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

## Solution

The task description is very simple and straightforward for this level and in the previous level you figured out how to access bandit0 using SSH. Hoping you didn't disconnect from the previous level before moving on, this will be pretty quick and only require 1 or 2 commands.

Again, if you don't know much about SSH or Secure Shell Protocol, I would recommend reading up on it a little to help with a better understanding. But I will briefly explain one thing, when you use SSH to log into a machine as a user, you will initially be in the home directory. For example, in our case we logged into bandit0, therefore we are in the home directory of the user, bandit0.

A couple ways to confirm where you are include:
* The command `pwd`, which outputs the current working directory
* Looking for `~` on the left near the username, this indicates the current working directory is the home directory

If you are logged into bandit0, simply running `ls` to confirm the required file is there, and then running the command `cat <filename>`, you will recieve the password for the next level.

![Terminal Output](/assets/images/otw-b0-1.png)
