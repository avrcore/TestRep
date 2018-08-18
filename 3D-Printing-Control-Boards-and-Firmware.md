# Control Boards:

Options I think are worth using

* Arduino Mega2560 and Premium RAMPS or RAMPS 1.4.2
* Good 8-bit all-in-ones like RAMBo, RUMBA, MKS BASE/GEN/GEN-L
* Arduino Due and RADDS, Get PanelDue to run RRF, or RADDS LCD to run Repetier/MK4duo
* DuefWifi and PanelDue but so expensive
* If using an 8-bit board, stick with 1/16 steppers due to low step rate, either A4988 or TMC2100
* If using a 32-bit board, use whatever you like, TMC still the best option

## 8-Bit Arduino

### Arduino Mega 2560 + RAMPS

http://reprap.org/wiki/RAMPS_1.4

* The most common controller and shield
* 12V power input
* Can be modded for 24V power input
* Cheap crappy RAMPS from China are likely to use under-rated MOSFETs and melt things around them, avoid cheap clones
* Use of under-rated polyfuses is not ideal
* Always use an external heatbed MOSFET

### Better RAMPS

* Staticboards Premium RAMPS: https://www.tindie.com/products/staticboards/ramps-14-sb-premium/
* GermanRepRap RAMPS 1.4.2: http://reprap.org/wiki/RAMPS_1.4#RAMPS_1.4.2

### MKS Boards

* Made by Chinese vendor Makerbase
* All-in-one 2560 + RAMPS boards
* Closed source designs
* 12V or 24V power input
* BASE - A4982 1/16 stepper drivers built in
* GEN - Pololu stepper driver sockets, FT232 USB serial, larger board size than BASE
* GEN-L - Pololu stepper driver sockets, CH340 USB serial, smaller board size than GEN, half the price of GEN
* Makerbase reverse the EXT1 and EXT2 LCD connectors so you need to shave the tabs off LCD cables and reverse them if not using MKS-branded 2004 (RRD Smart Controller) or 12864 (RRD Full Graphic Smart Controller)
* MKS have cases for their boards on their GitHub: https://github.com/makerbase-mks/MKS-STL

## 8-Bit Firmware Options

### Marlin

http://www.marlinfw.org/

* Most popular
* Easy well-commented configuration
* I like the menu better than Repetier

### Repetier Firmware

https://www.repetier.com/firmware/v092/

* Online configuration tool
* Configuration harder than Marlin
* Slightly faster stepping rate than Marlin (12kHz vs 10kHz)

### Teacup Firmware

* Super confusing to setup, you need to know the Arduino pin assignments for everything

## 32-bit (Smoothieboards)

### Firmware - Smoothieware

http://smoothieware.org/

* Pre-compiled firmware flashed to board, configuration all controlled by config file on microsd
* Config relatively well documented
* Software-settable motor current, no more trimpots
* Custom menu items easily programmed
* Web interface which looks like Pronterface
* Not as mature as Marlin or RepRapFirmware

Moving my bitch session to a separate page: [My opinion on Smoothieware](3D-Printing-Smoothieware-Opinion).

### Smoothieboard

http://smoothieware.org/smoothieboard

* Original Smoothieware board
* Allegro A5984 1/32 drivers onboard
* 120MHz and 200MHz CPU options
* Open source, the Smoothie community like and support these

### Azteeg X5 and X5 Mini

https://www.panucatt.com/azteeg_X5_GT_reprap_3d_printer_controller_p/ax5gt.htm

https://www.panucatt.com/azteeg_X5_mini_reprap_3d_printer_controller_p/ax5mini.htm

* Pololu stepper sockets
* 120MHz CPU
* Open source, Smoothie community like and support these
* Up to 30V power input
* Up to 48V stepper power input

### Re-ARM for RAMPS

https://www.panucatt.com/Re_ARM_for_RAMPS_p/ra1768.htm

* Same form factor as Mega2560, intended to use RAMPS shield
* 100MHz CPU
* 10-30V power input

Originally a Kickstarter project but now available for general sale.

Open source, Smoothie community like and support these.

### MKS SBASE

https://github.com/makerbase-mks/MKS-SBASE

* Closed source clone of Smoothieboard
* Arthur Wolf [fucking hates them](http://forums.reprap.org/read.php?13,499322) and will send you to MKS first for any questions
* Five built-in DRV8825 stepper drivers with a jumper to run 1/16 or 1/32
* 4 pins to run external drivers if desired, have seen someone running TMC2100
* Makerbase's GitHub has example config - some of the pinouts for motors, fan, temperature, panel and sdcard SPI channel are different to Smoothieboard
* I can't get it to recognise the sdcard is re-inserted without a reboot
* 100MHz CPU
* See 8-bit MKS notes for LCD cable and printable case details

~~~
TODO: Document SBASE pinouts
~~~

* v1.2 fixed the heatbed
* v1.3 fixed the Ethernet crystal and added a driver heatsink

### AZSMZ Mini

* Closed-source Chinese Smoothie clone
* Hated by the Smoothie community
* Build quality looks pretty poor
* 16-bin Pololu stepper sockets
* 100MHz CPU

## 32-bit (Arduino Due)

### Firmware - RepRapFirmware

http://reprap.org/wiki/RepRap_Firmware

* Currently mostly maintained by dc42 from Duet3D
* Promoted vigorously by him, though he seems friendly and helpful in all interactions
* Only supports PanelDue displays

### Firmware - Repetier Firmware

https://www.repetier.com/firmware/v092/

* As per Repetier section above
* Supports RADDS shield and Smart RAMPS shield
* Supports RADDS LCD2004 and a number of other Due-specific displays

### Firmware - MK4duo

https://github.com/MagoKimbra/MK4duo

* Port of Marlin to 32-bit Due platform
* Supports RADDS shield and Smart RAMPS shield

### Arduino Due and RADDS

http://www.reprap.me/arduinodue.html  
http://www.reprap.me/radds-v15.html  
http://www.reprap.me/radds-lcd-display.html  

* RAMPS-like shield designed for Arduino Due (3.3v TTL)
* Pololu stepper sockets
* Runs Repetier Firmware or RepRapFirmware
* Repetier: Buy a RADDS LCD (or make a cable to convert LCD2004/GLCD12864 maybe?)
* RepRapFirmware: Buy a PanelDue

### DuetWifi

https://www.duet3d.com/DuetWifi - http://reprap.org/wiki/DuetWifi

* Made by Duet3D, company of the RRF author dc42
* All-in-one Arduino Due and shield
* Up to 25V power input
* Five built-in TMC2660 stepper drivers (1/16 stepping, 16x interpolation, effective step 1/256)
* Only works with PanelDue controllers, no RADDS LCD or 2004/12864
* UK exchange rate makes the total package really expensive

### Arduino Due and RAMPS-FD

http://www.reprap.org/wiki/RAMPS-FD

* Forget RAMPS-FD, seriously it's not even an option
* RAMPS-like shield designed for use with Arduino Due (3.3v TTL) but also works on Mega 2560
* RAMPS-FD v1.x has safety concerns, Geeetech sold the unfinished pre-release design despite being asked not to
* bobc designed RAMPS-FD v2.2 but gave up the project, and all the Chinese sellers still sell v1
* Fixes for v1 boards are detailed at: http://forums.reprap.org/read.php?219,424146
* Fixes require SMD soldering so not recommended, hence avoid RAMPS-FD altogether