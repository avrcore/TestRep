# Stepper Motors

Learn how stepper motors work:

* https://www.youtube.com/watch?v=eyqwLiowZiU
* https://www.youtube.com/watch?v=bkqoKWP4Oy4

Modern 3D printers usually use NEMA17 motors which are 1.7" (43.2mm) square across the face. There are larger sizes like NEMA23 and NEMA34 which are common on CNCs. There are smaller sizes like NEMA14 and NEMA11 too. The first RepRap 3D printers used NEMA14s but the most powerful ones (40Ncm) were right on the limit of what they could do well.

Look up your motor's part number and pay attention to the **Step Angle**, **Holding Torque**, **Rated Current**, **Voltage**, and possibly **Inductance**.

3D printers need at least 35 Ncm holding torque. Generally the longer the motor the more torque it has, but not always, there are some "pancake steppers" specifically designed to be high-torque in a small light package.

You can gear up a motor, either with metal gears or printed gears, to increase the torque at the cost of decreasing speed. For example, a 3:1 gearset triples torque but results in a third of the speed.

Ideally you want the highest torque for the lowest current within the driver's capabilities. A 50Ncm 1.5A motor is a better choice than a 50Ncm 3A motor, none of the common drivers can even supply 3A.

Some people think it's better to pick a very powerful motor and under-drive it, to give a wild example pick a 100Ncm 2A motor and run it at 1A. Torque-to-current doesn't scale linearly, so it's probably hard/impossible to calculate effective torque at a given current. You could measure it. Speed also affects torque.

Not every motor vendor publishes in Ncm so check other stupid manufacturer measurements at a torque converter like: http://www.numberfactory.com/nf%20torque.htm

* 1 N-cm = 0.1019716 kg-cm
    * 40 Ncm = 4.08 kg-cm
* 1 N-cm = 1.41612 oz-in
    * 40 Ncm = 56.64 oz-in

I find my extruder needs more current (1A to 1.2A) than the XYZ motion motors (0.6A to 0.8A) on my 1.3A 35Ncm motors.

Use the Step Angle to work out motor steps for firmware with: http://www.prusaprinters.org/calculator/

Also note there's really no point going above 1/32 microstepping with an ATmega2560 (RAMPS), it can only supply that many steps reliably at 100mm/sec. You need at least that to bridge.

* http://reprap.org/wiki/Step_rates

~~~
TODO: Write my own Step Rates page, or maybe just correct RRW
~~~

## How to Tune Stepper Drivers

Turn power off, unplug the stepper motor cables, turn power back on and tune the stepper drivers.

When done, turn power off, plug in the stepper motor cables, turn power back on and test motor movement.

Don't tune stepper drivers with the motors plugged in, if you accidentally set current too high you can fry the motor or the stepper.

Don't plug or unplug stepper motors with the power on.

Measure DC voltage between the stepper trimpot and 12V ground. The ground at the 12V supply to the control board is fine to use.

Look up the correct current for your motor part number. If you have cheap Chinese motors with no part number, assume they have a max of 1.25A to be safe.

Look up the proper formula for your stepper drivers below, and find the voltage which corresponds with the current you want to set.

Pro tip: Get slip-on alligator clips for your multimeter. Clamp ground to a 12V ground wire and clamp positive to your screwdriver. This way you'll measure the voltage as you adjust and don't need three hands.

## References

* http://reprap.org/wiki/Stepper_motor
* http://reprap.org/wiki/NEMA_17_Stepper_motor

## Where to Buy in Australia

