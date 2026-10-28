---
title: "Email Browser Isolation"
tags: [hidden-1, hidden-2, hidden-3]
read_time: "__ min read"
---

# Email Browser Isolation

A seemingly mundane thing that has always captivated me for some reason since starting college is email security and the technology behind emails in general. 

I don't know what I find so interesting about it but every summer, no matter what team I'm working on, I'm doing something related to it.

This past summer during my internship, my main project of the term was to look into different enterprise browsers and browser isolation solutions for the company. 

Email security and browser security may seem like very different areas but there is actually some overlap between the two, and without one you add more risk to the other.

I spent about 2-3 months looking into these solutions alongside my other work and it got me thinking about how I could build my own; so I did. 

This post is going to walk through what I built and how, and talk about everything I learnt along the way. 

## Disclaimer

In some of my most recent posts I have mentioned moving away from Google products, but at the time of building this project I had no intentions of switching over so unfortunately it is built around it.

Eventually I would like to look into setting up something similar in my other email, but in the meantime I did still want to write about my first "version" of this if you will.

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

There is a company who offers this at larger scale for businesses, but as I'm not a business and I cannot modify my email gateway in anyway, I wanted to build a personal version for myself that is slightly modified.

The idea being, a script would read unopened emails from my mailbox, parse for any links from its content, and then rewrite them wrapped in the URL that opens the site inside an isolated browser.

Gmail also can't modify emails coming in, so I decided it would be best to send a separate email with details about the original with it's now wrapped links.

To avoid sending myself a bunch of extra emails, I also created some labels for emails I typically expect so I can sort them, and then only apply the above to unexpected senders or whatever I decided.

This isn't the most secure if one of those companies is comprised and sending out phishing emails, however, I already practice not clicking links in emails to begin with.

### The Plan

I had outlined some steps to help plan out the process for building it, and they are listed below:

1. determine rules or classifications for applying filters/labels to my mailbox
2. find a software/application/plugin that I can apply to my personal email to run scripts
3. create an initial script for reading my emails and parsing links from them
4. find a free browser isolation site that fits my needs and is simple to use
5. rewrite the links to open in the browser isolation
6. apply this to my script and base what happens with the email on the above
7. determine how to re-write this info back to myself and add it to the script

Some stuff was still up in the air when originally outlining them, but nonetheless it did give me a good chance to start and then figure out those areas as I went.

### Chosen Software/Applications

