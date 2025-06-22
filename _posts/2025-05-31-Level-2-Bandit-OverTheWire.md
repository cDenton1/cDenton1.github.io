---
title: "Level 2 - OverTheWire Bandit"
tags: [overthewire, otw-bandit, hidden-1, hidden-2]
read_time: "1 min read"
---

# Level 2 - OverTheWire Bandit

To know more about OverTheWire, check out my post, [Bandit Writeup - OverTheWire](https://cdenton1.github.io/2025/05/19/Bandit-Writeup-OverTheWire.html). For these walkthroughs, I am using a Linux OS.

---

Previous Level: [Level 1 - File Reading](https://cdenton1.github.io/2025/05/25/Level-1-Bandit-OverTheWire.html)

---

## Level Goal

The password for the next level is stored in a file called - located in the home directory

## Solution

This level from a first glance might seem very easy, however, if you don't have some experience working with a Linux OS, you may have never seen something like this. `-` is most often seen when used for setting flags/options with a command, for example `-p` to set a port for `ssh`.

Running the command `ls` will show that the file we're looking for is in the home directory, and similar to the last level, you might run the command `cat <filename>` to see what's in it. When running `cat -` though, `cat` will read the `-` as STDIN and wait for user input.

The way we'll get around this is by including the file path before the filename, in our case `cat ./-`. Running this, you will recieve the password to the next level.

```
bandit1@bandit:~$ ls
-

bandit1@bandit:~$ cat ./-
[password]
```

---

Next Level: [Level 3 - Spaces In a Filename](https://cdenton1.github.io/2025/06/21/Level-3-Bandit-OverTheWire.html)

---
