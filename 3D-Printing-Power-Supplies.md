## Power Requirements

Watts = Volts x Amps

* Heatbed 200x200 MK3 = 120W (12V 10A)
* Hotend, generic = 40W (12V 3.3A)
* Hotend, E3D = 30W (12V 2.5A)
* Motors NEMA17, four of = 25W (3.75V 1.5A each, typical estimate)
* Arduino Mega = 2.5W (0.5A 5V)
* Fans, two of = 3.2W (0.15A 12V each, high estimate)

So without a heatbed you need about 75W (12V 6.25A) which includes a little overhead.

With a heatbed you need at least 200W (12V 16.7A).

It's fairly common to run the electronics on one supply and the heatbed on another supply via MOSFET/SSR, especially if using 12V electronics and 24V heatbed.

## Power Bricks

For electronics-only or for printers without a heated bed.

* Switchable aftermarket laptop power bricks are available in a selection of fixed wattages, notably 90W and 120W. For variable voltage supplies, check it can actually supply sufficient current at 12V, some have a high current rating overall but can only do a lower current at 12V. Probably better to over-rate this because China. AU$15 to AU$35 on Aliexpress and eBay, search "12v 120w power supply". Variety of barrel jacks.
* XBox 360 Slim power brick 135W. 129W 12V 10.83A and 5W 5Vsb 1A. About AU$25. Dual barrel connector.
* Xbox 360 Original power 203W. 198W 12V 16.5A and 5W 5Vsb 1A. About AU$35 and harder to find because old. 6-pin Molex(?) connector.

## ATX Power Supply

* Hook ATX green to `PS_ON` (D12) on RAMPS.
* MKS boards don't expose D12 so sacrifice a servo pin like D11 and edit the relevant `pins.h`.
* 12V is yellow, 5V is red.
* Some ATX power supplies need a load on 5V to regulate the 12V load properly. A 4R7 10W resistor provides enough but gets too hot to touch. Two resistors in parallel should be cooler. You can also place the resistor in the path of the power supply fan to cool it. You can get "chassis mount" resistors which come in a metal heatsink.
* Suggested resistors based on Ohm's law and guesswork:
    * Wattage - Single or Parallel
    * 10W - 4R7 or 3R9
    * 20W - 4R or 2R
    * 50W - 2R or 1R
    * 100W - 1R or 0R5
* In Marlin, set `#define POWER_SUPPLY 1`
* G-Code `M80` to turn on, `M81` to turn off.
* If powering off at the end of prints, keep in mind you'll turn off 12V including the hotend fan which could lead to heat creep into the coldend and melt your filament into the heatbreak. Maybe try `M109 S45` to wait until the heater block cools down. From memory there's a timeout on that command. Maybe it's better to use `M85 S900` (timeout after 15 minutes) to let the printer cool down then power off.
* **Most importantly**, check the power supply specs! Most power supplies break into various "rails" which provide different currents. It's common to run electronics on one rail and heatbed on another.
* Power supplies known to provide one big 12V rail:
    * EVGA, eg: [430 W1 White](http://au.evga.com/products/product.aspx?pn=100-W1-0430-KR)
    * Corsair, eg: [CX450](http://www.corsair.com/en-us/cx-series-cx450-450-watt-80-plus-bronze-certified-atx-psu-na) and [VS400](http://www.corsair.com/en-gb/vs-series-vs400-400-watt-80-plus-white-certified-psu-na)
* Modular power supplies help identify rails and keep the wiring tidy.

## References

* http://reprap.org/wiki/Heatbeds_-_A_beginner%27s_guide
* http://reprap.org/wiki/PCB_Heatbed
* https://tlextrait.svbtle.com/arduino-power-consumption-compared
* https://github.com/foosel/OctoPrint/wiki/Control-your-printer%27s-ATX-PSU-through-a-RAMPS-board-using-OctoPrint
* https://www.reddit.com/r/3Dprinting/comments/5616h3/need_help_on_how_to_configure_ps_on_pin_on_a_mks/
* http://doc.3dmodularsystems.com/atx-power-supply-conversion-for-3d-printer-usage/
* http://reprap.org/wiki/PC_Power_Supply
* https://brazenartifice.wordpress.com/2011/12/26/atx-power-supply-5v-load-resistor-for-better-12v-regulation/
* http://reprap.org/wiki/Balancing_ATX_Supplies