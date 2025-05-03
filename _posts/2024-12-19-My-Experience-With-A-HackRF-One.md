---
title: "My Experience with a HackRF One"
image: assets/images/hackrfone.png
tags: [projects]
read_time: "9 min read"
---

During one of my last weeks in my wireless security class this semester, one of the tasks for a lab I was working on was to use a HackRF One to tune into different radio stations and signals. During my lab period I spent a few hours trying out different SDR software, tuning into different channels and just messing around. Even after the class, I continued to try different things with it. This tool is actually really cool and I wish I had more than a couple days to use it, but the semester ended recently and I have to return it to the school prior. In my short time of testing things out and just learning more, I tested out listening to a wider range of signals, looked into other signals besides typical radio stations, and looked into some projects I could try that would incorporate the HackRF.

---

## Introduction

HackRF One is a software defined radio (SDR) peripheral capable of transmission or reception of radio signals from 1 MHz to 6 GHz, it is an open source hardware platform which can either be used as a USB peripheral, which was what I was doing, or programmed for stand-alone operation. I’m summarizing Great Scott Gadgets’ website so if you are curious in reading more about the device itself, check out their site here [https://greatscottgadgets.com/hackrf/one/](https://greatscottgadgets.com/hackrf/one/).

For my lab, the few signals we were tasked with tuning to were, 92.1 FM stereo, Weather Radio Canada: Alberta network 162.4, Channel 19 CB Radio 27.185, and SAIT Cleaners 463.6125. The first two we were able to get pretty clear once we had semi-figured out what we were doing with the tuning and the bandwidth. But the other two signals we tried to tune in on were quite a bit harder as the SAIT Cleaners one had no traffic and Channel 19 CB Radio is considered obsolete and very rare to come across now. It was considered one of the most frequently used channels in the Citizen Band radio service; primarily used by truckers and commercial drivers.

I could definitely learn or use some more practice with this kind of stuff, but it also helped quite a bit once I switched from SDR++ on a DragonOS VM to SDR# on my Windows machine. SDR++ wasn’t terrible but our instructor made it sound like it was going to be way easier to use compared to trying to setup SDR#, in my opinion it was the opposite and now I’ll know for the future.

---

## Experimenting and Testing

Once I was home, I tried some of the channels from the assignment again to see if I could get a clearer signal than what I was getting on campus or not. I honestly didn’t get as much of a difference at home just because I don’t think I really had time to learn how to tune it properly, so I was just sliding everything around until it became somewhat bearable to listen to but it did help me learn some things. Later on I tested out listening to some new stations, not only FM but some AM as well. I didn’t have any luck with the AM stations I was trying, but for the most part all the FM ones I tried I got pretty clear signals for.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733762069924/34c7eb79-f335-4da9-823c-947fd202ab86.png align="center")

For the listed stations in the screenshot above, and just having a reference for what radio stations to try, I used the link below. It seems to be just a little outdated, but otherwise great and a pretty useful list to get started with, [https://www.radiostationworld.com/locations/canada/alberta/calgary/radio\_stations/](https://www.radiostationworld.com/locations/canada/alberta/calgary/radio_stations/). I didn’t try every station from the list but I did use it as a guide of what to try out and what to look for.

### SDR Software Output

When trying 93.7, I noticed if I lined up the bandwidth enough to get a really clear signal, the SDR software was able to output the title of the song and artist of whatever was currently playing. So Friday, Dec 6th, sometime between 7pm and 9pm, the station was playing **When I’m Gone By Ryland Moranz**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1733762686104/60dc833a-15c6-4bac-b991-a084efadaa0c.png align="center")

Most of the other stations did give some sort of output for that as well but some of them were very little. I’m assuming that it was a mix of SDR# having a hard time decoding the metadata and I wasn’t getting a clear enough of a signal for the software to receive the metadata. Another station I did get a really clear signal on was 107.3 which was playing **Use Somebody By Kings Of Leon**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734376433489/3f397894-093a-41e5-a460-dcffaeb584d0.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734376833236/9ab68850-5dfe-47b5-83be-35db1e91f64a.png align="center")

### Change in Radio Station Broadcasters

Going back to why I believe the website I was using for reference (Radio Station World) was a little outdated is because, going to Global News’ website shows they still host QR Calgary on 770 AM but I couldn’t find anything on their site for 107.3 FM (the station I originally assumed I was tuning in on, shown above). On Radio Station World’s website they list that station as QR Calgary and a news/talk station, which is what it used to be. But back in July of this year it became, Iconic Alternative 107.3 The Edge, a classic alternative radio station apart of the Corus Entertainment Network which is a massive media and content company with audiences all over the world. This was a bit of a rabbit hole I fell into but it doesn’t have much relation with the HackRF besides that, so I’ll keep it to that one fact.

### Interesting Signal Discovery

Something else interesting I noticed with the stations CJWE/CFWE and Shine FM, was that even though CJWE’s Calgary station is 88.1 and Shine FM’s Calgary station is 88.9, I could listen to CJWE on 98.1 and Shine FM on 98.9. And it wasn’t like these signals were weaker, I could listen to them just as clear as when listening on the actual station frequencies and they both had the same output from the metadata.

