## GPIO Introduction

**G**eneral **P**urpose **I**nput **O**utput is a feature of the Raspberry Pi which lets you interact with real-world components like LEDs, switches, sensors, motors, and much more.

The GPIO pins are:

* [Raspberry Pi 3 and 2 and B+](http://i.imgur.com/tZPijCk.png)
* [Raspperry Pi 1B](http://i.imgur.com/ELzPEgJ.jpg)
* [All models including old Raspberry Pi 1](http://i.imgur.com/g3Phdyw.png)

Start with this Adafruit learning series: https://learn.adafruit.com/series/learn-raspberry-pi

## Project Ideas

There are many project ideas linked in the sidebar of [r/raspberry_pi](https://www.reddit.com/r/raspberry_pi/).

Here is a rough progression of things a beginner could try:

* Blink an LED
* Make a row of LEDs follow back and forward like Night Rider
* Read some data from the real world like a temperature/humidity sensor
* Make something happen when you press a button
* Detect movement/objects with a PIR or ultrasonic sensor

## Component Kits

You can get many cheap kits of parts off eBay/Amazon but they tend to just be random things thrown in together. Once you start doing a particular project, you often find the kit doesn't have the part you need and you end up having to buy stuff from the electronics store.

The best kits are the ones that come with a book, and the kit lets you do everything described in the book, but those are also the most expensive ones.

A good kit will have a breadboard, DuPont wires, some LEDs, a large collection of resistors of at least a dozen resistances, and a variety of modules with breadboard spacing.

## Power Requirements and Arduino Compatibility

The Pi uses 3.3V TTL. Many components, including those intended for Arduino, use 5V TTL. If you feed your Raspberry Pi 5V TTL you will fry it, so learning to make a *voltage divider circuit* and use a *level shifter IC* should be on your todo list. You'll almost certainly need these as you progress onto a wider range of projects:

* https://learn.sparkfun.com/tutorials/voltage-dividers
* https://learn.sparkfun.com/tutorials/using-the-logic-level-converter

As a rule of thumb, if a component needs 5V power, it probably supplies 5V TTL.

Don't power anything requiring more than 16mA off the GPIO pins, and don't power anything requiring more than 50mA off the 3.3V GPIO pin. The 5V GPIO pins come thru from the power supply, limits are discussed more on the [Raspberry Pi Power](Raspberry-Pi-Power) page. If you're doing motors keep this in mind, those can draw over 200mA under load.

## Education and Research

As you might have picked up, Adafruit and Sparkfun are good places to start learning about components and concepts:

* https://learn.adafruit.com/
* https://learn.sparkfun.com/

If you have a particular sensor you wish to use, search Google for the part number and "raspberry pi". This will often provide tutorials and Python libraries, so you can get up and running using your component as quickly as possible.

For example, if we want to use a DS1307 Real Time Clock, search for `DS1307 raspberry pi`.

If you are using Python, you should also search the Python Package Index at https://pypi.python.org/ or by using `pip search partnumber` on the commandline, there may be a library in there too.

If you cannot find a pre-existing library, then look up the manufacturer's page for the component and get the Data Sheet. This wil explain how to interface with the component, though it's not always easy to figure out, and is probably a task for advanced users.

## Further Reading

* http://elinux.org/RPi_Tutorial_Easy_GPIO_Hardware_%26_Software
* http://elinux.org/RPi_GPIO_Code_Samples
* http://jamesreubenknowles.com/level-shifting-stragety-experments-1741