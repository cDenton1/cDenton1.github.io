---
title: "My Experience With a Raspberry Pi Pico"
image: assets/images/raspberrypipico.png
tags: [raspberry-pi, projects, python]
read_time: "11 min read"
---

# My Experience With a Raspberry Pi Pico

Four weeks into the semester, my Internet of Things Systems class has quickly become a favourite. Right off the bat we got into working with a Raspberry Pi Pico which has been really exciting. Outside of our first two labs I have been experimenting with other components to expand and solidify my understanding of the wiring aspect and get a feel for everything. In this article I want to talk about what I learned and show some of the things I made on my own, including a couple of projects.

---

## First Time Hands-On

I’ve heard of the Raspberry Pi before but prior to this semester I hadn’t really ever done anything with them. In junior high I remember vaguely getting the opportunity to work with Arduinos but that was about it when it came to stuff like this. Getting to work with a Raspberry Pi for one of my classes is really exciting. Our first lab focused on LED’s and buttons, which was a great first step. Our second lab stepped it up quite a bit by combining some aspects from the first lab along with ADCs (Analog to Digital Convertor), potentiometers, and seven segment displays. The more I work on these labs and follow along with the exercises in class, the more I look at my Raspberry Pi and think about other really cool things I could do.

---

## Coin Flip Project

Many people don’t know this about me, but throughout high school, when it came time to writing my final project for my computer science class, I would make a coin flip game. Each year I did this, expanding more on the previous one. First year I kept it simple with just the terminal, next year I added some physical buttons and LEDs, and then lastly a GUI. I’ve continued this tradition for anytime I learn a new programming language (i.e. Python and Assembly). Now being the Raspberry Pi runs on MicroPython, it’s not much of a new language but it introduces a physical component. As a way to carry on tradition, I have written a coin flip game that utilizes those elements for my Raspberry Pi.

### OLED Display

