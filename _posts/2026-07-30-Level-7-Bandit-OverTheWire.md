---
title: "Level 7 - OverTheWire Bandit"
tags: [overthewire, otw-bandit, hidden-1, hidden-2]
read_time: "1 min read"
---

# Level 7 - OverTheWire Bandit

To know more about OverTheWire, check out my post, [Bandit Writeup - OverTheWire](https://cdenton1.github.io/2025/05/19/Bandit-Writeup-OverTheWire.html). For these walkthroughs, I am using a Linux OS.

### Level Goal

The password for the next level is **stored somewhere on the server** and has all of the following properties: owned by user bandit7, owned by group bandit6, and 33 bytes in size.

### Solution

This level is very similar to the last: we need to find the file with the password based on 3 known characteristics, and its location. 

However, for this one we only know that it's on this server, no directory to start.

Knowing the user ownership and group ownership, we can use those attributes as part of a `find` command to search for any file on the server that fits those characteristics. The command looks like: `find / -user bandit7 -group bandit6`.

Skimming through the output and passing over any with `Permission denied` next to it, we eventually come across `/var/lib/dpkg/info/bandit7.password`.

From here, simply using `cat` we can output the contents of the file and retrieve our password.

```
bandit6@bandit:~$ find / -user bandit7 -group bandit6
find: '/drifter/drifter14_src/axTLS': Permission denied
find: '/root': Permission denied

...

bandit6@bandit:~/inhere$ cat /var/lib/dpkg/info/bandit7.password
[password]
```

---

<div style="display: flex; justify-content: space-between;">
  <span>Previous Level: <a href="https://cdenton1.github.io/2026/07/29/Level-6-Bandit-OverTheWire.html">Level 6 - File Characteristics</a></span>
  <!-- <span>Next Level: <a href="...">Level 8 - ...</a></span> -->
  <span>Main Post: <a href="https://cdenton1.github.io/2025/05/19/Bandit-Writeup-OverTheWire.html">Bandit Writeup - OverTheWire</a></span>
</div>
