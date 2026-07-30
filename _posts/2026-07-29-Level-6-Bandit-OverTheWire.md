---
title: "Level 6 - OverTheWire Bandit"
tags: [overthewire, otw-bandit, hidden-1, hidden-2]
read_time: "2 min read"
---

# Level 6 - OverTheWire Bandit

To know more about OverTheWire, check out my post, [Bandit Writeup - OverTheWire](https://cdenton1.github.io/2025/05/19/Bandit-Writeup-OverTheWire.html). For these walkthroughs, I am using a Linux OS.

### Level Goal

The password for the next level is stored in a file somewhere under the **inhere** directory and has all of the following properties: human-readable, 1033 bytes in size, and not executable.

### Solution

Similar to the last level of dealing with specific filetypes, for this one we are focusing on specific file characteristics.

From the level description we know the directory where the file is located, so we first move there running the command `cd inhere`.

Once in there, running the command `ls` will list everything in the directory. However, for this level it will benefit us to run `ls` with some additional flags.

Running `ls -alR` will provide a lot more results, with a lot more info:

- `a` displays hidden files
- `l` displays detailed information
- `R` does recursive listing, so listing anything in subdirectories and so on

In the returned results we are looking at the column just before the date, reading the size and looking for 1033 bytes.

Eventually identifying the file `./maybehere07/.file2` and using `cat` to output its contents.

```
bandit5@bandit:~/inhere$ ls -alR
.:
total 88
drwxr-x--- 22 root bandit5 4096 Sep 19 07:08 .
drwxr-xr-x  3 root root    4096 Sep 19 07:08 ..
drwxr-x---  2 root bandit5 4096 Sep 19 07:08 maybehere00

...

./maybehere07:
total 56
drwxr-x---  2 root bandit5 4096 Sep 19 07:08 .
drwxr-x--- 22 root bandit5 4096 Sep 19 07:08 ..
-rwxr-x---  1 root bandit5 3663 Sep 19 07:08 -file1
-rwxr-x---  1 root bandit5 3065 Sep 19 07:08 .file1
-rw-r-----  1 root bandit5 2488 Sep 19 07:08 -file2
-rw-r-----  1 root bandit5 1033 Sep 19 07:08 .file2
-rwxr-x---  1 root bandit5 3362 Sep 19 07:08 -file3
-rwxr-x---  1 root bandit5 1997 Sep 19 07:08 .file3
-rwxr-x---  1 root bandit5 4130 Sep 19 07:08 spaces file1
-rw-r-----  1 root bandit5 9064 Sep 19 07:08 spaces file2
-rwxr-x---  1 root bandit5 1022 Sep 19 07:08 spaces file3

...

bandit5@bandit:~/inhere$ cat ./maybehere07/.file2
[password]
```

---

<div style="display: flex; justify-content: space-between;">
  <span>Previous Level: <a href="https://cdenton1.github.io/2026/05/25/Level-5-Bandit-OverTheWire.html">Level 5 - Human Readable Files</a></span>
  <!-- <span>Next Level: <a href="...">Level 7 - ...</a></span> -->
  <span>Main Post: <a href="https://cdenton1.github.io/2025/05/19/Bandit-Writeup-OverTheWire.html">Bandit Writeup - OverTheWire</a></span>
</div>
