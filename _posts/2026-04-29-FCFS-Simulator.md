---
title: "FCFS Simulator - Raspberry Pi Pico W"
tags: [hidden-1, hidden-2, hidden-3]
read_time: "__ min read"
---

# FCFS Using Queues

For my Data Structures class (DATA 3000) we had an assignment where we had to implement a first come first serve algorithm and queues to schedule processes based on user input, and calculate related times.

I really enjoyed this assignment and it actually helped me to better understand FCFS for things like networking, so I wanted to take it another step further and build a physical device to simulate it visually.

For any of the files I share in this post check out this GitHub repo, [Raspberry Pi Files](https://github.com/cDenton1/Raspberry-Pi-Files) and to see a visual demonstration of this project check out this video, [FCFS Simulator | Raspberry Pi Pico W](https://www.youtube.com/watch?v=2HHmcEU_PsQ).

## First Come First Serve

## Setup

I started designing my idea for this project back in March not long after finishing the initial project that inspired it. 

The school project itself was very helpful for demonstrating, however, I wanted a visual aspect and remembered the 10 segment LED bar graph display that I have.

This project did not seem very complex but it was the most wiring I had done for anything, using two components that I hadn't really used before, and required that I properly understood first come first server scheduling.

For this post I have explained the setup for each part seperately before discussing any of the code, to hopefully make it easier to understand.

### Parts

What you need:
- 1x LCD Display
- 1x Rotary Encoder
- 1x 10 Segment LED Bar Graph Display
- 1x 1K ohm resistor
- 10x 220 ohm resistors (this changes based on the LED colour)
- 30x (roughly) wires

**LCD Display** - This component has 16 pins, not all of them are needed but a lot of them are and it can get quite complex. 

Note, I specifically used a HD44780 LCD built for Arduinos, but if you are using a Raspberry Pi Pico I would recommend getting the _____ module for easier use.

Below is what I setup and found worked best for me.

| Component Pin | Plugged |
| ------------- | ------- |
| VSS | GND |
| VDD | 5V (for the Pico, VBUS) |
| VO | GND (1K ohm resistor) |
| RS | GPIO# |
| RW | GND |
| E | GPIO# |
| D0-3 | nothing |
| D4-7 | GPIO#-# |
| A | 5V (VBUS) |
| K | GND |

**Rotary Encoder** - Quite a simple part, it rotates freely compared to a potentiometer and it has a button.

| Component Pin | Plugged |
| ------------- | ------- |
| GND | GND |
| + | 5V (VBUS) or 3.3V |
| SW | GPIO# |
| DT | GPIO# |
| CLK | GPIO# |

**10 Segment LED Bar Graph Display** - For this part, there are 10 sets of pins like if you had 10 separate LEDs. 

One side is connected to a GPIO and the other is grounded with a 220 ohm resistor. For my setup I did GPIO pins #-#.

Below is a photo of everything setup together, it extremely crowded but I my best with what I had available to me.

[ image ]

### Code

## Conclusion
