I've come across several people who wish to have a Raspberry Pi handheld with a composite/SPI screen when operating as a handheld, yet have the option to plug into a HDMI display when they're at home or near a display.

As far as I know, the display cannot be switched live. You can't be playing on the handheld display, plug into HDMI, and continue where you left off.

However, you can detect the HDMI state on boot with `tvservice`, change the `config.txt` options, and reboot again. This script automates that process at boot time.

To use the HDMI display: Power down, plug in HDMI, power on.

To use the composite/SPI display: Power down, unplug HDMI, power on.

#### The Script

Put this script somewhere like `/bin/displaydetect.sh` and set it executable with `chmod +x /bin/displaydetect.sh`

~~~bash
#!/bin/bash

# this is the config.txt parameter you want to change
MYTHING="overscan_scale"

# store the output of the tvservice commands
TVN=$(/opt/vc/bin/tvservice -n)
#TVS=$(/opt/vc/bin/tvservice -s)

# if the above command matches the HDMI display detected pattern
if [ $(echo "$TVN" | egrep -c "device_name") -gt 0 ]
then
    # we're plugged into HDMI
    OUTPUT="hdmi"
else
    # we're plugged into Composite
    OUTPUT="comp"
fi

# when plugged into HDMI, run this
if [ "$OUTPUT" == "hdmi" ]
then
    # if a line starts "my_parameter" without a comment
    if [ $(egrep -c "^$MYTHING" /boot/config.txt) -gt 0 ]
    then
        # comment out the parameter and reboot
        sed -i -e "s/^$MYTHING/#$MYTHING/g" /boot/config.txt
        reboot
    fi
fi

# when plugged into Composite, run this
if [ "$OUTPUT" == "comp" ]
then
    # if a line starts "#my_parameter" commented out
    if [ $(egrep -c "^#$MYTHING" /boot/config.txt) -gt 0 ]
    then
        # uncomment the parameter and reboot
        sed -i -e "s/^#$MYTHING/$MYTHING/g" /boot/config.txt
        reboot
    fi
fi
~~~

Edit `/etc/rc.local` so it runs that script, and also set that executable with `chmod +x /etc/rc.local`

If you wish to add additional parameters like overscan left/right/top/bottom, you can create additional variables like `MYTHINGA` and `MYTHINGB` and duplicate the `sed` commands.

Reference post:

* https://www.reddit.com/r/RetroPie/comments/4izmyb/handheld_gamepi_hdmi_output/