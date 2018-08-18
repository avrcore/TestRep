Many people repeat the meme that the Raspberry Pi *requires* a 2.5A power supply but this is not necessarily true.

This page attempts to quantify Raspberry Pi power requirements, then recommend some power supplies.

## Power Requirements

* The SoC (CPU and GPU) draw:
    * Pi 3: 1.35A under stress test, typical usage like 800mA, and 300mA at idle
    * Pi 2: 850mA under stress test, and 300mA at idle
    * Pi 1: 500mA under stress test, typical usage like 500mA, and 200mA at idle
    * Pi Zero: 350mA under stress test, typical usage like 250mA, and 100mA at idle
* The USB controller chip on the Pi 1/2/3 requires 240mA
* The LEDs require 5mA each
* HDMI output required 25mA to one monitor tested
* The built-in Ethernet requires 2mA
* Pi 3 built-in Wifi consumes around 20mA when idle
* Unknown to me: Max current requirements of the Pi 3's onboard Wifi and Bluetooth

So far that's maximum 1.5A for a Pi 3, and around 1.1A for the other models. From there, the difference comes in the USB peripherals you plug in, and things you power off the 5V GPIO pins. The USB specification mandates that a port supply 500mA, however each model has a max total current it can supply to all USB ports:

* Pi 1: 500mA
* Pi 1B and Pi 2: 600mA, or 1.2A with `max_usb_current=1`
* Pi 3: 1.2A

## Power Draw Limit

The max current able to be drawn through the entire 5V line from the microUSB power connector is:

* Early/Asian Pi 1 with T075 polyfuse = 750mA (fuse blows at 1.1A)
* Late/British Pi 1 with miniSMDC075 polyfuse = 750mA (fuse blows at 1.5A)
* Pi 1 B+ with 1812L or similar polyfuse = 2A (fuse blows at 3.5A)
* Pi 2 with 204L polyfuse = 2A (fuse blows at 4A)
* Pi 3 with MF-MSMF250 polyfuse = 2.5A (fuse blows at 5A)

