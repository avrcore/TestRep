# G-Code

G-Code is the "language" our printers speak. Every instruction which results in a change or action on the printer is G-Code.

If you are old, you may have used an Apple II in school. These computers had a language called Logo which had a "turtle" that could move around and draw a line as it moved. You can see from the picture below, if you tell the turtle `forward 100 pixels` and `right 90 degrees` enough times, you will draw a square:

![Logo Turtle](https://theantiroom.files.wordpress.com/2011/04/logo_turtle.jpg)

3D printers and G-Code work the same way. Heat the nozzle to a certain temperature, move the nozzle around left/right/up, move the filament extruder forward at the correct rate, and your plastic part is created. It's just a more complex version of the turtle.

There is a giant list of all supported G-Code on the RepRapWiki, however that's a huge list and is not very gripping reading. It is a very good reference though, so bookmark it:

* http://reprap.org/wiki/G_code

Not every G-Code operates the same on every firmware, some do/don't implement some codes, or there may be slight differences in operation. Each printer firmware may document specific details of its supported G-Code better than the RepRap Wiki:

* http://marlinfw.org/meta/gcode/
* https://duet3d.com/wiki/G-code
* http://smoothieware.org/supported-g-codes

But this are still large lists. Where's the crash course? Inspired by the rather clickbait-titled [The 10 Most Common G-Code Commands for 3D Printing](https://www.simplify3d.com/support/articles/3d-printing-gcode-tutorial/) by S3D, this article is a list of useful G-Code you can have in the start/stop G-Code of your slicer or may realistically find useful to learn.

## Useful G-Code

### `;` - Comment

The `;` character doesn't do anything, it's considered a comment. The comment can be either on a line on its own, or after valid G-Code. For example, both of these are the same and the firmware will only see `G28`:

~~~
; perform home sequence
G28
~~~

~~~
G28 ; perform home sequence
~~~

### `G28` - Perform Home Sequence

`G28` tells your printer to perform the home sequence. 3D printers work on an "open loop" control system, meaning they find the home position (usually minimum or maximum travel) with the Home command, and then keep track of movement from that point.

The printer knows it's at position 0 because the switch activated. If it moves 100 right then 50 right, it knows it's at position 150. If it moves 100 left, it knows it's at position 50. If we come along and moves the motor manually with our hand (or more likely in real life, the motor skips steps) then the printer doesn't know about that, it still thinks it's at position 50. That's why we home.

There do exist "closed loop" systems where the motor knows how much it's moved and can feed that information back to the control board and firmware. These are usually called "servo motors" or "encoders". These are quite expensive so not common to see on hobby-level 3D printers, but there are a few projects out there to convert our cheap stepper motors into feedback-capable servo motors.

Read more on RRW:

* http://reprap.org/wiki/Motor_control_loop

You can also specify specific axes to home. For example, to home only the X and Y axis: `G28 X Y`

### `G29` - Perform Auto-Level Sequence

TODO. for all y'all fancy inductive probe or BLTouch or IR probe users

### `G1` - Move

### `G90` and `G91` - Set Positioning Mode

~~~
; set absolute positioning
G90

; set relative positioning
G91
~~~

Most commonly used to move the Z axis out of the way after a print, eg:

~~~
G91     ; set relative positioning
G1 Z+10 ; raise Z 10mm
G90     ; set back to absolute positioning
~~~

For excitement, set your extruder to relative and start a print. Be AMAZED at the huge fat line of filament the printer can actually output when forced to! Not recommended.

### `G92` - Set Position

~~~
; set axis to position
G92
~~~

Most commonly used to do a prime or retract, eg:

~~~
G92 E0       ; zero extruder
G1 E2 F1800  ; prime 2mm
G92 E0       ; zero extruder
~~~

~~~
G92 E2       ; set extruder
G1 E-2 F1800 ; retract 2mm
G92 E0       ; zero extruder
~~~

### `M92` - Set axis steps per unit

~~~
M92 X80 Y80 Z400
M92 E420:420
~~~

Useful when testing different E steps settings. Syntax for multiple extruders shown above.

### `M104` and `M109` - Extruder Temperature Control

~~~
M104 S190  ; set extruder to 190C and don't wait
M109 S190  ; set extruder to 190C and wait for it
~~~

### `M140` and `M190` - Bed Temperature Control

~~~
M140 S60  ; set bed to 60C and don't wait
M190 S60  ; set bed to 60C and wait for it
~~~

### `M106` and `M107` - Fan Control

~~~
M106 S255  ; fan on full
M106 S127  ; fan at 50%

M106 S0    ; fan off
M107       ; fan off
~~~

### `M117` - Display Message

~~~
M117 PC LOAD LETTER
~~~

### `M119` - Get Endstop Status

~~~
M119
~~~

Example return when Z endstop is closed (i.e. carriage in middle of bed, but bed at zero):

~~~
Reporting endstop status
x_min: open
y_min: open
z_min: TRIGGERED
~~~

### `M201` to `M204` - Speed and Acceleration Control

~~~
M201 X1500 Y1500 Z100 E10000  ; set max print acceleration (mm/s/s)

M202 X1500 Y1500 Z100 E10000  ; set max travel acceleration (mm/s/s)

M203 X18000 Y18000 Z300 E2700 ; set max feedrate (mm/min)

M204 P1500 T1500              ; set default acceleration for Print and Travel (mm/s/s)
~~~

### `M302` - Allow Cold Extrude

~~~
M302 S0
~~~

Useful when tuning E steps and you're not actually feeding filament thru the nozzle.

### `M303` - PID Autotune

See calibration page for firmware specifics:

https://github.com/superjamie/lazyweb/wiki/3D-Printing-Calibration#firmware-temperature

### `M420` - Bed Level State

http://marlinfw.org/docs/gcode/M420.html

### `M500` to `M502` - EEPROM Commands

~~~
M500  ; store current settings to EEPROM
M501  ; read settings from EEPROM
M502  ; revert to "factory settings" from firmware
~~~

I don't use EEPROM so I don't play with these much.

### `M115` and `M503` - Firmware Info and Report Settings

Used to get the current configuration in your firmware. Useful if you're upgrading a firmware on an old printer and don't have the original config file.

### `M600` - Filament Change

Lets you change filament. You can pass XYZ parameters to move the nozzle to a particular spot.

~~~
M600 X0 Y20 Z5 ; move to X0 Y20 and lift 5, pause and wait for filament change
~~~

### `M665` - Delta Calibration

This lets you enter in delta parameters like rod/radius/tower offsets. You are supposed to use this with `G33` to do the delta auto calibration. I would also like to try this manually with the [least squares calculator](http://www.escher3d.com/pages/wizards/wizarddelta.php).

## Useful Start G-Code

~~~
G28
G29 ; if you have a Z probe
~~~

If you have a simple printer or you prefer manual bed levelling, `G28` is all you need in your start G-Code. In fact, if you're willing to home before you start printing, you don't really even need that, but it's a good idea to keep it in. It doesn't take long.

If you have a bed probe, you could also add the probe sequence `G29`, however if you can't be bothered waiting for your printer to manually touch 9 or 16 or 25 points of the bed before each and every print, maybe it's easier if you just do the bed probe from the LCD screen or Pronterface or OctoPrint or however you do it. You can do that each power on, or you can save it to EEPROM. Unless your printer has a very unstable bed, a manual probe should last at least a few prints before needing to be done again. A good printer should last at least a month before needing a new probe.

~~~
G92 E0       ; zero extruder
G1 E2 F1800  ; prime 2mm
G92 E0       ; zero extruder
~~~

~~~
G0 X0 Y20 F9000   ; go to front corner
G0 Z0.25          ; drop the bed
G92 E0            ; zero extruder
G1 X60 E12.5 F600 ; draw prime line
G92 E0            ; zero extruder
~~~

## Useful End G-Code

~~~
M104 S0       ; hotend off
M140 S0       ; bed off
M107          ; fan off
~~~

~~~
G92 E3        ; set extruder
G1 E-3 F1800  ; retract
G92 E0        ; zero extruder
~~~

~~~
G1 X0 F3600   ; move X carriage out of the way
G1 Y200 F3600 ; present print for mendels
~~~

~~~
G1 X100 Y180  ; get out the way for cube frames
~~~

~~~
G28 ; get out the way for deltas, homes carriages to the top
~~~

~~~
M84           ; motors off
~~~

### References

* http://reprap.org/wiki/G_code
* https://en.wikipedia.org/wiki/G-code
* https://www.simplify3d.com/support/articles/3d-printing-gcode-tutorial/
