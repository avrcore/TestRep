# How To Calibrate Your 3D Printer Properly

### General Links

* Tom Sanladerer's Guides - https://www.youtube.com/playlist?list=PLDJMid0lOOYnRCAdbFfzECor3EbqF8euw
* Maker's Muse 3D Printing 101 - https://www.youtube.com/playlist?list=PLTCCNNvHC8PDR_jQy609toqq8EAfhiOOL
* RepRap Wiki - http://reprap.org/wiki/

### Stepper Driver Current

* When to Calibrate: During initial setup, when changing motor, stepper driver, or control board

**Don't calibrate stepper driver current with the motors plugged in!** If you set the current to the wrong value, you risk frying the driver or motor or both. Don't unplug the motors with the printer powered on either.

Power off your printer, unplug the motors, turn on the printer, and tune the drivers.

When you're finished, power off the printer, plug the motors back in, and turn on the printer.

Look up the formula for your stepper drivers. Some common ones:

* A4988: `V = A * (8 * R)` which is also `A = V / (8 * R)`
* DRV8825: `V = A * (5 * R)` which is also `A = V / (5 * R)`
* TMC2100: `V = A / (1.77 / 2.5)` which is also `A = V * (1.77 / 2.5)`

Basic tables for each driver are given on my [Stepper Motors and Drivers page](https://github.com/superjamie/lazyweb/wiki/3D-Printing-Stepper-Motors-and-Drivers).

Set your multimeter to DC voltage (2V). Measure voltage between DC negative of the power supply, and positive to the adjustment trimpot on the driver board. A good tip is to use multimeter clamps, so you clamp one to the negative on the power supply, and positive to your screwdriver. Then the screwdriver can measure voltage at the trimpot as you adjust it.

Look up the current of your motors, set to that current or just under it. You may be able to get away with 80% less current depending on setup. If you have very powerful motors like 70Ncm you can set the motors to way less torque to keep them cool.

Current and torque do not scale linearly, reducing current by 20% can reduce torque by 40% or more. If you find you're skipping motor steps *and everything else is mechanically good* then you could try increase motor current to avoid the skipped steps. If you have a mechanical problem, don't just increase stepper driver current to power through it, solve the mechanical problem properly.

I find the extruder needs more torque than the motion axes. This makes more sense as it's easier to move a carriage sliding over smooth and lubricated rods/rails/wheels than it is to force millimeters of filament through a very tiny nozzle hole. A geared extruder can help avoid the need for such a powerful filament motor.

References:

* https://github.com/superjamie/lazyweb/wiki/3D-Printing-Stepper-Motors-and-Drivers
* http://reprap.org/wiki/A4988_vs_DRV8825_Chinese_Stepper_Driver_Boards
* http://reprap.org/wiki/Stepper_motor
* [Tom Sanladerer - 3D printing guides - How steppers work and how to adjust their drivers](https://www.youtube.com/watch?v=bItYRMLGoVc)

### XYZ Steps

* When to Calibrate: During initial setup, when changing axis gear/motor/parts

The correct figures for X, Y, and Z axis are based on physical properties of your printer. You don't "tune" or adjust these unless you change parts.

The correct way to determine these values is to find your motor's step angle (usually 1.8° or 0.9°), the microstepping of your drivers (usually 1/16 or 1/32), your belt pitch (2mm for GT2), and either the number of teeth on your pulleys (usually 16, 20, or 36) or the pitch of your leadscrew (usually 2mm or 8mm).

The formulas are given on Triffid Hunder's calibration guide, but the Prusa Calculator makes this very easy, so just use that.

* http://www.prusaprinters.org/calculator/
* http://www.reprap.org/wiki/Triffid_Hunter's_Calibration_Guide

These are your "Steps Per mm" values to be entered into the firmware, eg:

~~~
1.8 deg motor, 1/16 microstep, GT2 belt, 20T pulley = 80 steps/mm
1.8 deg motor, 1/16 microstep, 2mm leadscrew = 1600 steps/mm

#define DEFAULT_AXIS_STEPS_PER_UNIT   { 80, 80, 1600, E }
~~~

References:

* [Tom Sanladerer - 3D printing guides: Calibration and why you might be doing it wrong](https://www.youtube.com/watch?v=Mbn1ckR86Z8)

### Extruder Steps

* When to Calibrate: During initial setup, when changing extruder motor/parts, when changing filament material

You can do a rough calculation for extruder steps based on the diameter of the gear, or just set to `100` and test from there.

If you're using a geared extruder then multiply that start value by the gear ratio.

Mark the filament at the entrance to the extruder, tell the printer to extrude 100mm, then mark where the filament stops.

Remove the filament, measure between the two marks. Now adjust the E steps per mm by the following calculation:

~~~
new_e_steps = old_e_steps * (100 / distance_actually_moved)
~~~

Now do it again. Mark, extrude, mark, measure.

When you tell the printer to extrude 100mm and the measurement between the marks is actually 100mm, you've calibrated correctly.

This goes as the last measurement:

~~~
#define DEFAULT_AXIS_STEPS_PER_UNIT   { X, Y, X, 100 }
~~~

Different filament (eg: PLA vs PETG) will likely be gripped differently by the gear as the teeth "dig in" more or less, which modifies the effective diameter of the pulley and hence modifies the ideal steps. You can set the value in the start G-code for a material like `M92 E101`

References:

* [Tom Sanladerer - 3D printing guides - Calibrating your extruder](https://www.youtube.com/watch?v=YUPfBJz3I6Y)
* http://zennmaster.com/makingstuff/reprap-101-calibrating-your-extruder-part-1-e-steps
* http://reprap.org/wiki/G_code#M92:_Set_axis_steps_per_unit

### Speeds

* When to Calibrate: During initial setup, when changing mechanical/weight properties of the printer.

~~~
TODO - make nicer
~~~

tl;dr - don't try and print too fast

start with XY max 250, XY accel 1000, XY jerk 4, print speed 40mm/sec

work up from there

accel 1500 and jerk 10 is very good! jerk should be 10% of print speed or less

different materials require different print speeds, so tune speed with PLA (or whatever you print with most often) and just tell the slicer to print slower for other materials as required

* [Vibration ripple shadow ghosting test by orcinus](https://www.thingiverse.com/thing:277394)
* [Tom Sanladerer - 3D printing guides - Tuning speeds](https://www.youtube.com/watch?v=7HsIZuj9vOs)
* [Tech2C - This is Y](https://www.youtube.com/watch?v=AKTvykTPjQw)

### Firmware Temperature

* When to Calibrate: When changing thermistor, when changing printer location, when changing environmental conditions (eg: cold winter, hot summer)

Set up the right Thermistor in the firmware.

Do PID Autotune of your nozzle and bed from cold. Tune under regular running conditions, like if you use a silicone sock and a fan on the hotend, have both of those on. If you use a glass or PEI sheet, have that clamped to the bed too.

`M303 E0 S200 C8` for nozzle, and `M303 E-1 S60 C8` for bed.

Different firmware uses different IDs for nozzle and bed:

* Marlin: `E0` (hotend) and `E-1` (bed)
* Repetier: `P0` (hotend) and `P3` (bed)
* RepRapFirmware: `H1` (hotend) and `H0` (bed)
* Smoothieware: `E0` (hotend) and `E1` (bed)

If you find your temperatures are still not a flat line after PID tuning, you can modify by hand. The RigidTalk Wiki provides good instructions for understanding PID calculations. The RepRapWiki gives some good multipliers for changing the formulas around a bit.

References:

* [Tom Sanladerer - 3D printing guides - Using Marlin's PID autotune](https://www.youtube.com/watch?v=APzJfYAgFkQ)
* [RigidWiki - PID tuning](http://rigidtalk.com/wiki/index.php?title=PID_tuning)
* [RepRapWiki - PID Tuning](http://reprap.org/wiki/PID_Tuning)

### Filament Temperature

* When to Calibrate: When using a different brand, color, or material of filament for the first time

~~~
TODO - make nicer
~~~

print a temperature tower, find one on Thingiverse. i like this one: [heat tower 195C to 225C by MBTech](https://www.thingiverse.com/thing:2213410)

change temperature as you print it, corresponding with the design of the tower:
* in Slic3r use Shape Modifiers, or write some [conditional gcode](https://github.com/alexrj/Slic3r-Manual/blob/master/src/advanced/conditional-gcode.md), or use a [postprocessing script](https://www.thingiverse.com/thing:1579403)
* in Cura use the TweakAtZ plugin in Extensions, Post Processing, Modify G-Code ([video](https://www.youtube.com/watch?v=Wkegh9WRaxs))
* in S3D use multiple processes covering a height range and change the temperature in each process

once you have the tower printed, pick which temperature looks best or gives the strongest layer adhesion or the best detail or the best bridging or whatever aspect you care about. often the strongest parts will not be the best looking parts, everything is a tradeoff

for pla use range 185c to 235c

for petg use range 205c to 265c

* https://www.thingiverse.com/search?q=temperature+tower
* https://www.thingiverse.com/search?q=temperature+calibration

### Filament Diameter

* When to Calibrate: For each filament spool

Being sure to keep tension on the roll so it doesn't unwind in a big mess, pull a couple of meters out of the filament roll and measure the diameter along various points and around various angles.

Take note of all these measurements and find the average, this is the value you put into the slicer for filament width.

If the filament varies by more than 0.02mm either way (eg: the average is 1.71mm but varies more than 1.69 and 1.73 along or around), or is larger than 1.76mm anywhere, then return the spool as faulty.

### Extrusion Flow Rate

* When to Calibrate: For each filament spool, for each extrusion width, when changing nozzle

In your slicer, set your extrusion width to 120% of nozzle diameter. If you have a 0.4mm nozzle, then set to 0.48mm.

Use [Desi Quintans' Flowrate calculator](http://www.desiquintans.com/flowrate) to print a two-wall shape and dial in the extrusion multiplier until you are reliably getting two walls at 2x extrusion width.

Once you've found the exact number, print Desi's shape with whatever settings you care about most. I print at 4 walls, 8 top/bottom layers, and 50% infill. I also resize the shape to 40x40x8 to make a nice-feeling filament swatch. Vary the flow rate until the top layer comes out perfectly flat and not overstuffed.

That is the ideal flow rate.

Depending on the model and filament and other factors, it may still be helpful to add 1% or 2% to that number, just to fill in tiny gaps and ensure fat overlapping extrusions. There are probably situations where it's better to remove 1% or 2%. Tune as needed based on your print results.

* http://www.desiquintans.com/flowrate
* https://www.youtube.com/watch?v=7ls3B97IHyg

## Stringing, Bridging, Retraction

* When to Calibrate: When using a different brand, color, or material of filament for the first time

~~~
TODO - make nicer
~~~

print some torture tests

* [Ultrafast and economical stringing test by s3sebastian](https://www.thingiverse.com/thing:2219103)
* https://www.thingiverse.com/search?q=retraction+test
* https://www.thingiverse.com/superjamie/collections/3dp-test-calib-torture

futz with the settings like print speed, retraction, coast, wipe, cooling until you get good print quality.

be careful using large retraction numbers, you risk pulling hot molten filament up past the transition zone into the coldend, where it will rapidly solidify and cause a clog. 2mm retraction is usually enough for a good filament path with PLA. stringy filaments like PETG can use more, maybe 3mm or 4mm. the maximum retraction you should use is 5mm. direct drive generally needs less retraction than Bowden. using [Capricorn XS tube](http://www.captubes.com/) can help reduce the amount of retraction required.

some filament softens more than others, so if the coldend is not well cooled then the material can compress and get wider and cause a clog that way. it's not just cheap filaments which do this, even brand name stuff has different ideal temperatures.

if you want to avoid the nozzle dragging across top surfaces and leaving scars, use Z hop on retraction.

## Print Quality Troubleshooting

You don't need to remember these all off the top of your head. Skim the pictures and look for poor aspects of printing which you should watch for in your prints.

If you ever have a print problem, go through these guides and try the suggestions within:

* http://support.3dverkstan.se/article/23-a-visual-ultimaker-troubleshooting-guide
* https://www.simplify3d.com/support/print-quality-troubleshooting/
* http://deltaprintr.com/troubleshooting/
* http://builda3dprinter.eu/information/troubleshooting-guide/
* https://all3dp.com/1/common-3d-printing-problems-troubleshooting-3d-printer-issues/
* http://reprap.org/wiki/Print_Troubleshooting_Pictorial_Guide

## Maintenance / Service

Every Print

* Inspect axis belts/rods/rails/slots for any random plastic pieces and remove
* Remove any debris off print bed
* If using a dry build surface, clean the surface by wiping with 99% isopropyl alcohol
* If using adhesive (glue/hairspray), ensure adhesive is level and there's enough on there
* Clean boogers off nozzle as the print starts

Weekly or Fortnightly

* Level bed manually (even if you're using auto levelling)

Monthly or when a roll of filament runs out

* Check belt tension: https://www.thingiverse.com/thing:2230598
* Clean rods
* Lubricate rods/screws/bearings
* Blow dust out of fans
* Blow dust out of filament gear
* Check/clean/replace [filament dust filter](https://www.thingiverse.com/thing:497764)
* Check nozzle bore to ensure nozzle isn't wearing out/down too much

Quarterly

* Check set screws on motor pulleys, couplers, extruder gear
* Check screws holding in all frame, motors, rods, and other mounted parts
* Inspect Bowden tube and trim/replace if required
* Clean nozzle using [cold pull](https://printrbot.zendesk.com/hc/en-us/articles/202100554-Unclogging-the-Hotend-using-the-Cold-Pull-method) aka [atomic method](https://ultimaker.com/en/resources/19510-how-to-apply-atomic-method)
* Ensure nozzle is fastened against heatbreak while hot
* Update firmware
* Update host software

References

* https://www.lulzbot.com/maintaining-your-3d-printer
* https://www.youtube.com/watch?v=SdXT_SR8dC8
* http://support.3dverkstan.se/article/42-basic-maintenance
* https://pinshape.com/blog/3d-printer-maintenance/
* https://ultimaker.com/en/resources/21925-maintaining-your-printer
* http://zx3d.com.au/maintenance/
* https://www.makerbot.com/media-center/2011/01/27/how-to-get-better-results-from-your-3d-printer-%E2%80%93-part-iv
