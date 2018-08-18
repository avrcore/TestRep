Most people do not need additional cooling.

If your Pi is sitting there idle, as a personal desktop/server, or playing games, you probably don't need to worry about it.

Consider additional cooling if you're overclocking, running all cores at 100% usage, or storing the Pi in a very hot cramped environment.

### Heatsinks and Fans

The little 15x15x4mm heatsinks aluminium/copper heatsinks commonly seen in most kits barely do anything, a few degrees at most, they are a waste of time.

Some of them even come with regular double sided tape which probably insulates and keeps heat in instead of conducts heat away!

This guy did a good series of heatsink tests from an 85C baseline:

* <https://www.youtube.com/watch?v=e6okZKRwnTQ> - Tiny heatsink = 80C
* <https://www.youtube.com/watch?v=5Ud-grj4Zl0> - Tiny heatsink and fan = 60C
* <https://www.youtube.com/watch?v=1AYGnw6MwFM0> - Giant heatsink = 50C
* <https://www.youtube.com/watch?v=WfQMLInuwws> - Giant heatsink and fan = 35C

So, the bigger the passive heatsink the better. A fan also helps.

I have fitted several Pi models with 25x25x15mm heatsinks using good 3M conducive double sided adhesive. They *juuust* fit most cases with a flat top. They end up the same height as the USB ports.

### Fan Power

You can power a small 5V fan off the 5V GPIO pin. A 40mm fan should be enough, but you could go as small as 20mm. Try to get a quiet fan. Just moving the hot air away is probably enough, you don't need a noisy jet turbine.

The 5V GPIO pins are passed through directly from the microUSB power supply connector, so you can drive a fan off the 5V pin safely. A fan will typically draw around 200mA. Most fans are rated in watts and `Watts / Volts = Amps`, so say `0.5W / 5V = 0.2A (200mA)`.

The max current able to be drawn through the entire 5V line is covered at [Raspberry Pi Power](https://github.com/superjamie/lazyweb/wiki/Raspberry-Pi-Power).

**Do not use the 3.3v or other GPIO pins to power high current devices!!!** The rest of the GPIO pins pass through the SoC and can only supply very limited current. The exact number seems unknown, but a safe max seems 50mA over all GPIO pins, and max 16mA per pin.

References:

* http://raspberrypi.stackexchange.com/questions/9298/what-is-the-maximum-current-the-gpio-pins-can-output
* https://raspberrypi.stackexchange.com/questions/51615/raspberry-pi-power-limitations
* http://elinux.org/RPi_Low-level_peripherals#Power_pins
* https://www.raspberrypi.org/forums/viewtopic.php?f=44&t=12498

### Thermal Test

* CPUBurn-A53

~~~
wget https://raw.githubusercontent.com/ssvb/cpuburn-arm/master/cpuburn-a53.S
gcc -o cpuburn-a53 cpuburn-a53.S
while true; do vcgencmd measure_clock arm; vcgencmd measure_temp; sleep 10; done& ./cpuburn-a53
~~~

* Linpack

~~~
sudo apt-get install libmpich-dev
wget http://web.eece.maine.edu/~vweaver/junk/pi3_hpl.tar.gz
tar -xvzf pi3_hpl.tar.gz
chmod +x xhpl
./xhpl
~~~