## Config File

The DOSBox config file is at `~/.dosbox/dosbox-SVN.conf` and the mapper file is `mapper-SVN.map`

The `~/.dosbox/` directory is a symlink to `/opt/retropie/configs/pc/` directory

## C Drive

I put my C drive in `~/RetroPie/roms/DOS` and then edit the launcher `RetroPie/roms/pc/+Start\ DOSBox.sh` to have the line:

~~~
    params=(-c "@MOUNT C /home/pi/RetroPie/roms/DOS" -c "@C:")
~~~

This keeps the PC menu in RetroPie tidy.

## Per-game Config

~~~
    params=(-conf "~/.dosbox/game.conf" -c "@MOUNT C /home/pi/RetroPie/roms/DOS" -c "@C:")
~~~

To have a custom key map, in that config file, set:

~~~
mapperfile=mapper-game.map
~~~

## Keymapper

Good ideas:

* mod3 = Select
* Shutdown = Start mod3
* Cycles Down = L2 mod3
* Cycles Up = R2 mod3

Logitech F710:

~~~
mod_3 "stick_0 button 8" 
hand_shutdown "key 290 mod1" "stick_0 button 9 mod3" 
hand_cycledown "stick_0 button 6 mod3" "key 292 mod1" 
hand_cycleup "key 293 mod1" "stick_0 button 7 mod3" 
~~~

Should probably make a convention like:

* L2 = y
* R2 = n
* Select = Esc
* Start = Enter

## Aspect Ratio

To get games to display in the correct aspect ratio:

~~~
windowresolution=1024x768
output=overlay
aspect=true
~~~

Setting `output=opengl` does not harm performance and applies bilinear smoothing to make things look nicer, though it also renders a mouse cursor in the top left corner which is rather annoying.

Setting `windowresolution=1280x960` looks nicer but performance suffers for more complex games. Maybe this is OK to use with less demanding games.

Setting `scaler=normal2x` doesn't seem to make any performance difference, unsure if it makes a graphical difference either. Even on weaker games, `scaler=normal3x` affects performance.

## Performance

I always configure my DOSBox with a fixed cycles count like:

~~~
core=auto
cputype=auto
cycles=20000
cycleup=1000
cycledown=1000
~~~

Doom maxes out the CPU at about 21000 cycles, which gives about 21fps, about the performance of a 486-66 according to the Doom Benchmark - https://www.complang.tuwien.ac.at/misc/doombench.html

Performance depends on the game, OMF2097 performs well on its "Pentium" setting with only 12500 cycles. IIRC from using real hardware, that setting actually did require a Pentium or faster.

Probably worth playing with `cycles=max` with some games. In Doom this dropped 4fps. In OMF2097 it makes the game so fast as to be unplayable.

## Compiling from Source

The binaries RetroPie provides are compiled with the Pi 2 compiler flags, but recompiling on a Pi 3 uses different flags, which appears to give a boost in performance. I haven't benchmarked this.

## Mouse control with analog stick (PS3)

Download and compile joymapper: https://sourceforge.net/projects/linuxjoymap/

Find device ID:

~~~
cat /proc/bus/input/devices

I: Bus=0003 Vendor=054c Product=0268 Version=0111
N: Name="Sony PLAYSTATION(R)3 Controller"
P: Phys=usb-bcm2708_usb-1.3.2/input0
S: Sysfs=/devices/platform/bcm2708_usb/usb1/1-1/1-1.3/1-1.3.2/1-1.3.2:1.0/0003:054C:0268.0001/input/input2
U: Uniq=
H: Handlers=js0 event2 
B: PROP=0
B: EV=1b
B: KEY=7 0 0 0 0 0 0 0 0 0 0 0 0 ffff 0 0 0 0 0 0 0 0 0
B: ABS=7fffff00 27
B: MSC=10
~~~

Example PS3 mouse mapping for DOSBox:

~~~
# General Mouse Mapping for DOSBox

# Map Mouse
axis vendor=0x054c product=0x0268 src=0 target=mouse axis=0
axis vendor=0x054c product=0x0268 src=1 target=mouse axis=1
button vendor=0x054c product=0x0268 src=14 target=mouse button=0
button vendor=0x054c product=0x0268 src=15 target=mouse button=1

# Navigation
button vendor=0x054c product=0x0268 src=0 target=kbd button="esc"
button vendor=0x054c product=0x0268 src=3 target=kbd button="enter"
button vendor=0x054c product=0x0268 src=12 target=kbd button="space"

button vendor=0x054c product=0x0268 src=4 target=kbd button="up"
button vendor=0x054c product=0x0268 src=5 target=kbd button="right"
button vendor=0x054c product=0x0268 src=6 target=kbd button="down"
button vendor=0x054c product=0x0268 src=7 target=kbd button="left"
~~~

To use the mouse, DOSBox needs to capture it. This can be done by pressing CTRL+F10 or left-clicking.

Example launcher for Albion:

~~~
#!/bin/bash
sudo /home/pi/joymap/loadmap /home/pi/RetroPie/roms/pc/dosbox-map/mouse.map &
/opt/retropie/emulators/dosbox/bin/dosbox -c "mount c /home/pi/RetroPie/roms/pc" -c "c:" -c "cd ALBION" -c "LAUNCH.EXE" -c "exit" 
sudo killall loadmap
sleep 1
~~~

* https://retropie.org.uk/forum/topic/2032/dosbox-mapping-mouse-to-ps3-analog-stick/
* http://blog.petrockblock.com/forums/topic/mapping-a-game-controller-in-kodi/#post-106586

## Mouse control with analog Stick (Logitech F710)

This isn't working, the mouse tries to stay in one corner and is way too fast. It also seems you need to map *every* key you want to use, not just the mouse axes. Kinda thinking this is no good for me.

#### Device

~~~
I: Bus=0003 Vendor=046d Product=c21f Version=0305
N: Name="Logitech Gamepad F710"
P: Phys=usb-3f980000.usb-1.2/input0
S: Sysfs=/devices/platform/soc/3f980000.usb/usb1/1-1/1-1.2/1-1.2:1.0/input/input3
U: Uniq=
H: Handlers=event3 js0 
B: PROP=0
B: EV=20000b
B: KEY=7fdb0000 0 0 0 0 0 0 0 0 0
B: ABS=3001b
B: FF=1 7030000 0 0
~~~

#### Mapper

~~~
~/.dosbox/mouse.map

# left analog left/right
axis vendor=0x046d product=0xc21f src=0 target=mouse axis=0
# left analog up/down
axis vendor=0x046d product=0xc21f src=1 target=mouse axis=1
# L3 = left click
button vendor=0x046d product=0xc21f src=11 target=mouse button=0
# R3 = left clock
button vendor=0x046d product=0xc21f src=12 target=mouse button=1
~~~

#### Launcher

~~~
Street Rod.sh

#!/bin/bash
sudo /home/pi/joymap/loadmap /home/pi/.dosbox/mouse.map &
/opt/retropie/emulators/dosbox/bin/dosbox -c "@MOUNT C /home/pi/RetroPie/roms/DOS" -c "@C:" -c "@CD SR1" -c "@SR.EXE" -c "@EXIT"
sudo killall loadmap
sleep 1
~~~

## References

* https://www.reddit.com/r/RetroPie/comments/50q04s/getting_320x200_dos_games_to_display_correctly/
* https://retropie.org.uk/forum/topic/2032/dosbox-mapping-mouse-to-ps3-analog-stick