For this project I used an OLED screen, the specific one I used was labeled IIC OLED. To use an OLED display you will need a library file. When initially setting up my OLED display I followed this website for help, [https://randomnerdtutorials.com/raspberry-pi-pico-ssd1306-oled-micropython/](https://randomnerdtutorials.com/raspberry-pi-pico-ssd1306-oled-micropython/), it was where I found the code I used for the OLED library and it included some help for setting up the wiring and some test code to ensure proper setup.

Very easily, copy the provided code from the website and save it to a Python file on you Pi named `ssd1306`. You might be able to name it something else if you prefer, but I stuck with that to be safe. Then when coding the display make sure to import the file simply including the line `import ssd1306` or `from ssd1306 import SSD1306_I2C`. Once you have setup the display and tested to make sure it works correctly, you can move on to try out different things.

### Code Walkthrough

If you want to follow along, here is a list of what I used and what you will need:

* x1 - Raspberry Pi Pico (mine is a Pico W, doesn’t matter though)
    
* x1 - Breadboard (recommended at least 45 rows)
    
* x2 - Buttons
    
* x2 - 1K ohm resistors
    
* x1 - OLED display
    
* x7 - Cables
    

Here is how I setup my breadboard:

* Ground the negative rail on the breadboard by attaching a cable into it and lining it up with one of the GND pins of the Pico (**overlapping white cable below**)
    
* OLED display - requires four cables at the top (left to right)
    
    * GND (**white cable below**) - connected to the negative rail of the breadboard, or connected directly to one of the GND pins of the Pico
        
    * VCC (**red cable below**) - connected into pin 36, the 3V3(OUT) power pin, this supplies the required voltage to the display
        
    * SCL (**yellow cable below**) - connected into pin 7, GP5, this is the clock line
        
    * SDA (**orange cable below**) - connected into pin 6, GP4, this is the data line
        

The SCL and SDA pins can use other GPIO pins, just make sure to specify them when initializing the I2C in your code, in the line:

```python
i2c = I2C(0, scl=Pin(5), sda=Pin(4))
```

* Buttons - left is tails, right is heads
    
    * Each button requires a cable attached to one of the GPIO pins (**green cables below**)
        
        * The left one is connected to pin 20, GP15
            
        * The right one is connected to pin 19, GP14
            
    * Each button also needs to be grounded, I used one resistor on each, and attached them into the negative rail of the breadboard
        

Like the SCL and SDA pins, the buttons can use other GPIO pins, just make sure again to adjust your code to properly initialize the pins in the lines:

```python
button_heads = Pin(14, Pin.IN, Pin.PULL_UP)  # button for heads
button_tails = Pin(15, Pin.IN, Pin.PULL_UP)  # button for tails
```

Here is what mine looked like in reference to what I explained above:

![](/assets/images/raspberryPic1.jpeg "Coin Flip Setup")

Below is the code I wrote for the coin flip game. Once started it will prompt you with heads or tails and tell you which button on the breadboard is for which; left is tails and right is heads. When a user presses a button, the OLED screen will display whatever they had chosen before calling the coin flip animation function, it will decide the result of the coin flip randomly before cycling through the draw coin and draw ellipse functions three times. Then it will land on the winning side and print out a message saying the user either won or lost. If you’re interested, I have included a link to a video below the following code, demonstrating everything combined and running together.

```python
import time
import random
from machine import Pin, I2C
from ssd1306 import SSD1306_I2C     # SSD1306 OLED library

WIDTH = 128    # OLED display width
HEIGHT = 64    # OLED display height

i2c = I2C(0, scl=Pin(5), sda=Pin(4))     # initialize I2C
oled = SSD1306_I2C(WIDTH, HEIGHT, i2c)   # initialize OLED

button_heads = Pin(14, Pin.IN, Pin.PULL_UP)  # button for heads
button_tails = Pin(15, Pin.IN, Pin.PULL_UP)  # button for tails

# draw an ellipse to simulate a rotating circle
def draw_ellipse(cx, cy, rx, ry, fill=1):
    if ry == 0:     # Prevent division by zero
        return
    for x in range(-rx, rx + 1):
        for y in range(-ry, ry + 1):
            if (x**2) / (rx**2) + (y**2) / (ry**2) <= 1:
                oled.pixel(cx + x, cy + y, fill)

# draw the coin with text
def draw_coin(text, rx, ry):
    oled.fill(0)     # clear display
    draw_ellipse(64, 32, rx, ry, fill=1)             # draw an ellipse to represent the coin
    draw_ellipse(64, 32, rx - 2, ry - 2, fill=0)     # outline by clearing the inner ellipse
    if ry > 5:             # display text only if the coin is "wide enough" during the flip
        x_pos = 64 - 3     # center H or T
        y_pos = 28
        oled.text(text, x_pos, y_pos, 1)
    oled.show()

# coin flip animation
def coin_flip_animation():
    result = random.choice(["H", "T"])     # final result

    # simulate flipping by changing the ellipse height (ry)
    for _ in range(3):                     # repeat flipping cycles
        for ry in range(20, 2, -2):        # flatten the ellipse
            draw_coin("", 20, ry)
            time.sleep(0.005) 
        for ry in range(2, 21, 2):         # expand the ellipse
            draw_coin("", 20, ry)
            time.sleep(0.005) 

    # final state: Show the result
    oled.fill(0)     # clear display
    draw_ellipse(64, 32, 20, 20, fill=1)     # draw the final circle
    draw_ellipse(64, 32, 18, 18, fill=0)     # outline
    oled.text(result, 60, 28, 1)             # center the result ("H" or "T")
    oled.show()
    time.sleep(0.5)     # show the result

    return result

# main game function
def game():
    oled.fill(0)
    oled.text("Left for Tails", 10, 25) 
    oled.text("Right for Heads", 6, 45)
    oled.show()

    # wait for the user to press a button
    while True:
        if not button_heads.value():     # check if heads button is pressed
            oled.fill(0)
            oled.text("You chose Heads", 5, 10)
            oled.show()
            time.sleep(1)
            user_guess = "H"
            break
        elif not button_tails.value():     # check if tails button is pressed
            oled.fill(0)
            oled.text("You chose Tails", 5, 10)
            oled.show()
            time.sleep(1)
            user_guess = "T"
            break

    result = coin_flip_animation()     # flip the coin and get the result

    # check if the user won
    oled.fill(0)
    if user_guess == result:
        oled.text("You win!", 27, 10)
    else:
        oled.text("You lose!", 27, 10)
    oled.show()
    time.sleep(2)     # show the result
    oled.fill(0)
    oled.show()       # clear the screen

game()   # run the game
```

Coin Flip Demonstration - https://youtu.be/iaiqS3QNyAw

---

## New Physical Elements

To some people this next stuff might not be as cool as the coin flip game I just showed but it introduces some more things I had never worked with prior to my IoT Systems class which is why I want to talk about it.

### Potentiometer

For our second lab we used something called a potentiometer, it has three pins: two of which are connected to a resistive element, and the third being attached to a wiper that moves. It is a type of variable resistor that allows for adjusting the resistance manually, in my case I’m using a rotary potentiometer which is controlled by turning a dial. For the setup of a potentiometer, one of the two outside pins is grounded while the other is connected to the 3V3(OUT) power pin, and the third middle pin is connected to any of the ADC pins on the Pi.

Some simple code below allows for the brightness of an LED to be changed while also printing out the brightness percentage. This is done by reading the potentiometer value, setting the LED brightness to it, and then printing the calculated percentage.

```python
from machine import ADC, Pin, PWM
from time import sleep

# Initialize the ADC pin connected to the potentiometer
potentiometer = ADC(26)  # GP26/ADC0

# Initialize LED on GPIO pin 15
led = PWM(Pin(13))
led.freq(1000) # Set PWM frequency

# Conversion factor for 12-bit resolution (0–4095 to 0–100%)
conversion_factor = 65535 / 100

try:
    while True:
        # Read the raw ADC value 
        raw_value = potentiometer.read_u16()
        led.duty_u16(raw_value) # Set LED brightness
        
        # Convert the ADC value to a percentage
        percentage = int(raw_value / conversion_factor)
        
        # Print the percentage value
        print(f"Brightness Percentage: {percentage}%")
        
        # Delay for readability (adjust as needed)
        sleep(0.05)

except KeyboardInterrupt:
    # Turn off the LED
    led.duty_u16(0)
```

### 10 Segment Bar Graph Display

Very simple to imagine: ten LED bars in a row connected together into one display. Each LED has two pins, one needs to be connected to a GPIO pin and the other grounded. These can probably change depending on the part but for the most part it looks like the only difference in these bar graph displays is the LED colour.

### Code Walkthrough

If you want to follow along again, here is a list of what I used to combine the above two pieces and make one pretty cool project:

* x1 - Raspberry Pi Pico (mine is a Pico W, doesn’t matter though)
    
* x1 - Breadboard (recommended at least 50 rows)
    
* x10 - 220 ohm resistors (this would change depending on the LED colour)
    
* x1 - 10 segment bar graph display
    
* x14 - Cables
    
* x1 - Potentiometer
    

Here is how I setup my breadboard:

* Ground the negative rail on the breadboard (**white cable below on the right**)
    
* Potentiometer - requires three cables at the top
    
    * GND (**white cable below on the left**) - connected to the negative rail of the breadboard, ensure it’s grounded, or connect the cable directly to one of the GND pins of the Pico
        
    * VCC **(long red cable below)** - connected to pin 36, the 3V3(OUT) power pin
        
    * ADC (**long blue cable below**) - connected to pin 31, GP26/ADC0
        
* 10 segment bar graph display **(black and white bar below)**
    
    * Each LED segment requires a cable on one side connected to a GPIO pin **(rainbow assortment of cables below)**, I used GPIO pins 6-15
        
    * Each segment also needs to be grounded, I used one resistor on each, connected to the negative rail of the breadboard
        

Here is what mine looked like in reference to what I explained above:

![](/assets/images/raspberryPic2.jpeg "Bar Graph Setup")

Once you have set this up, if you use the code I included below, as you turn the knob clockwise on the potentiometer, the LEDs on the display will get brighter from right to left; if you turn it counter-clockwise, they will get dimmer. It will also print the brightness percentage of the display in the terminal. If you’re interested, I have included a link to a video below the following code, demonstrating everything combined and running together.

```python
from machine import Pin, ADC, PWM
import utime

# GPIO pins for each LED in the display
led_pins = [6, 7, 8, 9, 10, 11, 12, 13, 14, 15]

# each LED pin as a PWM output to control brightness
pwm_leds = [PWM(Pin(pin)) for pin in led_pins]
for pwm in pwm_leds:
    pwm.freq(1000)

# potentiometer input GP26/ADC0
potentiometer = ADC(26)

while True:
    # read the potentiometer value (0-65535)
    pot_value = potentiometer.read_u16()

    # map the potentiometer value to the number of LEDs
    num_leds_on = int(pot_value / 6553.5)

    # map the potentiometer value to brightness
    brightness = int(pot_value)

    # turn on LEDs based on the number and control brightness with PWM
    for i in range(10):
        if i < num_leds_on:
            pwm_leds[i].duty_u16(brightness)
        else:
            pwm_leds[i].duty_u16(0)
            
    # convert the ADC value to a percentage
    percentage = int(pot_value / (65535 / 100))
    
    # print the percentage value
    print(f"Brightness Percentage: {percentage}%")

    utime.sleep(0.1)  # small delay for smooth updates
```

Potentiometer and Bar Graph Display - https://youtu.be/pedJWBaIUEM

---

## Conclusion

Raspberry Pi’s are pretty cool and capable of so much even when combined with such small and simple physical elements. Each physical element opens up the door of possibilities for projects with the Pi and combining them can make for some pretty interesting things. I love getting to work with the Raspberry Pi and I’m really excited to learn and work more with them as the rest of the semester goes on. I know I’ve barely broken the ice for what can be done with them but I feel like this has given me a solid base to work with and it’s a great start.
