This is a fairly complete guide on installing the excellent [One Must Fall: 2097](https://en.wikipedia.org/wiki/One_Must_Fall:_2097) in RetroPie's DOSBox so you can play it with a gamepad.

This assumes knowledge of how to SSH into your Pi or use the commandline, how to edit and save config files in Linux, and how to use the keymapper in DOSBox.

You need a keyboard and mouse connected for the initial setup, but not to play once it's properly setup.

I tried with the RetroPie 4.0.2 image, and with RetroPie 3.8.1 image with the 4.0.0-dev setup script.

On a Pi 1 (overclocked to 900/450/450/2) this ran too slow, even with all the sound and graphics settings turned down. The Pi 1 can only manage about 3000 DOSBox cycles effectively which isn't enough to play this smoothly.

On a Pi 3 (no overclock) this ran so fast I had to restrict DOSBox's speed settings to a fixed cycle count. It runs really well!

### Install DOSBox

Run the setup script:

~~~
cd ~/RetroPie-Setup
sudo ./retropie_setup.sh
~~~

Manage Optional packages, Install `dosbox` package.

### Configure DOSBox

I configure a fixed CPU cycles or it runs too fast, and increase the sound buffer because I have always found this helps prevent sound stuttering.

I set the joystick type to FCS Thrustmaster so it recognises my axis-based D-Pad as joystick input. I don't know if this is required in DOSBox-SVN on the Pi, but I have to do this on my desktop DOSBox-0.74 setup so I did it here too.

You can edit the config files in `~/.dosbox/` and they are placed into `/opt/retropie/configs/all/pc/` automatically. The two files are a hardlink to the same inode, so there is no difference in which file you edit.

I copied the original config and mapper files to create a game-specific config which we'll launch later.

These are the changes I made, not the complete file:

~~~
~/.dosbox/omf2097.conf

[sdl]
windowresolution=1024x768
mapperfile=omf2097.map

[render]
aspect=true
scaler=none

[cpu]
core=auto
cycles=fixed 12500
cycleup=1000
cycledown=1000

[mixer]
prebuffer=50

[joystick]
joysticktype=fcs
~~~

Launch the DOSBox keymapper to bind a keyboard keypress to a joystick button by running:

~~~
/opt/retropie/emulators/dosbox/bin/dosbox -startmapper
~~~

I configured like this:

* Enter key = B button
* Right Shift key = Y button
* Esc key = X button
* Arrow keys = D-Pad
* Quit DOSBox = Select+Start
* DOSBox cycles down = Select+L2
* DOSBox cycles up = Select+R2

Using a Logitech F710 resulted the following changes to the mapper file:

~~~
~/.dosbox/mapper-SVN.map

hand_shutdown "stick_0 button 9 mod3" "key 290 mod1" 
hand_cycledown "key 292 mod1" "stick_0 button 6 mod3" 
hand_cycleup "stick_0 button 7 mod3" "key 293 mod1" 
key_esc "stick_0 button 3" "key 27" 
key_enter "stick_0 button 0" "key 13" 
key_a "stick_0 button 4" "key 97" 
key_d "stick_0 button 5" "key 100" 
key_rshift "stick_0 button 2" "key 303" 
key_lctrl "stick_0 button 0" "key 306" 
key_space "stick_0 button 2" "key 32" 
key_up "stick_0 hat 0 1" "key 273" 
key_left "stick_0 hat 0 8" "key 276" 
key_down "stick_0 hat 0 4" "key 274" 
key_right "stick_0 hat 0 2" "key 275" 
mod_3 "stick_0 button 8" 
~~~

I also removed the joystick mappings later in this file which were added automatically. Any entry which starts with `j` just remove the configured parameter from it so you have an empty list like this:

~~~
jbutton_0_0
jbutton_0_1
jaxis_0_1-
jaxis_0_1+
jaxis_0_0-
jaxis_0_0+
jbutton_0_2
jbutton_0_3
jbutton_1_0 
jbutton_1_1 
jaxis_0_2-
jaxis_0_2+
jaxis_0_3-
jaxis_0_3+
jaxis_1_0- 
jaxis_1_0+ 
jaxis_1_1- 
jaxis_1_1+ 
jbutton_0_4
jbutton_0_5
jhat_0_0_0
jhat_0_0_3
jhat_0_0_2
jhat_0_0_1
~~~

### Install OMF

Put OMF in the a directory `/home/pi/RetroPie/roms/DOS/OMF`, note this is not the `pc` directory. This way the folder doesn't appear in the PC menu and look messy.

I copied an existing install from my PC so I had already run `SETUP.EXE` and configured the soundcard. If you haven't done this, connect a USB keyboard and do so.

Configure Player 1 to use the "Right Keyboard" controls, and Player 2 to use the "Left Keyboard" controls.

### Create Launcher

~~~
cd ~/RetroPie/roms/pc
cp "+Start DOSBox.sh" "One Must Fall 2079.sh"
~~~

In that new file, setup the parameters to launch and quit automatically:

~~~
    params=(-c "@MOUNT C /home/pi/RetroPie/roms/DOS" -c "@C:" -c "@CD OMF" -c "@FILE0001.EXE" -c "@EXIT")
~~~

Restart EmulationStation and you're done!

### References

* http://dosonthepi.blogspot.com/2015/01/run-dos-games-in-retropie_15.html
* http://dosonthepi.blogspot.com/2015/01/configure-game-controllers-in-dosbox_29.html
* http://www.dosbox.com/DOSBoxManual.html#Joystick