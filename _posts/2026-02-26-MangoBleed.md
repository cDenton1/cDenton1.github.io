---
title: "MangoBleed Writeup - Hack The Box"
tags: [hidden-1, hidden-2, hidden-3]
read_time: "__ min read"
---

# MangoBleed Writeup - Hack The Box

The other day I was scrolling through LinkedIn and came across someone talking about a phishing related Sherlock on Hack The Box. 

I have only ever done one Sherlock challenge before and it has been forever since then. To avoid laying around and doing nothing once I finished my classes, I thought it would be fun to do the phishing one. 

However, that specific Sherlock was VIP, so I picked one that was available to me - [MangoBleed](https://app.hackthebox.com/sherlocks/MangoBleed?tab=play_sherlock), a DFIR one labeled very easy.

In this post I'm going to walk through my entire process of solving each task and investigating the scenario.

## What is a Sherlock?

Before we jump into doing this, let me quickly run through what a Sherlock is for those who may not know.

Sherlocks are investigative challenges that test your defensive skills. You explore the aftermath of targeted cyber attacks, figuring out what happened through the provided knowledge.

Categories of these challenges include cloud, DFIR, SOC, malware analysis, and threat intelligence. And difficulties range from very easy to insane.

## Sherlock Scenario

> You were contacted early this morning to handle a high‑priority incident involving a suspected compromised server. The host, mongodbsync, is a secondary MongoDB server. According to the administrator, it's maintained once a month, and they recently became aware of a vulnerability referred to as MongoBleed. As a precaution, the administrator has provided you with root-level access to facilitate your investigation.
> 
> You have already collected a triage acquisition from the server using UAC. Perform a rapid triage analysis of the collected artifacts to determine whether the system has been compromised, identify any attacker activity (initial access, persistence, privilege escalation, lateral movement, or data access/exfiltration), and summarize your findings with an initial incident assessment and recommended next steps.

## Tasks

### Task 1

> What is the CVE ID designated to the MongoDB vulnerability explained in the scenario?

In the scenario given to us above, it mentions a vulnerability referred to as MongoBleed. Doing a quick internet search for the vulnerability gives us a ton of results, including from the MongoDB blog.

The post is a security update regarding this specific vulnerability, it lists the CVE as well as links to the CVE record, which provides even more info.

`CVE-2025-14847`

### Task 2

> What is the version of MongoDB installed on the server that the CVE exploited?

Once I started looking through the provided files for this challenge, I wasn't entirely sure where to go. So I started with a pretty broad command to just output anything MongoDB related.

The command I ran was `grep -r -i "MongoDB" [file path]/MangoBleed` (replace [file path] with your actual file path). It gave me a ton of results, including the following lines that I found extremely helpful in narrowing down the answer:
```
[file path]/MangoBleed/live_response/packages/dpkg_-l.txt:ii  mongodb-database-tools             100.14.0                                amd64        mongodb-database-tools package provides tools for working with the MongoDB server: 
[file path]/MangoBleed/live_response/packages/dpkg_-l.txt:ii  mongodb-mongosh                    2.5.10                                  amd64        MongoDB Shell CLI REPL Package
[file path]/MangoBleed/live_response/packages/dpkg_-l.txt:ii  mongodb-org                        8.0.16                                  amd64        MongoDB open source document-oriented database system (metapackage)
[file path]/MangoBleed/live_response/packages/dpkg_-l.txt:ii  mongodb-org-database               8.0.17                                  amd64        MongoDB open source document-oriented database system (metapackage)
[file path]/MangoBleed/live_response/packages/dpkg_-l.txt:ii  mongodb-org-database-tools-extra   8.0.17                                  amd64        Extra MongoDB database tools
[file path]/MangoBleed/live_response/packages/dpkg_-l.txt:ii  mongodb-org-mongos                 8.0.16                                  amd64        MongoDB sharded cluster query router
[file path]/MangoBleed/live_response/packages/dpkg_-l.txt:ii  mongodb-org-server                 8.0.16                                  amd64        MongoDB database server
[file path]/MangoBleed/live_response/packages/dpkg_-l.txt:ii  mongodb-org-shell                  8.0.16                                  amd64        MongoDB shell client
[file path]/MangoBleed/live_response/packages/dpkg_-l.txt:ii  mongodb-org-tools                  8.0.16                                  amd64        MongoDB tools
```

For this question we're looking for the server version, which in output I copied above, it's the third from the bottom.

I think another easier way of searching for this, if you have some experience with MongoDB and what the logs look like, would be searching for `mongodb-org-server` itself instead. This would minimize the results and narrow it down much faster and easier.

`8.0.16`

### Task 3

> Analyze the MongoDB logs to identify the attacker’s remote IP address used to exploit the CVE.

For this I knew I wasn't going to be able to just use grep and find the answer, I was going to need to figure out where this specific log file was to look through. 

So I ran the command `ls -laR` to output everything in this directory and subdirectories. There was a lot of output here, which did help me figure out where to go but I think there might be another way to do it.

Using the filepath that I found above, I was originally thinking of just running `cat '[root]/var/log/mongodb/mongod.log'` but I realized very quickly just how large this file was and decided it be best to pivot a different direction.

Instead I ran `head` and `tail` on the file to get a feel how the logs were laid out. 

From there I tweaked the command from earlier to filter specifically for network related events, `cat '[root]/var/log/mongodb/mongod.log' | grep "NETWORK"`. Right at the top there were some pretty suspicious lines:
```
{"t":{"$date":"2025-12-29T05:25:52.743+00:00"},"s":"I",  "c":"NETWORK",  "id":22943,   "ctx":"listener","msg":"Connection accepted","attr":{"remote":"65.0.76.43:35340","isLoadBalanced":false,"uuid":{"uuid":{"$uuid":"099e057e-11c1-46ed-b129-a158578d2014"}},"connectionId":1,"connectionCount":1}}
{"t":{"$date":"2025-12-29T05:25:52.744+00:00"},"s":"I",  "c":"NETWORK",  "id":22944,   "ctx":"conn1","msg":"Connection ended","attr":{"remote":"65.0.76.43:35340","isLoadBalanced":false,"uuid":{"uuid":{"$uuid":"099e057e-11c1-46ed-b129-a158578d2014"}},"connectionId":1,"connectionCount":0}}
```

Lines like these went on for ages, and the hint for this task mentioned keeping an eye out for a pattern like that. In each line there are some important bits, including the remote of presumably the attacker.

`65.0.76.43`

### Task 4

> Based on the MongoDB logs, determine the exact date and time the attacker’s exploitation activity began (the earliest confirmed malicious event)

From the lines I copied above in the previous task, that was the initial activity from the attacker of when the exploitation activity began. 

The command `cat '[root]/var/log/mongodb/mongod.log' | grep "65.0.76.43"` does confirm it though as well.

`2025-12-29 05:25:52`

### Task 5

> Using the MongoDB logs, calculate the total number of malicious connections initiated by the attacker.

The command I used for confirmation in the last task, `cat '[root]/var/log/mongodb/mongod.log' | grep "65.0.76.43"`, I used again here. The output is the same as what I showed in task 3, only those lines but they go on for so long. 

Included in each line is a connection ID, and at the end of this output we see the following:
```
{"t":{"$date":"2025-12-29T05:27:07.159+00:00"},"s":"I",  "c":"NETWORK",  "id":22943,   "ctx":"listener","msg":"Connection accepted","attr":{"remote":"65.0.76.43:37290","isLoadBalanced":false,"uuid":{"uuid":{"$uuid":"dd6d41f0-9b61-4d23-ba8e-45eee13d9913"}},"connectionId":37630,"connectionCount":1}}
{"t":{"$date":"2025-12-29T05:27:07.160+00:00"},"s":"I",  "c":"NETWORK",  "id":22944,   "ctx":"conn37630","msg":"Connection ended","attr":{"remote":"65.0.76.43:37290","isLoadBalanced":false,"uuid":{"uuid":{"$uuid":"dd6d41f0-9b61-4d23-ba8e-45eee13d9913"}},"connectionId":37630,"connectionCount":0}}
```

For this question I initially assumed that it was only looking for the amount of accepted connections, and since the connection ID only seems to increase after an accepted and ended connection, I thought the answer was 37630.

However, I realized it was looking for the total number to include both. Two ways of solving from here was tweaking the command I used above or just multiplying the number found earlier by two.

I took the route of running the other command, because I did want some more practice. I know there is a count option, I just don't use it a lot, and wanted to take the opportunity. The command now just looked like this: `cat '[root]/var/log/mongodb/mongod.log' | grep -c "65.0.76.43"`

To double check the number I got was correct, I did also divide it by two and compared it with what I found earlier. And even though I had to run extra commands, it actually gave me the opportunity to confirm my final answer even more instead of just guessing it on a whim.

`75260`

### Task 6

> The attacker gained remote access after a series of brute‑force attempts. The attack likely exposed sensitive information, which enabled them to gain remote access. Based on the logs, when did the attacker successfully gain interactive hands-on remote access?

...

### Task 7

> Identify the exact command line the attacker used to execute an in‑memory script as part of their privilege‑escalation attempt.

...

### Task 8

> The attacker was interested in a specific directory and also opened a Python web server, likely for exfiltration purposes. Which directory was the target?

...

## Conclusion

I had a ton of fun working through this, so much in fact I did another one the next morning in between working on writing this post and some homework.

These types of challenges are always what I'm looking for in CTFs so now knowing that Hack The Box has over a hundred, I'm going to be doing them all the time.

Working with logs like these aren't something I have a ton of experience with either and this is the kind of thing I eventually want to do once I'm finished school, so it's really good practice.

The scenarios being based on actual and often recent CVEs adds an extra layer of learning to it. Reading up on the vulnerabilites and exploits a little before going through and hunting them down is really cool and gives users the opportunity to see what the actual results might look like.

I am huge fan of these right off the bat and I'm looking forward to more.
