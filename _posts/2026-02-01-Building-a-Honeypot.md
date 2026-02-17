---
title: "Building a Honeypot"
tags: [hidden-1, hidden-2, hidden-3]
read_time: "_ min read"
---

# Honeypot

I built a **Honeypot** on my Raspberry Pi Pico W during my winter break and I've been dying to talk about it. 

In this post I'm going to share the building process, all the planning, everything I learnt, and my finished product.

## What is a honeypot?

...

## My Idea and Plan

Using my Raspberry Pi Pico, I wanted to build a honeypot from scratch in micro python. I planned to set it up to be a wifi access point, log connection attempts, host a fake captive portal, and use a Discord webhook for notifications.

I outlined a project skeleton and steps for my initial plan, then adjusted both as necessary while building the honeypot.

### Project Skeleton
| File        | Reason |
|-------------|-------------|
| Main Script | entry point |
| WiFi AP Script | access point setup |
| Captive Portal Script | http server and pages |
| Portal HTML | HTML code used by the captive portal |
| HTML Forms | initial form and success form |
| ~~Notifier Script~~ | ~~Discord webhook~~ |
| Logger Script | event logging |
| Config Script | SSID, webhook, and any other settings |
| Background Script | combination of the dns hijacking and connections |
| Connection Monitoring | monitor devices connecting to the AP and log to the terminal |
| DNS Hijacking | redirect users to the web server form |
| README File | project walkthrough and explanation |

### Project Steps
1. Get AP mode working and connecting some devices to it
2. Start a server and test visiting it
3. ~~Setup Discord webhook and confirm the alerts work~~
4. Improve the captive portal and make final adjustments

Note - something I had outlined initially, I later found out while building that it wouldn't possible, so I still included it above but crossed it out. 

Throughout building the above, I also tracked everything I was doing and learning.

## What I Built

...

## Challenges I Faced

...

## Conclusion

...
