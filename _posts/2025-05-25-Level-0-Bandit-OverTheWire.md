---
title: "Level 0 - OverTheWire Bandit"
tags: [overthewire, otw-bandit, hidden-1, hidden-2]
read_time: "1 min read"
---

# Level 0 - OverTheWire Bandit

To know more about OverTheWire, check out my post, [Bandit Writeup - OverTheWire](https://cdenton1.github.io/2025/05/19/Bandit-Writeup-OverTheWire.html). For these walkthroughs, I will be using a Linux OS.

### Level Goal

The goal of this level is for you to log into the game using SSH. The host to which you need to connect is bandit.labs.overthewire.org, on port 2220. The username is bandit0 and the password is bandit0. Once logged in, go to the Level 1 page to find out how to beat Level 1.

### Solution

The task description of this level provides everything necessary for you to complete it, all you need to do, is piece together the required command. 

|Credential|Value                      |
|:---------|:--------------------------|
|Server    |bandit.labs.overthewire.org| 
|Port      |2220                       |
|Username  |bandit0                    |
|Password  |bandit0                    |

Secure Shell Protocol or SSH, is the command you will also be using, both for solving this level but also accessing almost every other level moving forward. If you don't know much about SSH, I would recommend searching it up online or refrencing the man page for it via `man ssh`.

Using the above information we have, the command will look something like this: `ssh <username>@<server> -p <port>` 

You can then fill in the command with the information you know, which should look like this: `ssh bandit0@bandit.labs.overthewire.org -p 2220`

Once you hit enter, it will prompt you for the password, which you can type in (if using a Linux OS, it will be hidden). If you did the right command, and entered the password correctly, you should be greeted with a welcome page similar to the one shown below.

```
$ ssh bandit0@bandit.labs.overthewire.org -p 2220                                                                                                                                          
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

bandit0@bandit.labs.overthewire.org's password: 

      ,----..            ,----,          .---.
     /   /   \         ,/   .`|         /. ./|
    /   .     :      ,`   .'  :     .--'.  ' ;
   .   /   ;.  \   ;    ;     /    /__./ \ : |
  .   ;   /  ` ; .'___,/    ,' .--'.  '   \' .
  ;   |  ; \ ; | |    :     | /___/ \ |    ' '
  |   :  | ; | ' ;    |.';  ; ;   \  \;      :
  .   |  ' ' ' : `----'  |  |  \   ;  `      |
  '   ;  \; /  |     '   :  ;   .   \    .\  ;
   \   \  ',  /      |   |  '    \   \   ' \ |
    ;   :    /       '   :  |     :   '  |--"
     \   \ .'        ;   |.'       \   \ ;
  www. `---` ver     '---' he       '---" ire.org


Welcome to OverTheWire!
...
```

The first level was simply to login, meaning you have successfully completed it and are ready to move onto the next level.

---

<div style="display: flex; justify-content: space-between;">
  <span>Main Post: <a href="https://cdenton1.github.io/2025/05/19/Bandit-Writeup-OverTheWire.html">Bandit Writeup - OverTheWire</a></span>
  <span>Next Level: <a href="https://cdenton1.github.io/2025/05/25/Level-1-Bandit-OverTheWire.html">Level 1 - File Reading</a></span>
</div>
