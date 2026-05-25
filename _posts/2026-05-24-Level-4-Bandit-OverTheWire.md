---
title: "Level 4 - OverTheWire Bandit"
tags: [overthewire, otw-bandit, hidden-1, hidden-2]
read_time: "1 min read"
---

# Level 4 - OverTheWire Bandit

To know more about OverTheWire, check out my post, [Bandit Writeup - OverTheWire](https://cdenton1.github.io/2025/05/19/Bandit-Writeup-OverTheWire.html). For these walkthroughs, I am using a Linux OS.

---

Previous Level: [Level 3 - Spaces In a Filename](https://cdenton1.github.io/2025/06/21/Level-3-Bandit-OverTheWire.html)

---

## Level Goal

> The password for the next level is stored in a hidden file in the inhere directory.

## Solution

Similar to the previous levels, we are dealing with Linux naming conventions; specifically for hidden files.

Hidden files within Linux are typically named starting with a period (`.`), so running `ls` normally will not list them.

From the level description we know the directory where the file is located, so we first move there running the command `cd inhere`.

Then running the command `ls -a` or `ls -A` will list everything in the directory, including anything hidden.

Once we figure out the name of the hidden file and run the command `cat ...Hiding-From-You`, we will get the password for the next level. 

```
bandit3@bandit:~/inhere$ ls -a
.  ..  ...Hiding-From-You

bandit3@bandit:~/inhere$ cat ...Hiding-From-You
[password]
```

---

Next Level: Level 5 - Human Readable Files

---
