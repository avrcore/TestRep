## Overview

This is a set of instructions to create a minimal bootable LiveUSB which runs Linux but boots to DOSBox so you appear to have a DOS computer. This allows you to carry around DOS games and play them on most modern desktops and laptops.

* **Author:** Jamie Bainbridge - jamie.bainbridge@gmail.com
* **License:** [Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/)

## Requirements

* Ubuntu Mini Remix - http://www.ubuntu-mini-remix.org/ - pick the 16.04 i386 image
* Unetbootin - http://unetbootin.github.io/
* A PC with internet access, at least to get DOSBox working and some games on there
* USB drive, 4 GiB or 8 GiB is fine (see [note](#note-about-usb-drive-size) below)
* Some basic knowledge of Linux text editing with `nano` or `vim` and `sudo`. If you don't know this then search Google for "linux nano tutorial" and "linux sudo tutorial".
* An understanding of DOSBox and [its configuration file](http://www.dosbox.com/wiki/dosbox.conf)

## Create a Bootable Drive with Persistent Storage

Format the USB drive as FAT32.

Ensure the first partition is bootable. In Linux use `fdisk` and toggle bootable flag with **a**. In Windows use `diskpart` and set **active**.

Use UNetbootin to put the Ubuntu Mini Remix image onto the USB drive. Select persistent space.

Ubuntu Mini Remix takes up about 400 MiB, so give the size of your thumb drive minus 500 MiB, up to a maximum of 4000 MiB persistent space. eg:

* 2 GiB USB drive - give 1500 MiB persistent space
* 4 GiB USB drive - give 3500 MiB persistent space
* 8 GiB USB drive - give 4000 MiB persistent space

See the [note](#note-about-usb-drive-size) below to understand why you can't add more space than 4 GiB.

The below steps will consume about 640 MiB of the persistent space.

## Network

Plug Ethernet in before boot.

If you forgot to do this, then just reboot:

    sudo reboot

## WiFi (untested)

Find your wireless device name with `ip addr`. If you don't see a wireless device I'd assume your wireless card is not supported in Linux, or you need Ethernet to install more stuff to get the wireless to work, which is beyond the scope of this tutorial.

If you do have a wireless device, I'll assume it's `wlan0`, edit `/etc/network/interfaces` as below:

    auto lo 
    iface lo inet loopback

    iface eth0 inet dhcp

    allow-hotplug wlan0
    auto wlan0
    iface wlan0 inet dhcp
        wpa-ssid "SSID"
        wpa-psk  "Password"
        wireless-power off
    iface default inet dhcp

Reboot:

    sudo reboot

## Setup SSH

The next steps are easier if you can remote in and copy-paste from this page, so we'll setup SSH remote console for that.

You'll also use SSH to copy games onto the LiveUSB system, so do this anyway.

The default login name is `ubuntu` with a blank password.

In `/etc/apt/sources.list` add `universe multiverse` to the end of each repo line.

It should look like this:

    deb http://archive.ubuntu.com/ubuntu/ xenial main restricted universe multiverse
    deb http://security.ubuntu.com/ubuntu/ xenial-security main restricted universe multiverse
    deb http://archive.ubuntu.com/ubuntu/ xenial-updates main restricted universe multiverse

Update repos:

    sudo apt update

Install SSH server:

    sudo apt install openssh-server

Start SSH server:

    sudo systemctl start ssh

Make SSH server start on boot:

    sudo systemctl enable ssh

Edit `/etc/ssh/sshd_config` and change `PermitEmptyPasswords` from `no` to `yes`:

    PermitEmptyPasswords yes

Restart the SSH server to apply this change:

    sudo systemctl restart ssh

If you need to know the LiveUSB system's IP address:

    ip address

Connect to the LiveUSB via SSH. On Windows you can use [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), on Mac you can use [iTerm2](http://www.iterm2.com/).

## Install DOSBox

Make a directory to be the DOSBox C drive:

    mkdir ~/DOS

Install DOSBox and audio:

    sudo apt install dosbox alsa-base pulseaudio

Run DOSBox on its own to make a base config file:

    dosbox

This will look like messy text so type `EXIT` and press **Enter**. If that doesn't work try **Ctrl+c**. Use **Ctrl+l** (lowercase L) to clear the terminal. If the terminal is still messed up, log out with **Ctrl+d** and log back in or just reboot with `sudo reboot`.

To run DOSBox in text mode (without X), we need to use the SDL from RetroPie which is patched to allow hardware scaling on the framebuffer.

We're going to follow part of RetroPie's [Ubuntu x86 Tutorial](https://github.com/RetroPie/RetroPie-Setup/wiki/RetroPie-Ubuntu-16.04-LTS-x86-Flavor) but just install RetroPie's DOSBox, which will install the required SDL. We won't install full RetroPie. We won't use use RetroPie's newer DOSBox-SVN.

Install requirements:

    sudo apt install git dialog

Install RetroPie Setup Script and run it:

    git clone --depth=1 https://github.com/RetroPie/RetroPie-Setup.git
    cd RetroPie-Setup
    sudo ./retropie_setup.sh

In the RetroPie menu select **Manage packages** then **Manage optional packages** then **dosbox** then **Install from source**. Once that's done go **Back** until you exit the script.

Change back to the home directory:

    cd

Edit `~/.dosbox/dosbox-0.74.conf` as below:

    output=overlay
    usescancodes=false

    aspect=true
    scaler=normal2x
    # you can use the others like hq2x or advmame2x if you like them
    # you can use 3x if your screen is big enough

    cycles=25000
    # the ideal cycles number will depend on your computer and game
    cycleup=2500
    cycledown=2500

    [autoexec]
    @ECHO OFF
    MOUNT C ~/DOS
    C:

Run DOSBox as root and it should run on the framebuffer and look nice:

    sudo dosbox

To get out of this DOSBox window, type `EXIT` and press **Enter**

If you want to edit the DOSBox config settings, do that now and run `sudo dosbox` again to test your settings. Repeat until you're happy with it.

## Auto Login

Make this directory:

    sudo mkdir -p /etc/systemd/system/getty@tty1.service.d/

Edit `/etc/systemd/system/getty@tty1.service.d/autologin.conf` as below:

    [Service]
    ExecStart=
    ExecStart=-/sbin/agetty --autologin ubuntu --noclear %I \$TERM

## Auto Start DOSBox

Edit `/etc/profile.d/10-runthing.sh` as below:

    if [ "$(tty)" = "/dev/tty1" ]; then
        sudo dosbox
    fi

## Copying Games via Network (Recommended Method)

If you need to know the LiveUSB system's IP address:

    ip address

Use a graphical SFTP program like [Filezilla](https://filezilla-project.org/) (Lin/Win/Mac) or [WinSCP](https://winscp.net/) (Win) or [CyberDuck](https://cyberduck.io/) (Mac) to copy games to `/home/ubuntu/DOS/` on the LiveUSB system.

The username is `ubuntu` with a blank password.

If you copy new games on while DOSBox is running, press **Ctrl+F4** within DOSBox to refresh the files from disk.

## To Turn Off

Always shut down properly, don't just turn the power off and yank the USB drive out. If you do that, you may lose any changes you've made (like saved games) or corrupt the persistent storage.

Exit DOSBox with `EXIT` or **Ctrl+F9**.

Turn the real computer's power off:

    sudo poweroff

## Speed Control

Turn DOSBox cycles down with **Ctrl+F11**.

Turn DOSBox cycles up with **Ctrl+F12**.

The number of cycles these change correspond to `cycledown` and `cycleup` in the DOSBox config file.

You can also set a cycles value inside DOSBox with a command like: `CYCLES 10000`

## Volume Control

Run `MIXER` inside DOSBox.

You can also use this mixer on the Linux commandline:

    alsamixer

## Mouse Support

Download [CuteMouse](http://cutemouse.sourceforge.net/) and run it inside DOSBox with `CTMOUSE`

You could add that command to the `[autoexec]` section in `dosbox.conf` if you like.

You can adjust mouse sensitivity like `CTMOUSE /R5`, the R number can be 1 to 9 (0 is auto).

You can also adjust mouse sensitivity with `sensitivity` setting in the DOSBox config file.

## Limitations

On some systems, the DOSBox window won't consume the whole screen. This is more likely to happen on laptops. Desktop monitors may have hardware scaling options, play around with the physical menu buttons on the monitor.

DOSBox's aspect correction can be ugly (it just inserts an extra line every 4). Nice scaling modes like `hq2x` help with this. You can't use OpenGL scaling on the framebuffer to scale properly.

Keep in mind that DOSBox in text mode will always be small, but graphics modes will scale up depending on the `scaler` value you've set in the DOSBox config file.

Normal DOSBox cannot scale up past 3x. Some of the forks like [Daum](http://ykhwong.x-y.net/) have 4x and 5x scaling for larger screen resolutions, but installing those is beyond the scope of this tutorial.

## Advanced Topics

If you want to do these, I assume you understand a bit about Linux and filesystem images and other stuff. If you're not comfortable with those topics and just want to play DOS games, read no further.

### Copying Games via USB in Linux/Mac (Not Recommended)

I haven't tested this much, I don't know if it's how you should use the persistent filesystem.

    sudo mkdir -p /mnt/dos
    sudo mount -o loop /path/to/usbdrive/casper-rw /mnt/dos
    sudo chmod -R ugo+rw /mnt/dos/upper/home/ubuntu/DOS

Copy games to `/mnt/dos/upper/home/ubuntu/DOS/`

When finished, unmount the filesystem:

    sudo umount /mnt/dos

Safely eject the USB drive.

### Copying Games via USB in Windows (Not Recommended)

I haven't tested this much, I don't know if it's how you should use the persistent filesystem.

I don't use Windows so I don't know the exact steps for this, you'll need to figure the details out yourself.

Use [OSFMount](http://www.osforensics.com/tools/mount-disk-images.html) to mount the `casper-rw` file which is on the USB drive.

Copy games to `/upper/home/ubuntu/DOS/` inside that mount.

When done, unmount the OSFMount.

Safely eject the USB drive.

### Note About USB Drive Size

The largest file a FAT32 drive can hold is 4 GiB, and the persistent storage is kept within a single file.

This means even if you use a huge USB drive, the most you'll be able to store in the Linux live environment is 4 GiB of changes.

You can extend this space, see: https://askubuntu.com/questions/138356/how-do-i-get-a-live-usb-to-use-a-partition-for-persistence

If you end up with a casper-rw partition, obviously the instructions above to copy games via mounting the casper filesystem image will be different. If you understand enough to partition a disk, you know what you're doing.

----

## To Do List

I'm happy with this document for now.

The End.