* [Aus3D](http://stores.ebay.com.au/aus3d-shop)
    * NEMA17 - 50Ncm 1.68A - <http://www.ebay.com.au/itm/331816179183>
* [LearCNC](http://stores.ebay.com.au/learcnc)
    * NEMA17 - 67Ncm 1.5A - <http://www.ebay.com.au/itm/201006654389>
* [Stepper Online](http://stores.ebay.com.au/au-stepperonline)
    * NEMA17 - 45Ncm 2.0A 2.2V, 59Ncm 2.0A 2.8V, 65Ncm 2.1A 3.36V
    * NEMA14 - 40Ncm 1.5A 4.2V
    * All other sizes NEMA8 thru NEMA34
    * Pay attention, they sell a lot of motors with low current but high voltage (10V) demands

Or China lol

* 42HD4027-01 - 40Ncm 1.5A 3.8mH 40mm - [$16.65 ea](https://www.aliexpress.com/item/High-Quality-Nema17-Stepper-Motor-400mN-m-Min-42HD4027-1-5A-1-8degree-3D-printer-motor/32807398931.html)
* 42SHD0217-24B - 50Ncm 1.5A 5mH 40mm - [$60 set with extra 48mm motor motor](https://www.aliexpress.com/item/5pcs-3D-Printer-Nema-17-Stepper-Motors-42SHD0404-1-7A-Motor-4pcs-42SHD0217-1-5A-CNC/32824764988.html)
* 17HS8402 - 52Ncm 1.3A 3.2mH 48mm - [$15.72 ea](https://www.aliexpress.com/item/Free-shipping-1pcs-Nema17-Stepper-Motor-48mm-1-3a-52N-cm-Nema-17-motor-42BYGH-1/32803563953.html)
* 17HS4401 - 40Ncm 1.7A 2.8mH 40mm - [$8.888 ea (5x $44.40)](https://www.aliexpress.com/item/5pcs-4-lead-Nema17-Stepper-Motor-42-motor-Nema-17-motor-42BYGH-40MM-1-7A-17HS4401/32376023464.html) or [9.27 ea](https://www.aliexpress.com/item/CE-certification-1pcs-4-lead-Nema17-Stepper-Motor-42-motor-Nema-17-motor-42BYGH-1-7A/32377416566.html)

## Motors I Own

TEVO Tarantula ships with TEVO-branded `17HD40005-C5.18`.

The [TEVO Black Widow Community Guide](http://bit.ly/2k1dBoZ) says:

> TEVO has officially confirmed that these are an OEM version of the previously shipped `Busheng 17HD40005-22B`.

Aus3D sells Casun `42SHD0217-24B`, and I got some off Aliexpress in a pack as well with a longer motor.

The Casun website says 45Ncm but is unclear on other details. The datasheet on Aus3D says 50Ncm.

Internet pictures I can find say these:

| Manufacturer | Part No       | Step Angle | Holding Torque | Current | Voltage | Inductance | Length | Weight
|---           |---            |---         |---             |---      |---      |---         |---     |---
| Busheng      | 17HD40005-22B | 1.8°       | 36Ncm          | 1.3A    | 2V      | 3.2mH      | 40mm   | 294g
| Casun        | 42SHD0217-24B | 1.8°       | 50Ncm          | 1.5A    | 3.75V   | 5.0mH      | 40mm   | 284g
| Casun        | 42SHD0404-22  | 1.8°       | 52Ncm          | 1.7A    | 3.4V    | 3.8mH      | 48mm   | 331g

* http://www.haoyuelectronics.com/Attachment/17HD40005-22B/
* http://www.hotmcu.com/reprap-40mm-stepper-motor-p-214.html
* http://www.casunmotor.com/nema-17-stepper-motor
* http://aus3d.com.au/image/cache/catalog/products/stepper/40mmdatasheet-800x800.jpg
* http://reprap.org/wiki/NEMA_17_Stepper_motor
* http://g04.s.alicdn.com/kf/HTB1PpNjHFXXXXbQXXXXq6xXFXXXI/202171482/HTB1PpNjHFXXXXbQXXXXq6xXFXXXI.jpg
* http://g01.s.alicdn.com/kf/HTB12205FpXXXXbxbFXXq6xXFXXXp/220578812/HTB12205FpXXXXbxbFXXq6xXFXXXp.jpg

# Stepper Motor Drivers

I don't think there's any point going finer than 1/16 stepping with an 8-bit controller, the little MCU step rate is unable to keep up with very high speeds. I don't really see the advantage of 1/128 stepping when 1/256 interpolation exists. This essentially limits choices to A4988 or TMC2100.

| Manufacturer      | Chip    | Max Microstep | Max Current | Datasheet
|---                |---      |---            |---          |---
| Allegro           | A4988   | 1/16          | 2A          | [PDF](http://www.allegromicro.com/~/media/Files/Datasheets/A4988-Datasheet.ashx?la=en)
| Texas Instruments | DRV8825 | 1/32          | 2.2A        | [PDF](www.ti.com/lit/ds/symlink/drv8825.pdf)
| Trinamic          | TMC2100 | 1/16 (16x)    | 1.2A        | [PDF](https://www.trinamic.com/fileadmin/assets/Products/ICs_Documents/TMC2100_datasheet.pdf)
| Allegro           | A5984   | 1/32          | 2A          | [PDF](www.allegromicro.com/~/media/Files/Datasheets/A5984-Datasheet.ashx?la=en)
| Sanyo             | LV8729  | 1/128         | 1.3A        | [Alldatasheet](http://www.alldatasheet.com/view.jsp?Searchword=LV8729)
| Sanyo             | THB6128 | 1/128         | 2.2A        | [Datasheet-PDF](http://www.datasheet-pdf.com/PDF/THB6128-Datasheet-ETC-817135)
| Shenzhenshi Yongfukang | HR4988  | 1/128    | 2A          | [PDF](http://www.szczkjgs.com/UploadFiles/fujian/3721/HR4988.pdf)


## Allegro A4988

* 1/16 stepping
* 2A constant current max
* Most common so cheap and plentiful, a few dollars for a set of 5
* Check the sense resistor on clones, sometimes it is not 0.1R

~~~
Formula: V = A * (8 * R)
Amps    Volts
1.7     1.36
1.3     1.04
1.2     0.96
1       0.8
0.8     0.64
		
Formula: A = V / (8 * R)
Volts   Amps
0.6     0.75
0.8     1
1       1.25
1.25    1.5625
1.5     1.875
		
Current Sense Resistor		
0.1
~~~

I think these Allegros are the best choice. They just work and there are no hidden problems or gotchas like other drivers. The only downside, if you can even call it that, is that the stepper motors make their usual noise and are not silent.


## Texas Instruments DRV8825

* 1/32 stepping but can be set back to 1/16
* Much stronger than A4988 to move the carriage against
* Definitely the highest stepping you'd want to use with an 8-bit controller
* 2.2A constant max, 2.5A burst

~~~
Formula: V = A * (5 * R)	
Amps    Volts
0.5     0.25
0.75    0.375
1       0.5
1.1     0.55
1.2     0.6
1.5     0.75
		
Formula: A = V / (5 * R)		
Volts   Amps
0.5     1
0.55    1.1
0.575   1.15
0.6     1.2
1       2
1.25    2.5

Current Sense Resistor
0.1
~~~

These drivers make the motors emit a high-pitched whine. This is due to the supply voltage (12V) being larger than the motor voltage (typically 2V to 2.8V) and the stepper driver cannot chop low voltage effectively, so does a large step between "low" and "off" which happens at an audible frequency.

There is a circuit which can get rid of this, with ready-made PCBs sold on Aliexpress as "TL-Smoother". Make sure you get the ones with 8 diodes and not 4 diodes! The circuit is explained at:

* http://www.engineerination.com/2015/02/drv8825-missing-steps.html

This page also discusses noise:

* http://ebldc.com/?p=187

Some people say to use 3V or 4V motors and the whine goes away. Some people say to use higher motor supply voltage (24V) and this goes away. Some people cannot solve this even with the diode circuit, it seems motor inductance also plays a part, some people say low motor inductance (2mH) can cause the whine but high inductance (5mH) avoids it.

Some people say reducing current can also prevent the noise. For boards with digipots to adjust motor current, you could set the current high when printing, and low enough to hold the Z axis up when not printing, which would at least limit the noise. G-code to do this is in the format:

~~~
M907 X1.0 Y1.0 Z1.0 E1.5
~~~

* http://smoothieware.org/supported-g-codes

Apparently using a SilentStepStick Protector may help. As far as I can tell, this is just the diode circuit on a through-board. See:

* http://forums.reprap.org/read.php?13,666367
* http://www.watterott.com/en/SilentStepStick-Protector

Due to their sudden step in supply voltage, DRVs can cause the motors to miss steps on the jump which affects print quality. A "salmon skin" pattern is a common result. This has the same root cause as the whine above, so can be resolved with the same methods. There are some prints where this is more visible than others, this test print is probably a good place to start:

* [DRV8825 stepper driver missing steps torture test by megablue](http://www.thingiverse.com/thing:1997411)

It's also been found that some have the incorrect vref attachment to the IC, so you measure supply voltage under no load, instead of the intended vref:

* Thread: https://www.facebook.com/groups/1111815785498391?view=permalink&id=1694077400605557
* Comment: https://www.facebook.com/groups/cncbuilddesign/permalink/1694077400605557/?comment_id=1696135143733116

DRVs work flawlessly for some people and give constant problems for others. Given the intermittent success and still-poorly-understood nature of these problems, I find it hard to recommend DRVs for 3D printing.

## Trinamic TMC2100

* 0.5A max constant current (no cooling)
* 1.25A max constant current (active cooling)
* 2.5A peak current
* 1/16 stepping, but the chip can interpolate 16x in hardware, so the motor sees 1/256 stepping
* Silent operation
* Expensive, even for Chinese clones
* Really only needed on X and Y motor, not Z or E

~~~
Formula: V = A / (1.77 / 2.5)
Amps	Volts
0.5     0.71
0.55    0.78
0.75    1.06
1       1.41
1.25    1.77
		
Formula: A = V * (1.77 / 2.5)
Volts	Amps
0.65    0.4602
0.77    0.54516
1       0.708
1.25    0.885
1.5     1.062
1.75    1.239
~~~

There is a lot of misinformation about these around. As I've learnt more about TMCs, I've rewritten this section three times now. All this may be wrong so caveat emptor. Do your own research and don't spend a hundred bucks on electronics just because some self-taught guy on the internet like me said something.

TMCs have a feature called **microPlyer** which takes 1/16 steps as input, and interpolates these 16x down to 1/256 steps in hardware. This smaller effective microstep results in extremely quiet print operation, apparently at the cost of some effective torque at the motor.

TMCs have two operating modes: **stealthChop** which is low torque and low noise, and **spreadCycle** which is high torque and a little noisier. There is a lot of advice out there that only spreadCycle is useful for 3D printers, but this is not true. The following research paper from Trinamic shows no effective torque difference between the two modes at 400rpm and below:

* https://www.trinamic.com/fileadmin/assets/Support/Appnotes/AN021-stealthChop_Performance_comparison.pdf

Let's calculate effective motor RPM for 3D print speeds:

~~~
360 degrees in a circle
1.8 degrees per step
200 full steps per revolution
80 1/16th steps per mm = 5 full steps per mm

60mm/sec print speed = 300 full steps per second
300 full steps per second / 200 full steps per revolution = 1.5 revolutions per second
1.5 RPS * 60 seconds = 90 RPM

100mm/sec print speed = 500 full steps per second
500 full steps per second / 200 full steps per revolution = 2.5 revolutions per second
2.5 RPS * 60 seconds = 150 RPM
~~~

So we're never getting to a speed where the torque advantage of spreadCycle becomes applicable. stealthChop should be fine. Still, many people say they needed spreadCycle to avoid skipped steps.

However, you won't be able to print very fast with an 8-bit board. TMCs tend to skip steps at higher speed. It's been thought this was due to torque losses in stealthChop but that can't be true. In the comments of the following article, Stephan Watterott theorises this is caused by multistepping:

* https://hackaday.com/2016/09/30/3d-printering-trinamic-tmc2130-stepper-motor-drivers-shifting-the-gears/

So to print consistently with TMCs you should avoid multistepping, which means staying under about 60mm/sec with the usual setup (1.8deg motors, 20T pulley, 2mm belt, 1/16 step rate) on 8-bit Arduino. 32-bit boards do not suffer such a limitation as they have a much higher step rate. I don't believe either Smoothieware or RepRapFirmware actually even do multistepping?

spreadCycle is entered by soldering two solder jumpers on the PCB, connecting CFG1 to ground. Some Chinese clone TMCs have this either mislabeled so you short out the chip and fry it, or don't have the IC's CFG1 actually exposed so you can never enter spreadCycle mode.

TMCs get up to 50C at 0.55A and will reach their 150C thermal shutoff at 1A, so require through-board heatpads attached to the bottom of the IC, large heatsinks, and active cooling (fans). A fan blowing down onto the drivers would be fine. You could even build them into a tunnel housing with two quiet fans, one blowing air in and one blowing air out, like this:

~~~
.--------------------------------------------.
[>>]    h h      h h                      [>>]
[>>]    h h      h h                      [>>]
[>>]  TMC2100  TMC2100   A4988    A4988   [>>]
[>>]   | X |    | Y |    | Z |    | E |   [>>]
'--------------------------------------------'
~~~

Some cheap Chinese TMCs have no through-pad on the bottom, so cooling is much less effective. The white FYSETC PCBs seem to be the good ones but this will probably vary per manufacturer. If they come with no solder pad or the chips are on top, do not buy.

TMCs can apparently suffer the same high-pitched whine as DRVs due to the supply voltage difference. Maybe the diode circuit will fix this. More research required.

It's said that 1/256 microstepping results in less torque, however Tom has an explanation I don't understand for why they actually result in more effective torque:

* https://www.youtube.com/watch?v=g6Bxoqr8QlY
* https://www.youtube.com/watch?v=mYuZqx8xwTg

Comments in Marlin seem to imply there's a ~140% increase in peak current vs RMS current:

~~~
#define AUTO_ADJUST_MAX     1300  // [mA], 1300mA_rms = 1840mA_peak
~~~

Running at 1v (0.75A) probably produces reliable enough torque in most decent motors.

In summary, TMCs seem like an okay option IF:

* You get good ones with proper through-board cooling
* You attach heatsinks to the cooling pads
* You cool the heatsinks properly with a fan
* You're willing to print slower than your board's single-step rate (say 60mm/sec with 8-bit Arduino)
* Your motors produce enough torque at 0.75A

## Trinamic TMC2130 and TMC2208

These are a TMC2100 with some extra features, summarised on the Watterott wiki:

* https://github.com/watterott/SilentStepStick

TMC2130 has an SPI configuration interface. The most notable feature is **stallGuard2** which detects motor load detection (i.e. skipped steps) and can be used for homing. Since the [Prusa MK3](http://www.prusaprinters.org/original-prusa-i3-mk3-bloody-smart/), their Marlin fork also has mid-print stall detection and recovery.

TMC2208 has a UART configuration interface. The most notable feature is **stealthChop2** which adds more acceleration to the stealthChop mode. There is no TMC2208 support in Marlin, so forget those, though there is a library if you want to write it yourself:

* https://github.com/MarlinFirmware/Marlin/issues/6044
* https://github.com/teemuatlut/TMC2208Stepper

The TMC2130 support is `HAVE_TMC2130` in `Configuration_adv.h` and uses the following library, which you can add through Arduino Library Manager:

* https://github.com/teemuatlut/TMC2130Stepper

The author of the libraries wrote a good usage overview on Hackaday:

* https://hackaday.com/2016/09/30/3d-printering-trinamic-tmc2130-stepper-motor-drivers-shifting-the-gears/

This author (unfortunately) chose to use some AUX3 pins which are also used by the EXT1/EXT2 LCD header. You could redefine some pins to operate with the AUX1 or AUX2 headers. I'm unclear how this would interact with the SPI channel to the sdcard. You may have to make some ugly adapter to use both LCD sdcard SPI and TMC2130 SPI at the same time.

The FYSETC clones I have seen don't appear to have proper through-board cooling, so genuine Watterott seems to be the only option here.

## Shenzhenshi Yongfukang (Heroic) HR4988

* 2A constant current
* Apparently no cooling issues even at max current
* 1/128 stepping, 32-bit board required
* Use the same formula as A4988, they apparently come with 0.1R sense resistors

Cheap Chinese steppers available on Aliexpress, $25 for a set of 5.

This seems too good to be true and it is. They chop extremely harshly at 1/16 and cause all sorts of noisy vibrations. If your printer sounds like a dot-matrix printer, check if you have these, as many Chinese sellers substitute HRs for proper A4988 so you can inadvertently end up with a set.

Avoid.

People running them at 1/128 say they are quiet though.

## Sanyo LV8729

* Constant current advertised as either 1.3A or 1.5A.
* Peak current 1.8A
* 1/128 stepping, 32-bit board required

Not much info out there on these. Supposed to be quiet.

* http://forums.reprap.org/read.php?160,724177

~~~
Formula: V = A * (0.5)	
Amps	Volts
1.5     0.75
1.3     0.65
1.2     0.60
1       0.50
0.8     0.40
	
Formula: A = V / (0.5)	
Volts	Amps
0.3     0.6
0.4     0.8
0.5     1
0.6     1.2
0.7     1.4
0.75    1.5
~~~

## Sanyo THB6128 (aka SD6128)

* 2.2A constant current
* Cooling unknown, needs at least big heatsinks
* 1/128 stepping, 32-bit board required
* `A = (V / 5) / R`

Sold in StepStick form factor as the [Panucatt SD6128](https://www.panucatt.com/product_p/sd6128.htm) or [RAPS128](http://reprap.org/wiki/RAPS128). Seems like a good idea.

## References

* http://reprap.org/wiki/Stepper_motor_driver
* http://reprap.org/wiki/StepStick
* http://reprap.org/wiki/DRV8825
* http://reprap.org/wiki/TMC2100
* http://reprap.org/wiki/A4988_vs_DRV8825_Chinese_Stepper_Driver_Boards
* https://www.facebook.com/groups/1711323699127948/permalink/1881251935468456/ - Tom Keidar TMC findings on D-Bot Facebook Group
* https://3dprinting.stackexchange.com/questions/623/real-life-stepper-speed
* https://electronics.stackexchange.com/questions/232674/transform-pulses-per-second-pps-to-rpm