So assuming the SoC and USB on a Pi 3 are drawing say 1.5A, the most you should really plug into the USB ports is about 1A. The Pi 1 seems to already be at the limit of what it can reliably draw and a [powered USB hub](http://elinux.org/RPi_Powered_USB_Hubs) should probably be used for anything serious.

The Pi Zero has no polyfuse or current protection anywhere and its SoC power usage is much lower, you can safely backpower a standalone Zero off a regular computer USB port and even run it in USB Gadget mode to get networking.

The HDMI port is current-limited to 200mA. It is expected that an unpowered HDMI-to-VGA adapter would draw more current than just a plain monitor.

References:

* https://www.raspberrypi.org/help/faqs/#powerReqs
* https://en.wikipedia.org/wiki/Raspberry_Pi#Specifications
* http://www.jeffgeerling.com/blogs/jeff-geerling/raspberry-pi-zero-power
* http://www.jeffgeerling.com/blogs/jeff-geerling/raspberry-pi-zero-conserve-energy
* http://www.pidramble.com/wiki/benchmarks/power-consumption
* https://www.raspberrypi.org/documentation/hardware/raspberrypi/schematics/README.md
* http://www.circuitbasics.com/raspberry-pi-zero-ethernet-gadget/

## USB Device Power Consumption

You can quantify power requirements of any specific device you have with `lsusb -v` and the `MaxPower` property.

Just because a device claims to require a certain current doesn't necessarily mean the device draws that current constantly or at all, it's just a maximum. For example, USB wifi dongles claim to require 450mA or more, but Jeff Geerling found his USB wifi dongle only drew 40mA at idle.

I am keeping a database of [game controller USB power consumption](Game-Controller-USB-Power-Database).

USB keyboard/mouse usually require 50mA to 150mA each.

## Power Supplies

**Don't use cheap rubbishy power supplies!!!**

If you're seeing the small rainbow square or lightning bolt in the top right corner, your Raspberry Pi is not getting enough power. Specifically, power has dropped to 4.65V or less.

Note this is *not* the giant rainbow splash screen which appears on boot. That's the GPU self-test and is perfectly normal. If desired, you can disable the rainbow splash screen with `disable_splash=1` in `/boot/config.txt`.

The red power LED will also blink if power drops below this level. This LED is connected to an APX803 supervisor chip which blinks the LED when power drops below 4.65V. This is visible on the schematics of the Pi B+ and Pi 3, look for `PWR_LOW_N`.

If you're seeing the yellow/orange/red square in the top right corner, those are temperature warnings. I have another page on [Raspberry Pi Cooling](https://github.com/superjamie/lazyweb/wiki/Raspberry-Pi-Cooling).

There are many aspects to a good power supply, covered in great detail on [Ken Shirrif's blog](http://www.righto.com/2012/10/a-dozen-usb-chargers-in-lab-apple-is.html).

I have used iPad chargers exclusively since I discovered Ken's blog, and have almost never seen the rainbow under-voltage square. You can get them in stores everywhere for under AUD$30.

It's true that the iPad charger does sag voltage at max load, but with my usages all being under 2A I'm happy I'm getting enough power.

It's true that HP and Samsung chargers are better, but they are hard to find in stores, and I don't trust eBay sellers enough that I believe they're genuine items and not inferior clones.

Other good choices are probably the [Adafruit Power Supply](https://www.adafruit.com/product/1995) and the [Official Raspberry Pi Power Supply](https://www.raspberrypi.org/products/universal-power-supply/).

The USB cable you're using matters as well. I've had cheap USB cables which could barely carry 200mA. Monoprice and BlitzWolf are good brands for USB cables. Fast-charge cables which come with brand-name smartphones and tablets are probably a good choice too.

You'll always get some voltage drop over a USB cable, a typical 1 metre cable drops about 0.25V. Some power supplies (like Adafruit) make up for this by intentionally supplying 5.25V. A very long cable like 3 metre or more is probably unsuitable to power a Raspberry Pi.

The Raspberry Pi 2 and Raspberry Pi 3 are said to be less sensitive to lower voltage and voltage fluctuations than the Raspberry Pi 1. 

References:

* http://www.righto.com/2012/10/a-dozen-usb-chargers-in-lab-apple-is.html
* https://www.raspberrypi.org/forums/viewtopic.php?t=82373
* https://www.reddit.com/r/raspberry_pi/comments/4wotty/can_we_get_a_better_explanation_on_the_red_led/
* https://www.raspberrypi.org/documentation/hardware/raspberrypi/schematics/README.md

## Power Cables

The cable which carries the power is also important. If receiving the under-voltage rainbow or lightning bolt, try a different cable. A good smartphone or tablet fast-charge cable should be sufficient.

There is always some voltage drop over any cable, some power supplies intentionally output slightly more than 5V to account for the drop over a typical cable (eg: [5.25V 2A over 1M](http://www.calculator.net/voltage-drop-calculator.html?material=copper&wiresize=52.96&voltage=5.25&phase=dc&noofconductor=1&distance=1&distanceunit=meters&amperes=2&x=51&y=21)).

A longer or thinner cable makes this worse, so don't use a 6-metre USB cable to power your Pi. Cheap crappy cables tend to have thinner wire, worse connectors, and degrade quickly over time.

You can get power cables with switches (to save you constantly unplugging the Pi). Cheap ones of these tend to cause a voltage drop, though good thick wire and quality connectors should help avoid any problem. This one from Adafruit should be good: https://www.adafruit.com/products/2379

As a recommendation, you want 22AWG wire or thicker (up to maybe 20AWG is realistic) and probably 1M (3 feet) or shorter length.

References:

* http://www.calculator.net/voltage-drop-calculator.html
* http://www.powerstream.com/Wire_Size.htm