When visiting the CJWE/CFWE website and looking at their frequency map, there is no station for the channel of 98.1, [https://cfweradio.ca/frequency-map/](https://cfweradio.ca/frequency-map/). The only frequencies listed under CJWE is Calgary 88.1, Lethbridge 103.5, and Medicine Hat 106.3. Even when looking under CFWE, which is for the rest of Alberta, there are no stations using that frequency and the closest to it is Edmonton 98.5.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734379014780/4d9878fc-bded-4fe3-913d-721a1f272214.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734379037959/e82972d7-241d-4839-83af-f2e9b5520a95.png align="center")

Visiting the Shine FM website, [https://www.shinefm.com/](https://www.shinefm.com/), it immediately lists all of their stations. Which is only three, 88.9 Calgary, 90.5 Red Deer, and 105.9 Edmonton, but nothing at 98.9.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734379150797/acbf1d48-83ac-49b7-8a1e-f93698584373.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734379201508/bf65b886-0f8a-41e5-8cab-31786134784a.png align="center")

Doing some research there were a few possible reasons for this:

1. Harmonics - these are multiples of a fundamental frequency generated during transmission or reception, the signal picked up could have been a spurious harmonic generated as a byproduct of imperfections in the HackRF’s receiver circuitry.
    
2. Intermodulation - this occurs when strong signals interact in a nonlinear system, like a receiver or transmitter producing phantom signals at other frequencies. If the HackRF’s front-end was overloaded by a strong signal it might have been mixed internally to produce the artifacts at the other frequencies.
    
3. Image Frequency - a mirrored signal caused by inadequate filtering, allowing signals from an unintended frequency to leak into the intended one.
    

There a few others but reading more into these, they seemed the most likely. At the time of trying these channels I wish I had tested out what could have been the reasons for this but I didn’t. If I did, here is what I would have done to test it out:

* Check with another receiver like a traditional FM radio or another SDR, I would have attempted tuning into the unintended frequencies on those.
    
* Using shielding to reduce the signal strength entering the HackRF to see if it was being overloaded.
    

### Radio Reference Database

Another site I checked out was, [https:/](https://cfweradio.ca/frequency-map/)[/www.radioreference.com/db/browse/ctid/5006](https://www.radioreference.com/db/browse/ctid/5006). This is the Radio Reference Database (RRDB) for Calgary County, it listed frequencies for public safety (Police, Fire, Alberta Health Services), search and rescue, correction facilities, municipal services, board of education, and higher education. It also links to other categories in Calgary like Calgary attractions, Calgary international airport, retail, businesses and a few others. A lot of the channels are mainly used for security but there are a bunch that are tagged for, emergency, school, business, aircraft, and public works.

I was really hoping to get a chance to test out tuning in on some of the frequencies used by the airport but I unfortunately didn’t get the chance. Like I mentioned above, the airport has it’s own category/section in the RRDB for Calgary County, [https://www.radioreference.com/db/aid/4966](https://www.radioreference.com/db/aid/4966). It is a long list with a lot of different frequencies and I think getting the chance to tune in would have been really cool.

---

## Project Ideas

I had three ideas for programs to build and things to do with or use alongside the HackRF One. For the most part these are just ideas but I would like to eventually work on fleshing them out more if I get the opportunity to work with a tool like this again. Some classmates and I initially looked into buying one but they can be expensive, looking into other tools similar might give me this opportunity.

### 1\. Building a simple SDR-Based Scanner

* Detects frequencies and signals
    
* Scan and log signals across a frequency range
    

Something like this could easily be done by writing a simple script in Python that scans a range of frequencies in small steps and records signal strength and frequency to a log. Optionally then I could also display the recorded info to add a bit more work to the program and make the info easier to read.

### 2\. ADS-B Aircraft Tracking

* Capture and decode ADS-B signals from airplanes to track flights in real time
    
* Track details like the location, altitude, and speed
    

For something like this I would need some software like **dump1090** or **gr-adsb** for recording the DS-B signals and I would need an antenna that can receive 1090 MHz signals. Then I could use more software for visualizing the flight paths, and optionally another python script for analyzing ADS-B messages.

### 3\. Building an FM Transmitter

* Create a basic FM radio station (unused and legal range)
    
* Use the HackRF One to broadcast an FM signal
    

Using something like hackrf-transfer I could broadcast a downloaded audio file on a specific frequency, I could then also experiment with live audio streams or generate FM signals dynamically with GNU Radio.

For this last one I did look more into it because the word legal made me curious what the radio transmitter laws are here in Canada and there are a lot. In the future if I had the opportunity I would look more into it again but from my understanding for something small like this I don’t believe I would need a license/certificate. Where I looked for this information was here [https://www.canada.ca/en/services/business/permits/federallyregulatedindustrysectors/broadcastingtelecommunicationsregulation.html](https://www.canada.ca/en/services/business/permits/federallyregulatedindustrysectors/broadcastingtelecommunicationsregulation.html) and here [https://ised-isde.canada.ca/site/spectrum-management-telecommunications/en/licences-and-certificates/exemptions](https://ised-isde.canada.ca/site/spectrum-management-telecommunications/en/licences-and-certificates/exemptions).

---

## Conclusion

This was a really fun tool to work with, I enjoyed testing out different frequencies and signals with it, and it was honestly one of my favourite things I learnt this semester at school. I think just getting my hand on something and getting to experiment with something that I found both so simple and complex at the same time was really cool. It wasn’t super complex compared to other devices we had worked with but it also wasn’t something that we would barely use for a lab that would only take a few minutes to complete. One day it would be nice to use something like the HackRF One again, but till then I’m going to continue to learn more, focus on working on other related hobbies, and finish up some already started projects.
