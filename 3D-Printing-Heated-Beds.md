## Heatbeds

A heatbed is a long copper track which provides a resistance to a power supply and converts the supply energy into heat energy. It's just a big resistor. By measuring the temperature around the bed with a thermistor, the control board can turn the bed on and off to maintain a relatively steady temperature.

### Designs

MK1 and MK2 heatbeds are just copper tracks printed on a PCB. They have the all the physical strength and stability of a PCB (i.e. none) so warping is common and people usually clamp a borosilicate glass (Pyrex) surface to them to compensate for this. It's probably a good idea to stick the glass to the PCB with 3M thermal transfer tape. The thermistor should measure through the bed and underneath the glass.

MK3 beds are aluminium plate with an isolated copper track on the back. As far as I understand, MK3 uses the same circuit as MK2B beds. The aluminium is supposed to provide a physically flatter and thermally more uniform heating surface than a PCB. Often cheap Chinese MK3 beds are bent, a 0.2mm dip/rise is common, up to 2mm is not unheard of.

MK42 heatbed is a PCB circuit with more even heating and is designed to work with the probe and firmware of the genuine Prusa i3. I believe this is just metal inserts in the 9 homing positions, plus a 4mm inductive sensor, plus Marlin configured to perform auto bed leveling on the metal homing spots.

Another popular option is to run an aluminium sheet and separate heater element. Cast aluminium is more flat and maintains a more stable shape under heat than rolled aluminium which is not flat and moves around a lot. A specific type of cast sheet called MIC6 has tolerances down to 0.006mm flatness which is suitable for 3D printing onto.

Some people run mains-powered heating elements switched by a Solid State Relay but any use of mains power by hobbyists on something you regularly touch with your hands seems unwise and unsafe.

It's a good idea to insulate the bottom of any heatbed for more efficient operation. Cork and fire/rescue blanket sheet are commonly used.

### Inductive Sensors

Inductive sensors are rated to detect thick steel or iron at their measurement distance. They detect aluminium at a larger distance, and do not detect hot things as easily as they detect cold things.

A 4mm inductive sensor generally detects a 3mm thick aluminium sheet at 0.8 to 1.0mm.

The result of this is that to detect a 3mm thick aluminium bed through a 3-4mm thick glass sheet, you need to use an 8mm inductive sensor and hope it doesn't crash into the glass. Good luck.

As mentioned above, the Prusa MK42 bed uses metal inserts in homing positions to work around this. The same effect can be achieved with steel washers/discs stuck to the bottom of the bed.

### Leveling

Most boards level at 4 corners, but this is not ideal as 3 positions define a plane, and leveling 4 corners results in each corner affecting the others so you need to go around a couple of times to correct inaccuracies as you level the bed.

Ideally beds should level in two corners plus the middle of the opposite side, which genuine MK3 do:

~~~
o---------o
|         |
|         |
|         |
'----o----'

o = homing screws
~~~

Clone MK3 are a metal copy of the MK2 and usually have four level screw holes, one in each corner.

### Power

You can run the bed off either 12V or 24V. 12V seems fine for beds up to 200x300mm. For larger beds, 24V is required to hit temperatures like 110C in under an hour.

A 12V circuit should have about 0.8 to 1.3 Ohm resistance. A 24V circuit should have 3 to 5 Ohm resistance. These vary depending on design and size of the bed.

A 24V bed can be run off a separate 24V power supply and use another 12V power supply to run the electronics, or you can have one 24V power supply and use a buck converter to step down some to 12V to run the electronics. RAMPS requires 12V for the logic circuit but can some electronics (Smoothieboard, MKS BASE/GEN/SBASE) will handle 24V natively.

You can run 24V through the RAMPS heatbed circuit if you change the MFR1100 polyfuse to a 24V fuse and remove the LED. It's easier and safer to just run the board of a SSR or MOSFET.

## References

* http://reprap.org/wiki/PCB_Heatbed
* http://rigidtalk.com/wiki/index.php?title=Modifying_a_RAMPS_1.4_board