This page talks about why to use an external heatbed MOSFET.

## Introduction

3D printers have a control board which drives the electronic components such as motors and heaters.

The heaters are switched on and off rapidly, usually using a PWM signal, to maintain a steady temperature based on feedback from thermistor temperature sensors.

The microcontroller cannot switch the heater signal directly, so a transistor (aka MOSFET) is used as a switch.

## Problem 1 - Board Connectors

The idea of plugging the heated bed wires directly into the control board comes from early RepRap days when heated beds were hand-made using nichrome wire or resistor arrays and output about 60W (12V 5A) or 100W (12V 8A).

Most 3D printer control boards use bed wire connectors which can handle up to 15A current, but most heatbeds these days draw much more current than this.

Using a figure of 0.5W per square centimeter, a 200mm bed will draw at least 200W (12V 17A), and a 300mm bed will draw 450W (12V 38A or 24V 19A). These figures are more than the rated capacity of the wire terminal.

Most screw-type wire terminals will work slightly above their rating **if** the wire connection is good. However, if the bed wires in that connector slightly loosen with vibration or time, or the bed is just too powerful, you'll melt the board connectors and possibly start a fire.

## Problem 2 - MOSFET choice

As stated above, the bed load is switched on and off rapidly using a transistor. The larger load a transistor has to switch, and the more resistance the transistor itself has, the hotter the transistor gets as it switches the load on and off.

The original [RAMPS design](http://reprap.org/wiki/RAMPS_1.4) uses a P55NF06L transistor, and many cheap RAMPS clones these days come with P60NF06 transistor. Both of these have a high resistance (RDSon at 4.5V = 20mO) which means they are able to switch a small load, such as the hotend or a weak old heatbed, but will get very hot when switching a large load such as a modern heatbed.

You can work out the expected temperature rise.

When switching a 4A (12V 48W) hotend current, this transistor will raise 20 degrees C above ambient temperature, that's fine.

However, when switching a 17A (12V 200W) bed current, this transistor will raise **about 350 degrees C** above ambient temperature!

That's well beyond the failure temperature of the component, beyond the holding temperature of the solder, and beyond the melt temperature of the surrounding components.

If you've ever bought a $7 RAMPS and let out the "[magic smoke](https://en.wikipedia.org/wiki/Magic_smoke)" the instant you heated the bed, that is why.

There are some control boards (MKS boards, StaticBoards Premium RAMPS, Cohesion3D, etc) which either use a better transistor with a lower resistance and so lower operating temperature, or which use a surface-mount transistor which can cool through the board's ground plane.

These solve the problem of the board's bed transistor getting too hot, but do not solve the previous problem of under-rated board connectors.

## Solution - External MOSFET

An external MOSFET is the board pictured here, with some suggested purchase links:

![](https://i.imgur.com/aishAV6.jpg)

* [From Makerbase](https://www.aliexpress.com/store/product/3Dprinter-heat-control-MKS-MOSFET-for-heated-bed-printer-head-MOS-30A/1047297_32405884519.html)
* [From Tevo](https://www.aliexpress.com/item/3D-Printer-parts-heating-controller-MKS-MOSFET-for-heat-bed-extruder-MOS-module-exceed-30A-support/32789089967.html)

(*I am not affiliated with either of these companies, they are just ones I have had good personal experience with*)

This board contains large connectors to the supply voltage and the heatbed, a transistor which can switch a large load, a heatsink to cool that transistor, and a smaller wire connector to accept bed switching signal from the 3D printer control board.

The small signal current between the external MOSFET and the control board's heatbed connector ensures the load on the board transistor is low, so does not exceed the current of the bed connector, and does not cause the board transistor to heat excessively (or really at all).

The large wire terminals, which are designed to accept a ring or fork connector crimped onto the heatbed wires, are rated to handle much higher current, so can pass the bed power without melting.

## Conclusion

With larger modern heated beds, running the bed current through the control board connectors is no longer viable.

I would not run a modern heatbed through any control board.

Always use an external bed MOSFET.

## Additional

### How about the hotend?

It is not necessary to switch the hotend with an external MOSFET, as this load is far to low to exceed the rating of the board connectors, or to cause even a poor transistor like the RAMPS to get very hot.

You can do it if you want, but it's perfectly safe to heat the typical 30W or 40W or 50W heater cartridge through the control board.

### This cheaper MOSFET type

It is common to see this smaller different type of external MOSFET for about half the price of the ones above:

![](https://i.imgur.com/CqmBV8y.jpg)

At the time of writing (mid 2018) there have been multiple reports across multiple 3D printing communities of these getting unnecessarily hot during heatbed operation - up to 100C at the heatsink. This is far too hot. You should be able to comfortably keep your finger on the heatsink during normal bed operation.

There appears to be some supply of this style of MOSFET which has a counterfeit or inappropriate component which results in too much heating.

I own two such boards and they work fine, and I'm sure there are plenty of people buying these and finding they work well too, but be aware that if you buy this cheaper type then you may end up with a dodgy one which gets too hot and need to buy a different one to replace it.

If I needed a new bed MOSFET, I would buy one of the MKS or Tevo ones, as they are from a fairly reliable manufacturer with good production consistency.

It is worth the extra 10 bucks to avoid your house burning down.

### Can I use a relay?

Maybe.

If it's a relay which goes "click" as it turns off and on, this is no good as it's a mechanical relay and can't switch fast enough for the rapid signal that comes from the control board.

If it's a Solid State Relay (SSR) which is designed to switch DC current using a DC signal, then yes that could be used. Make sure it is rated for switching more than your bed current on the load side.

Make sure you buy a good quality relay from a good electronics supplier like RS or Farnell, not a cheap fake relay from some dodgy foreign-sourced eBay or Amazon or Aliexpress seller.

Big Clive found this dangerous DC-AC counterfeit relay which was poorly made and actually rated for half what was written on the outside: https://www.youtube.com/watch?v=DxEhxjvifyY

### How about AC/mains heatbed?

I don't think hobbyists should use AC power for their heatbed, as it can be dangerous if setup wrong.

Sure, DC power can be dangerous too, but the power supply fuse and circuit provides some protection around this and I think AC has potential to be more harmful.

If you want to setup an AC mains heatbed then do the certification which electricians are required to do in your area so they can legally touch mains power. Then you should know how to setup the bed safely without killing yourself or someone else around you.