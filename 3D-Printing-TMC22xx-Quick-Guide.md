# TMC22xx Quick Guide

![SD2224](http://cdn3.volusion.com/pgsmd.pvnjz/v/vspfiles/photos/SD2224-2.jpg?1511378486)

## Overview

This page provides a basic guide to replace existing common A4988 or DRV8825 stepper drivers with TMC2208 or TMC2224 stepper drivers.

These drivers take input signal like 1/16 stepping and divide it in hardware down to 1/256 for (almost) silent stepper motor operation.

### Why choose TMC22xx over the TMC21xx?

* TMC2100/TMC2130 have stealthChop1 mode which is not strong enough for many 3D printers.
* The alternative is spreadCycle mode which causes most motors to whine with 12V power. 24V power is needed to avoid this.
* TMC2208/TMC2224 have stealthChop2 mode which is stronger and designed for 3D printers.
* TMC2208/TMC2224 operate in standalone mode, there is no need to hook up SPI wires. There is a UART interface supported in Marlin 1.1.7 and RepRapFirmware if you wish to use it but there is no need. (TMC21xx also have standalone mode)
* TMC2224 have stealthChop2/spreadCycle selection using the MS3 jumper.

### What am I missing out on?

* TMC2130 have stallGuard2 which does sensorless homing (and skip step detection in Prusa Marlin) if you use the SPI interface.
* TMC2208/TMC2224 don't have stallGuard at all. (nor does TMC2100)

I don't care for the stallGuard feature. If you want sensorless homing then don't use 22xx drivers. Instead, get 24V power (hotend, fans, modified Arduino or other control board, heatbed, etc) and run 2130 in spreadCycle mode with SPI wires.

## Product Pages

* [Panucatt SD2224](http://www.panucatt.com/product_p/sd2224.htm) (USA & worldwide)
* [Filastruder TMC2208](https://www.filastruder.com/products/silentstepstick-tmc2208-stepper-motor-driver) (USA & worldwide)
* [Watterott TMC2208](http://www.watterott.com/en/SilentStepStick-TMC2208) (EU)
* [FYSETC TMC2208](https://www.aliexpress.com/item/1pc-TMC2208-Stepping-Motor-Mute-Driver-Stepstick-Power-Tube-Built-in-Driver-Current-1-4A-Peak/32822931280.html) (China & worldwide)
* [blkbox_me TMC2208](https://www.ebay.com/itm/322580660552) (China & worldwide)

## Installation

### Prepare Driver

* If you need to solder pins to your driver, the IC goes on the bottom (facing the control board) as picured above. The heatsink goes on top (away from the control board). The driver is cooled through the PCB, not through the IC cap.
* If your driver has PCB color (silkscreen aka solder mask) over the cooling pad then don't worry about it. This affects cooling ability less than 5%.
* Wipe top of driver with isopropyl alcohol.
* Add heatsink so it covers the through-pad (the little grid of circles showing where the IC is).
* You need heatsinks and a fan blowing hot air away from the heatsink. Do not use these drivers without both heatsink and fan. They will shut down if they reach 150C internally and they will reach that temperature if you don't cool them.

### Install Driver

* Power off printer.
* Unplug X and Y motors.
* Remove old driver (A4988 or DRV8825).
* Set jumpers as follows:

| Driver and Stepping | MS1 | MS2 | MS3 |
|---------------------|-----|-----|-----|
| 2208 1/16 stepping  |  On |  On | Off |
| 2224 1/16 stepping  | Off |  On | Off |
| 2224 1/32 stepping  |  On |  On | Off |

* Orient new driver (TMC22xx) the correct way. Use the `EN`/`DIR`/`GND` markings on the driver and the control board.
* Plug new driver in.
* Power on printer.

### Configure Driver

* Use your multimeter in DC voltage mode, set to 2V if you have that sort of meter.
* Use vref hole on board and power supply ground to measure vref.

~~~
|   Top    |
|          |
| (+)  .'. |
'--------^-'
         |
         '--- vref
~~~

* Turn trimpot to set vref.
* If the trimpot is on the underside of the board and you turn it through a hole in the PCB, use a ceramic non-conductive screwdriver, do not use a conductive metal screwdriver.
* Formula is:
    * TMC2208: `vref = current * 1.3`
    * TMC2224: `vref = current / 2`
* I use these but your printer might need different:
    * TMC2208: `0.915V = 0.7A`
    * TMC2224: `0.35V = 0.7A`
* Maximum values to use, but try to use as far under these as you can:
    * TMC2208: `1.82V = 1.4A`
    * TMC2224: `1.0V = 2.0A`
* Power off printer.
* Plug X and Y motors in.
* Power on printer.

### Configure Firmware

You need to invert the motor direction for the X and Y drivers.

* Marlin or Repetier Firmware
    * Open `Configuration.h`
    * Change `INVERT_X_DIR` and `INVERT_Y_DIR` to the opposite of what they are now.
    * Upload firmware to the control board.
* RepRapFirmware
    * Set `M569 P0 Sx` and `M569 P1 Sx` to the opposite of what they are now, either `S0` or `S1`.
* Smoothieware
    * Invert `alpha_dir_pin` and `beta_dir_pin` with a `!` like `0.1!`
    * [Feel sorry for yourself that you use Smoothieware](https://github.com/superjamie/lazyweb/wiki/3D-Printing-Smoothieware-Opinion).
    * [See if Marlin 32-bit will run on your board soon](https://github.com/MarlinFirmware/Marlin/issues/7076).

#### Test

* Slowly move X/Y to the middle of their travel by hand.
* Have your finger on the reset button just in case.
* Home printer with `G28`.
* If movement is unexpected, press reset and troubleshoot.
* If all works well, do some test moves and prints.

### Troubleshooting

* If you skip steps, then increase vref in small steps like 0.05V and try again
* To change vref:
    * Power off
    * Unplug XY motors
    * Power on
    * Set vref
    * Power off
    * Plug motors in
    * Power on
* Don't turn the vref trimpot with the motors plugged in. If you accidentally set the driver too high and the motors are activated, you risk damaging the motor, driver, and other electronics.

### Additional

* On TMC2224, the MS3 pin controls stealthChop2 (off) or spreadCycle (on).
* TMC2208 does not have spreadCycle selection without the UART interface.
* If you have a Mendel or cube (cartesian, corexy, Ultimaker, etc) then use these drivers on the X and Y axes only. Don't use these drivers on Z or E.
* The Z and E axes benefit from more torque and don't move fast enough to make noise with regular drivers anyway. I think you should run Z at 1/4 stepping [to get more holding torque](http://www.designfax.net/cms/dfx/opens/enews/20140819DFX/MICROMOsteppingChart2.jpg).
* If you have a delta then use these drivers on X and Y and Z.

### References

* Trinamic datasheet: https://www.trinamic.com/fileadmin/assets/Products/ICs_Documents/TMC220x_TMC222x_Datasheet.pdf
* Panucatt user guide: http://panucattdevices.freshdesk.com/support/solutions/articles/1000258055-sd2224-drivers
* Watterott SilentStepStick documentation: http://learn.watterott.com/silentstepstick/