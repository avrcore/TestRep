This grew into PPAs, repos, and packages, but whatever

## PPAs

### Atom - text editor

https://launchpad.net/~webupd8team/+archive/ubuntu/atom?field.series_filter=xenial

    sudo apt-add-repository ppa:webupd8team/atom
    sudo apt install atom

### FreeCAD - parametric 3D modeler

https://launchpad.net/~freecad-maintainers/+archive/ubuntu/freecad-stable?field.series_filter=xenial

    sudo apt-add-repository ppa:freecad-maintainers/freecad-stable
    sudo apt install freecad

### FS-UAE - Amiga Emulator

https://launchpad.net/~fengestad/+archive/ubuntu/stable?field.series_filter=xenial

    sudo apt-add-repository ppa:fengestad/stable
    sudo apt install fs-uae fs-uae-arcade fs-uae-launcher

### GIMP - GNU Image Manipulation Program

https://launchpad.net/~otto-kesselgulasch/+archive/ubuntu/gimp?field.series_filter=xenial

    sudo add-apt-repository ppa:otto-kesselgulasch/gimp
    sudo apt install gimp

also a nightly/edge repo available:

https://launchpad.net/~otto-kesselgulasch/+archive/ubuntu/gimp-edge?field.series_filter=xenial

    sudo add-apt-repository ppa:otto-kesselgulasch/gimp-edge

### Inkscape - vector drawing tool

https://launchpad.net/~inkscape.dev/+archive/ubuntu/stable?field.series_filter=xenial

    sudo add-apt-repository ppa:inkscape.dev/stable
    sudo apt install inkscape

### Midori - lightweight web browser

https://launchpad.net/~midori/+archive/ubuntu/ppa

    sudo apt-add-repository ppa:midori/ppa
    sudo apt-get install midori

Also downloads on the website: http://midori-browser.org/download/ubuntu/

### OpenSCAD - constructive solid geometry 3D modeler

https://launchpad.net/~openscad/+archive/ubuntu/releases?field.series_filter=xenial

    sudo apt-add-repository ppa:openscad/releases
    sudo apt install openscad

Actually you're better to use the nightlies:

    wget -qO - http://files.openscad.org/OBS-Repository-Key.pub | sudo apt-key add -
    echo 'deb http://download.opensuse.org/repositories/home:/t-paul/xUbuntu_16.04/ ./' | sudo tee -a /etc/apt/sources.list.d/openscad.list
    echo 'deb http://download.opensuse.org/repositories/home:/t-paul/xUbuntu_18.04/ ./' | sudo tee -a /etc/apt/sources.list.d/openscad.list

### OpenXcom - turn-based strategy game

https://launchpad.net/~winterheart/+archive/ubuntu/openxcom?field.series_filter=xenial

    sudo apt-add-repository ppa:winterheart/openxcom
    sudo apt install openxcom

There's also these "official" repos by the developer, but they are not frequently updated:

https://launchpad.net/~knapsu/+archive/ubuntu/openxcom?field.series_filter=xenial

https://launchpad.net/~knapsu/+archive/ubuntu/openxcom-beta?field.series_filter=xenial

### Qt 5.6 - needed for Simplify3D 4.0 to work on Xenial

https://launchpad.net/~beineri/+archive/ubuntu/opt-qt562-xenial?field.series_filter=xenial

    sudo apt-add-repository ppa:beineri/opt-qt562-xenial
    sudo apt install qt56-meta-full

S3D is now BTFO, use [Cura](https://ultimaker.com/en/products/ultimaker-cura-software) or [Slic3r](https://dl.slic3r.org/dev/linux/).

### RetroArch - multi-platform emulator frontend and framework

https://launchpad.net/~libretro/+archive/ubuntu/stable?field.series_filter=xenial

    sudo apt-add-repository ppa:libretro/stable
    sudo apt install retroarch libretro-<whatever>

There's also a testing/nightly repo if you feel like updating hundreds of MiB of cores every day:

https://launchpad.net/~libretro/+archive/ubuntu/testing?field.series_filter=xenial

    sudo add-apt-repository ppa:libretro/testing

### UNetBootin - LiveUSB creator

* https://launchpad.net/~gezakovacs/+archive/ubuntu/ppa?field.series_filter=xenial

    sudo apt-add-repository ppa:gezakovacs/ppa
    sudo apt install unetbootin

### webupd8 - Lots of different packages

* https://launchpad.net/~nilarimogard/+archive/ubuntu/webupd8?field.series_filter=xenial

    sudo apt-add-repository ppa:nilarimogard/webupd8
    sudo apt install ncmpcpp

### Wireshark - packet capture analysis tool

* https://launchpad.net/~wireshark-dev/+archive/ubuntu/stable?field.series_filter=xenial

    sudo apt-add-repository ppa:wireshark-dev/stable
    sudo apt install wireshark

No Bionic yet.

## Repositories

### Google - Chrome and other evil internet overlordship

* https://www.google.com/linuxrepositories/

    wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
    sudo apt install google-chrome-stable

### Mopidy - MPD-compatible music daemon

* https://docs.mopidy.com/en/latest/installation/debian/

    wget -q -O - https://apt.mopidy.com/mopidy.gpg | sudo apt-key add -
    sudo wget -q -O /etc/apt/sources.list.d/mopidy.list https://apt.mopidy.com/jessie.list
    sudo apt install mopidy mopidy-youtube

### OpenVPN - VPN client/server

* https://community.openvpn.net/openvpn/wiki/OpenvpnSoftwareRepos

    sudo -s
    wget -O - https://swupdate.openvpn.net/repos/repo-public.gpg|apt-key add -
    echo "deb http://build.openvpn.net/debian/openvpn/stable xenial main" > /etc/apt/sources.list.d/openvpn-aptrepo.list
    sudo apt install openvpn

### WINE - Windows API compatibility layer

* https://www.winehq.org/pipermail/wine-devel/2017-March/117104.html

    wget https://dl.winehq.org/wine-builds/Release.key
    sudo apt-key add Release.key
    sudo apt-add-repository 'https://dl.winehq.org/wine-builds/ubuntu/'

No idea what to install.

## Packages

### mate-calc - the best calculator

MATE from 1.10 uses `galculator`. These are `mate-calc-common` and `mate-calc` from the Linux Mint repos.

http://packages.linuxmint.com/

    wget http://packages.linuxmint.com/pool/backport/m/mate-calc/mate-calc_1.18.1-1+sonya_amd64.deb
    wget http://packages.linuxmint.com/pool/backport/m/mate-calc/mate-calc-common_1.18.1-1+sonya_all.deb

Not needed as of Ubuntu 18.04 Bionic, as MATE 1.18 returns to using `mate-calc`.