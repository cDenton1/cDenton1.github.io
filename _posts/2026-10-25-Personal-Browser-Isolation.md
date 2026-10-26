---
title: "Personal Browser Isolation"
tags: [hidden-1, hidden-2, hidden-3]
read_time: "__ min read"
---

# Personal Browser Isolation

A seemingly mundane thing that has always captivated me for some reason since starting college is email security and the technology behind emails in general. 

I don't know what I find so interesting about it but every summer, no matter what team I'm working on, I'm doing something related to it.

This past summer during my internship, my main project of the term was to look into different enterprise browser and browser isolation solutions for the company. 

Email security and browser security may seem like very different areas but there is actually some of overlap between the two, and without one you add more risk to the other.

I spent about 2-3 months looking into these solutions alongside my other work and it got me thinking about how I could build my own; so I did. 

This post is going to walk through what I built and how, and talk about everything I learnt along the way. 

## Enterprise Browser Vs. Browser Isolation

If you don't work in corporate IT you have probably never heard of either of these so let me quickly break them down.

| | Definition | Examples |
|-|------------|----------|
| Enterprise Browser | A web browser designed for businesses that gives more control to IT and security teams and is often a lot more customizable and secure compared to a regular everyday browser used by the average person. | Chrome Enterprise, Palo Alto Prisma Browser, Island Enterprise Browser, Edge for Business |
| Browser Isolation | Web sessions hosted remotely and accessed through virtual streams by the user, meaning nothing from a website is loaded locally on a computer, minimizing possible risks. | Proofpoint Isolation, Cloudflare Browser Isolation, Palo Alto RBI (Remote Browser Isolation) |

Both offer the same goal of a more secure browser but do them quite differently and sometimes are even packaged together. Browser isolation solutions can also vary quite a lot in ability and setup: 

* Some are an add-on to an email gateway or service that rewrites links in emails and opens them in a browser isolation if clicked 
* Some require a manual launch by the user to be accessed, and
* Others trigger based on rules setup in an enterprise browser

The list of overall abilities of either one is extremely long, and the amount to choose from is also surprisingly massive. 

I won't be covering any of them in particular in this post but understanding what they are will give a better idea for what I built and why.

## The Project

One point I mentioned above is what immediately gave me this idea: an email gateway rewriting the links in emails to open in an isolated browser if clicked.

There is a company who offers this at larger scale for businesses, but being that I'm not a business and I cannot modify my email gateway in anyway, I wanted to build a personal version for myself that is slightly modified.

The idea being, a script would read unopened emails from my mailbox, parse for any links from its content, and then rewrite them wrapped in the URL that opens the site inside an isolated browser.

Now Gmail also can't modify emails coming in, so I thought why not take it a step further and create a security score for the email as well, and then send myself an email with some info, the score and the wrapped emails; kind of like a report.

The more I planned this out though the more I remembered just how many emails I get in a day. To minimize sending myself even more emails, I also created some labels for emails I typically expect so I can sort them, and then only applied the above to unexpected senders or whatever I decided.

This isn't the most secure though on the off chance that one of those companies is comprised and sending out phishing emails, however, I already don't really click links in emails anyway so I'm not very worried.

### The Plan

### What I Built

### Results

## Conclusion