For a a plugin that could run scripts on my personal email, I went with [Google Apps Script](https://developers.google.com/apps-script), which is a cloud-based JavaScript platform that can automate tasks across Google products.

I had never heard of it before and because at the time I basically used Google products for everything, I figured it would be the most useful and probably cheapest (in my case free).

For isolation browsers I was stuck between [Browserling](https://www.browserling.com/) and [MyBrowser](https://www.mybrowser.lol/). 

Browserling ended up not working the best in this case because trying to rewrite the link and wrapping it wasn't very possible. 

MyBrowser bugged out after opening like 3-4 sessions, but it was a lot easier to rewrite and I also assumed I would very rarely ever hit that limit so in this case it would be my chose.

### What I Built

For the privacy of myself I have redacted and shortened a few sections of the final script that I am sharing here and will maybe post to my GitHub as well.

This first section creates all the necessary labels such as what I want to be assigned for each label, the labels, and few extra things for the "scanned" emails.

```js
/* VARIABLES FOR SCANNED EMAILS */
var mtAddr;
var urls = [];
var wrapped = [];

const scannedEmailLabel = GmailApp.getUserLabelByName("SCANNED")
const scannedEmail = ["SCANNED"]

/* SENDER RELATED VARIABLES AND LABELS */
const placeholderSender = ["Placeholder Sender Name"]
const placeholderSenderLabel = GmailApp.getUserLabelByName("Placeholder Sender")

/* ATTACHMENT RELATED VARIABLES AND LABELS */
const placeholderFile = ["Placeholder File Name"]
const placeholderFileLabel = GmailApp.getUserLabelByName("Placeholder File")

/* SUBJECT RELATED VARIABLES AND LABELS */
const placeholderSubject = ["Placeholder Subject Name"]
const placeholderSubjectLabel = GmailApp.getUserLabelByName("Placeholder Subject")
```

Be aware that most of these are just placeholders and mine includes a lot more that are specific to how I want to sort my mailbox.

This second section is the function that actually handles labeling the emails. First searching my mailbox for any unread emails with no user labels.

Then it goes through one by one checking the sender, subject, and/or attachment of the email and assigning labels based off what I set above. For anything that doesn't get picked up by these labels got to the function that handles "scanning".

```js
function labelEmails() {
  const emails = GmailApp.search('is:unread has:nouserlabels');
  for (let i = 0; i < emails.length; i++){
    const messages = emails[i].getMessages();

    // breaking down the message into parts
    const messageFrom = messages[0].getFrom();
    const attachments = messages[0].getAttachments();
    const subject = messages[0].getSubject();

    // begin applying labels based off senders
    if (placeholderSender.some(name => messageFrom.includes(name))) {
      emails[i].addLabel(placeholderSenderLabel);
    } else if (placeholderSender.some(name => messageFrom.includes(name))) {
      // check for attachments
      if (attachments.length > 0) {
        // loop through all attachments
        for (let j = 0; j < attachments.length; j++) {
          const filename = attachments[j].getName();
          if (placeholderFile.some(name => filename.includes(name))) {
            emails[i].addLabel(placeholderFileLabel);
          }
        }
      } else {
        emails[i].addLabel(placeholderSenderLabel);
      }
    } else if (placeholderSender.some(name => messageFrom.includes(name))) {
      // check subject of email
      if (placeholderSubject.some(name => subject.includes(name))) {
        emails[i].addLabel(placeholderSubjectLabel);
      } else {
        emails[i].addLabel(placeholderSenderLabel);
      }
    // handle "scanned" and "scan" related emails
    } else if (messageFrom == "[ myself ]" && scannedEmail.some(name => subject.includes(subject))) {
      emails[i].addLabel(scannedEmailLabel);
    }else {
      scanEmail(messages[0]);
      sendEmail(messages[0]);
      emails[i].addLabel(scannedEmailLabel);
    }

    console.log(messages[0].getFrom()); 
  }
}
```

Again, similar to the first chunk I explained, I cut out a lot here and swapped stuff for placeholders so if you're building your own you will most likely need more conditions.

This next chunk does the so called scanning, first by replacing the slashes, semicolon, and spaces in each URL with the URL encoded values, and then creating the entire request with the MyBrowser URL before adding it to an array to store them.

```js
function scanEmail(message) {
  const body = message.getBody();
  const links = [...body.matchAll(/<a\b[^>]*\bhref\s*=\s*["']([^"']+)["']/gi)].map(match => match[1]);
  for (let i = 0; i < links.length; i++) {
    if (links[i].includes("mailto:")) {
      mtAddr = links[i].split(':')[1];
      links.splice(i);
    } else {
      var result = links[i].replace(/\//g, '%2F')
      result = result.replace(':', '%3A')
      result = result.replace(/ /g, '%20')
      urls.push(result);
    }
  }

  // loop through links to add them to the browser isolation url
  for (let j = 0; j < urls.length; j++) {
    var request = `https://www.mybrowser.lol/session?browser=firefox&url=${urls[j]}`;
    wrapped.push(request);
  }
  console.log(wrapped);
  return;
}
```

Lastly I built out the email being sent back to myself. This includes the subject and sender it's coming from and the list of wrapped URLs.

```js
function sendEmail(message) {
  const fromSubject = message.getSubject();
  const fromSender = message.getFrom();

  var linkString;
  for (let i = 0; i < wrapped.length; i++) {
    if (linkString == undefined) {
      linkString = `${i+1}: ${wrapped[i]}\n`;
    } else {
      linkString += `${i+1}: ${wrapped[i]}\n`;
    }
  }

  var emailaddress = '[ myself ]';
  var subject = `SCANNED "${fromSubject}"`;
  var message = `An email with the subject: ${fromSubject} \nSent from the address: ${fromSender} \n\nIncluded the following links: \n${linkString}`;

  GmailApp.sendEmail(emailaddress, subject, message);
}
```

### Results

I tested this by sending an email to myself with some URLs, receiving the following email:

```
An email with the subject: URL Test
Sent from the address: [ myself ] <myself@placeholder.com>

Included the following links:
1: https://www.mybrowser.lol/session?browser=firefox&url=https%3A%2F%2Fgithub.com%2FcDenton1
2: https://www.mybrowser.lol/session?browser=firefox&url=https%3A%2F%2Fyoutu.be%2Frwe9fKfnNS0?si=prqrPxmgvXjcdZTi
3: https://www.mybrowser.lol/session?browser=firefox&url=https%3A%2F%2Fbushrabook.com%2FBooks%2FCryptography%2520%26%2520Network%2520Security%25208th%2520Ed.pdf.pdf
```

I just tested some simple links for GitHub, YouTube, and my cryptography textbook, but it was good demonstration for some variety.

I don't think I'd very often get these but if I do, I think it could be quite useful to 1) know that this isn't a typical sender I receive from and 2) if I wanted to click the links, at least I know it won't run anything on my own machine.

## Conclusion

Even if this isn't a project I might ever really use, I have already noticed just how simple but useful I find the labeling itself to be. 

Everyone should practice not clicking email links but adding this extra layer makes me feel a little better if I was to click by accident or for whatever reason.

And also a great chance to continue practicing my JavaScript abilities and learn about the Gmail API.
