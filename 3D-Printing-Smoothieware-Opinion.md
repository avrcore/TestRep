# My Opinion on Smoothieware

## tl;dr

Don't buy a Smoothieware-compatible board.

If you want 32-bit right now, buy a [DuetWifi](http://duet3d.com/) if you can afford it, or buy an Arduino Due and [RADDS](http://www.reprap.me/radds-v15.html) if you can't.

You could also wait until later in 2018 for Marlin 2 to release, which should support the [Re-ARM](https://www.panucatt.com/Re_ARM_for_RAMPS_p/ra1768.htm). Get a good [Premium RAMPS](https://www.tindie.com/products/staticboards/ramps-14-sb-premium/) to go with it, don't use $7 Chinese rubbish RAMPS.

## Details

I don't have a high opinion of Smoothieware for the following reasons:

### No Soft Endstops

Soft endstops means when your carriage is at 0 and you tell it to move to -10, it doesn't move there because it will crash into the frame. It also means when the carriage on your 200mm axis is at 200 and you tell it to move to 210, it doesn't move there because it will crash into the frame.

Marlin has this. Repetier has this. RepRapFirmware has this. Every other printer firmware has this. Smoothieware does not have this. I hope you religiously remember the direction your LCD trimpot has to go in every situation, or manually level the bed 10mm inwards just to be safe.

There is the functionality to halt when an endstop is triggered, so if your endstops are at 0, your printer shuts down when you home axes with G28 before every print and you can never start a print. So useful.

There is no developer interest in implementing a basic "ignore" on movements past travel limits, because some users will complain about this. The creators would rather spend the time on a complex full-featured multi-option implementation to address all use cases for FFF, CNC, and laser cutter users.

In the meantime, don't accidentally move past your frame limit.

* Reference: [New to Smoothieware, are travel limits supported?](http://forum.smoothieware.org/forum/t-1478417/new-to-smoothieware-are-travel-limits-supported)
* Update: https://github.com/Smoothieware/Smoothieware/pull/1149

### Web Interface != sdcard

Smoothie comes with a basic web interface which looks like Pronterface, which is a pretty nice feature. You can also upload files to this and then print them.

However, only the internal sdcard is supported. I don't want my prints on the same sdcard as my configuration file. sdcard storage is already unreliable enough without having to worry about wearing out or killing the sdcard where the firmware updates and configuration file that run the printer are living.

I would much rather be able to use the external sdcard in the LCD screen.

Development have no interest in supporting upload to external sdcard because it is "unreliable". Most 3D printers in the world print successfully from sdcard every print but that apparently doesn't work. Marlin has a configuration option for CRC checking of sdcard communication but that apparently is not reliable either. Okay.

* Reference: [Feature Request: Web upload to /ext/ and play from /ext/](http://forum.smoothieware.org/forum/t-2223914/feature-request:web-upload-to-ext-and-play-from-ext)

## sdcard upload speed

Uploading to the sdcard via the web interface is slow. Despite various braggings saying how good Smoothie's sdcard interface is, in practical use it takes several minutes to upload a few MiB of g-code via the web interface, which is annoying to useless.

## Z probe != endstop

There is no support for using a Z probe as an endstop. Every other firmware supports this.

The suggested solution is to home Z to max and use a Z probe to find the bed. I have better things to do than sit and watch my printer home through 400mm of travel every time I turn it on or want to print something.

## BLTouch = ???

I have read every configuration and forum thread about it, I still have no idea how to use the BLTouch with Smoothieware. I'm not an idiot. It shouldn't be this hard, or if it is then the documentation should be much better.

## Limited Display Selection

I hope you like the Full Graphic LCD (GLCD12864) or US$75 Viki2 display because those are the only supported displays.

There is a way to make a "universal adapter" with another Arduino, which would theoretically allow you to use an older display like an LCD2004, however you'd need pretty good electronics skills to do so. The documentation around that should be much better.

* Reference: http://smoothieware.org/panel

## Additional Complaints

I'm not the only one:

* https://www.reddit.com/r/voroncorexy/comments/6e1jj1/looking_for_recommendations_on_32_bit_boards/
* https://www.facebook.com/groups/cncbuilddesign/permalink/1894095017270460/

You can also search for opinions on the [3D Printer Builder/Designer](https://www.facebook.com/groups/cncbuilddesign/) Facebook Group where there are plenty of other people who've been disappointed by Smoothieware and describe it as "under-developed" and "inferior".

## MKS SBASE

Chinese company Makerbase sell their own closed-source version of the Smoothieboard called the SBASE.

Because the board is closed source, the developer absolutely hates MKS and intermittently goes on forum rants and tells anyone with an SBASE to ask MKS before posting on the forums.

Makerbase are not blameless here. They have used the RepRap community to solve problems with their boards. The boards had a problem with the heatbed MOSFET which was fixed in v1.2. The boards had a problem with the Ethernet crystal which was fixed in v1.3.

The reason they give for not opening the designs is that other Chinese vendors will copy their design and flood the market with cheap clones. That's ironic considering that's exactly what they're doing, but there are already poorly-made clones of their boards floating around, either counterfeit MKS boards or third-party "KeYes" boards.

Apparently every other Smoothie-compatible board (Azteeg X5, Re-ARM, Cohesion3D) apart from being open source, gives Arthur Wolf money to support the firmware development. I'll let you read between the lines as you desire.

* Reference: [New MKS SBASE Smoothieware-compatible board from China](http://forums.reprap.org/read.php?13,499322)

## SBASE sdcard detection

With my SBASE and Full Graphic LCD, I need to either power on with an sdcard in, or insert an sdcard then reboot the board for the card to be detected. If I want to swap or remove and replace the sdcard, I need to reboot, I cannot swap the sdcard live. MKS support tell me to just reboot the board and there's no way to fix it. This is probably a limitation of the SPI channel the SBASE uses for sdcards rather than of Smoothieware, however it's still annoying.

## Summary

Smoothieware seems good on paper, but my experience has been poor. After using Smoothie for a while I went back to Marlin on Arduino. I wish I'd never bought this board and should probably sell it while it's still worth something.

## The Alternative

RepRapFirmware on the Due-based hardware seems much better. Everyone who buys one of these is very happy with it and raves about it like a religious experience, I have never seen an unhappy Duet owner. The developer dc42 has a much better attitude and is helpful, positive, and interested in community requests. Duet3D's DuetWifi is the best implementation, though the UK exchange rate and the fact that it only works with the expensive PanelDue makes these prohibitively expensive for me in Australia, over AUD$400. Filastruder in USA stock them too.

RepRapMe in Denmark sell a RADDS shield and Arduino Due which come to about AUD$180. Even with genuine TMC2100 steppers ($20 each) it's still significantly cheaper than a DuetWifi. You can run RepRapFirmware or Repetier Firmware, though I have never been able to get Repetier to work for me. There is also a firmware called MK4Duo which is a fork of older Marlin, it works okay. Note that RepRapFirmware only supports PanelDue displays and they are not cheap!

RADDS can be used with an LCD2004 or GLCD 12864 however those are 5v screens and the Arduino Due is 3.3v, plus the plug is not compatible, so you need to make a cable and futz around with voltages. People have done this but it's a bit of fiddly work. RepRapMe sell a RADDS LCD which is already 3.3v and has the right plug, I'd rather just buy the pre-made working LCD.

Personally I have bought a Re-ARM from Panucatt and a Premium RAMPS from StaticBoards and I'm waiting for Marlin 2 to be released. I have been very happy with Marlin and keep coming back to it so that's the path I have chosen.

So, if you want 32-bit:

* Best option now: Due + RADDS + PanelDue and run RepRapFirmware
* Best option soon: Re-ARM + Premium RAMPS and run Marlin 2.0 when it comes out
* Still probably good: Due + RADDS + RADDS-LCD and run RepRapFirmware or MK4duo