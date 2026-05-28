---
title: "Level 5 - OverTheWire Bandit"
tags: [overthewire, otw-bandit, hidden-1, hidden-2]
read_time: "2 min read"
---

# Level 5 - OverTheWire Bandit

To know more about OverTheWire, check out my post, [Bandit Writeup - OverTheWire](https://cdenton1.github.io/2025/05/19/Bandit-Writeup-OverTheWire.html). For these walkthroughs, I am using a Linux OS.

### Level Goal

The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed up, try the “reset” command.

### Solution

For this level we are moving away from naming conventions and instead dealing with specific file types; in this case human-readable.

From the level description we know the directory where the file is located, so we first move there running the command `cd inhere`.

Once in there, running the command `ls` will list everything in the directory. There are ten files in this level, all named pretty similarily but we only one want.

Using the command `file ./-file##` (change the hashtags to numbers), it will display the type of file.

Human-readable is often listed as `ASCII text`, so we need to use the `file` command and cycle through each one until we find the one we are looking for.

The human-readable one is `-file07` and once we figure that out, we can run the command `cat ./-file07` to output the next levels password.

**NOTE**: the reason we are including `./` in front of the filename in each command is due to the `-` at the beginning of it. If we were to run the commands without it, we would get errors about no file existing.

```
bandit4@bandit:~/inhere$ ls
-file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09

bandit4@bandit:~/inhere$ file ./-file00
./-file00: data

...

bandit4@bandit:~/inhere$ file ./-file07
./-file07: ASCII text

bandit4@bandit:~/inhere$ cat ./-file07
[password]
```

---

<div style="display: flex; justify-content: space-between;">
  <span>Previous Level: <a href="https://cdenton1.github.io/2025/06/21/Level-3-Bandit-OverTheWire.html">Level 3 - Spaces in a Filename</a></span>
  <!-- <span>Next Level: <a href="...">Level 6 - File Characteristics</a></span> -->
  <span>Main Post: <a href="https://cdenton1.github.io/2025/05/19/Bandit-Writeup-OverTheWire.html">Bandit Writeup - OverTheWire</a></span>
</div>
