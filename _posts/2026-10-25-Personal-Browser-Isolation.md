---
title: "Personal Browser Isolation"
tags: [hidden-1, hidden-2, hidden-3]
read_time: "__ min read"
---

# Personal Browser Isolation

A seemingly mundane thing that has always captivated me for some reason since starting college is email security and the technology behind emails in general. 

I don't know what I find so interesting about it but every summer, no matter what team I'm working on, I'm doing something related to it.

This past summer during my internship, my main project of the term was to look into different enterprise browsers and browser isolation solutions for the company. 

Email security and browser security may seem like very different areas but there is actually some overlap between the two, and without one you add more risk to the other.

I spent about 2-3 months looking into these solutions alongside my other work and it got me thinking about how I could build my own; so I did. 

This post is going to walk through what I built and how, and talk about everything I learnt along the way. 

## Disclaimer

In some of my most recent posts I have mentioned moving away from Google products, but at the time of building this project I had no intentions of switching over so unfortunately it is built around it.

Eventually I would like to look into setting up something similar in my other email, but in the meantime I did still want to write about my first "version" if you will.

## Enterprise Browser Vs. Browser Isolation

If you don't work in corporate IT you have probably never heard of either of these so let me quickly break them down.

| | Definition | Examples |
|-|------------|----------|
| Enterprise Browser | A web browser designed for businesses that gives more control to IT and security teams and is often a lot more customizable and secure compared to a regular everyday browser used by the average person. | Chrome Enterprise, Palo Alto Prisma Browser, Island Enterprise Browser, Edge for Business |
| Browser Isolation | Web sessions hosted remotely and accessed through virtual streams by the user, meaning nothing from a website is loaded locally on a computer, minimizing possible risks. | Proofpoint Isolation, Cloudflare Browser Isolation, Palo Alto RBI (Remote Browser Isolation) |

Both offer the same goal of a more secure browser but do them quite differently and sometimes are even packaged together. Browser isolation solutions can also vary quite a lot in ability and setup: 

* Some are an add-on to an email gateway that rewrites links in emails and opens them in a browser isolation if clicked 
* Some require a manual launch by the user to be accessed, and
* Others trigger based on rules setup in an enterprise browser

The list of overall abilities of either one is extremely long, and the amount to choose from is also surprisingly massive. 

I won't be covering any of them in particular in this post but understanding what they are will give a better idea for what I built and why.

## The Project

One point I mentioned above is what immediately gave me this idea: an email gateway rewriting the links in emails to open in an isolated browser if clicked.

There is a company who offers this at larger scale for businesses, but being that I'm not a business and I cannot modify my email gateway in anyway, I wanted to build a personal version for myself that is slightly modified.

The idea being, a script would read unopened emails from my mailbox, parse for any links from its content, and then rewrite them wrapped in the URL that opens the site inside an isolated browser.

Now Gmail also can't modify emails coming in, so I decided it would be best to send a separate email with details about the original and wrapped links.

The more I planned this out though, the more I was reminded by just how many emails I get in a day. 

To minimize sending myself even more emails, I also created some labels for emails I typically expect so I can sort them, and then only apply the above to unexpected senders or whatever I decided.

This isn't the most secure thing though on the off chance that one of those companies is comprised and sending out phishing emails, however, I already don't really click links in emails anyway so I'm not very worried.

### The Plan

I had outlined some step to help plan out the process for building it, and they are listed below:

1. determine rules or classifications for applying filters/labels to my mailbox
2. find a software/application/plugin that I can apply to my personal email to run scripts
3. create an initial script for reading my emails and parsing links from them
4. find a free browser isolation site that fits my needs and is simple to use
5. rewrite the links to open in the browser isolation
6. apply this to my script and base what happens with the email on the above
7. determine how to re-write this info back to myself and add it to the script

Some stuff was still up in the air when originally outlining them, but nonetheless it did give me a good chance to start and then figure out those areas as I went.

For a a plugin that could run scripts on my personal email, I went with [Google Apps Script](https://developers.google.com/apps-script), which is a cloud-based JavaScript platform that can automate tasks across Google products.

I had never heard of it before and because at the time I basically used it for everything, I figured it would be the most useful and probably cheapest (in my case free).

For isolation browsers I was stuck between [Browserling](https://www.browserling.com/) and [MyBrowser](https://www.mybrowser.lol/). 

Browserling ended up not working the best in this case because trying to rewrite the link and wrapping it wasn't very possible. 

MyBrowser bugged out after opening like 3-4 sessions, but it was a lot easier to rewrite and I doubt I would ever be clicking that many links in emails as well either.

### What I Built

### Results

## Conclusion
