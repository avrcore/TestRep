## Issue

When using a Raspberry Pi 2 and or Raspberry Pi 3, the 3.5mm audio connector next to the HDMI port produces a noticeable static "hiss" when configured to output sound.

## Solutions

#### Experimental Audio Driver

As of Feb 2016 there is a different audio method in the Pi firmware, set `audio_pwm_mode=2` in `/boot/config.txt` to use it. This works for me.

#### HDMI-to-VGA Adapter

The whole reason I needed 3.5mm audio out was because I have a VGA monitor for this spare Pi setup. I just bought a HDMI-to-VGA adapter with 3.5mm audio out, this works perfectly similar to HDMI Audio Splitter above.

You need to add `hdmi_drive=2` to `/boot/config.txt` when using one of these.


## Things Which Did Not Work

#### Disable Audio Dither

Some people have said adding `disable_audio_dither=1` to `/boot/config.txt` fixed this for them. Some people say this fixes it in RetroPie but not Raspbian. This did not fix it for me.

## Other Solutions

#### Constantly Play Silence

Some people have had success making the audio device active but constantly playing silence with a command like:

~~~
aplay -t raw -r 48000 -c 2 -f S16_LE /dev/zero
~~~

#### HDMI Audio Splitter

A "*HDMI Audio Splitter*" or "*HDMI Audio Extractor*" is an inline HDMI-to-HDMI adaptor box with a 3.5mm audio socket on it. You would configure the Pi to output audio via HDMI, then plug the speaker or headphone into the 3.5mm socket on the splitter.

These often have other output like RCA and optical SPDIF. These also usually require a separate power supply, usually in the form of a MiniUSB cable (note: not MicroUSB like the Pi's power connector). I expect you could power it off one of the Pi's USB ports.

#### USB Soundcard

Cheap USB soundcards can be purchased off eBay for under $10, and professional-quality USB audio interfaces go up in price from there. Professional audio interfaces are often called a USB DAC (Digital-to-Analog Converter).

The *Plugable USB Audio Adapter* is said to be a good low-cost choice. The *Audio Technica ATR2USB* seems the cheapest professional option. I have a *Behringer UCA222*. Other popular brands are M-Audio, Lexicon, Roland, and PreSonus.

To set this as the default audio interface in RetroPie, run `aplay -l` to list audio interfaces. You'll see something like:

~~~
card 0: ALSA [bcm2835 ALSA], device 0: bcm2835 ALSA [bcm2835 ALSA]
  ... other things here ...
card 1: Device [USB PnP Sound Device], device 0: USB Audio [USB Audio]
  ... other things here ...
~~~

So we can see `card 0` is the built-in audio, and `card 1` is the new audio interface.

Edit `/etc/asound.conf` and make it like the following, then reboot:

~~~
pcm.!default {
 type hw card 1
}
ctl.!default {
 type hw card 1
}
~~~

## Root Cause

Unknown. Some people say this is a hardware problem, some say it's a firmware problem, some think it's a driver problem.

## References:

* https://github.com/raspberrypi/firmware/issues/380
* https://delightlylinux.wordpress.com/2016/02/09/usb-audio-and-retropie-3-4-for-better-sound/
* http://raspberrypi.stackexchange.com/questions/19705/usb-card-as-my-default-audio-device/21989#21989
* https://www.raspberrypi.org/forums/viewtopic.php?f=29&t=136445