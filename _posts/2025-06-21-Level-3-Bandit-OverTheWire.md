---
title: "Level 3 - OverTheWire Bandit"
tags: [overthewire, otw-bandit, hidden-1, hidden-2]
read_time: "1 min read"
---

# Level 3 - OverTheWire Bandit

To know more about OverTheWire, check out my post, [Bandit Writeup - OverTheWire](https://cdenton1.github.io/2025/05/19/Bandit-Writeup-OverTheWire.html). For these walkthroughs, I am using a Linux OS.

---

Previous Level: [Level 2 - Accessing Unusually Named Files](https://cdenton1.github.io/2025/05/31/Level-2-Bandit-OverTheWire.html)

---

## Level Goal

> The password for the next level is stored in a file called **spaces in this filename** located in the home directory

## Solution

Just like the last level, we are continuing to deal with naming conventions within the Linux OS. In most cases, you will see spaces used for separating arguments or options in commands.

Running the command `ls` will show that the file we're looking for is in the home directory, and just like the last few levels, you will probably run the command `cat <filename>` or something similar to see what's in it. When running `cat spaces in this filename` though, `cat` will read each word in the command line as a separate file.

The way we'll get around this is by wrapping the filename in single or double quotes, the command would look something like `cat "spaces in this filename"`. Once ran, you will get the next levels password.

```
bandit2@bandit:~$ ls
spaces in this filename

bandit2@bandit:~$ cat "spaces in this filename"
[password]
```

---

Next Level: Level 4 - Hidden Files

---
