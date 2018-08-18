The **Raspbian** Linux distribution is based off **Debian** and so inherits Debian's package versions.

Debian tend to stick with the same package version for an entire release. Considering they release [about every 2 years](https://en.wikipedia.org/wiki/List_of_Debian_releases#Release_table) this can lead to outdated packages.

For an outdated package, how do we get a later package version on our Raspberry Pi?

## Use Ubuntu

The best answer is to use Ubuntu. This distro updates every 6 months, instead of every 2 years, and almost always has more recent package versions.

**Ubuntu MATE** has made the Raspberry Pi 2 and 3 an officially supported platform:

* https://ubuntu-mate.org/

A few other Ubuntu flavours for Pi 2/3 are available at the **Ubuntu Pi Flavour Maker**:

* https://ubuntu-pi-flavour-maker.org/

However, if you have an existing Raspbian install you don't want to blow away, or if you have a Raspberry Pi 1 or Zero, Ubuntu isn't an option for you.

## Debian Backports

Debian contributors take some packages from the next in-development version (called "testing") and release those packages for the current version (called "stable") via the **Backports** repository:

* https://backports.debian.org/

Debian maintain repositories for the Raspberry Pi's 32-bit architecture (called `armhf`) so we can often use Debian packages directly on the Raspberry Pi's Raspbian.

You can browse the Jessie Backports package list at:

* https://packages.debian.org/jessie-backports/

## WARNING

**This won't always work and may leave your OS broken beyond repair, so make a backup of your sdcard before starting!**

The Backports repo is not as well-tested as Raspbian or Debian, and it's possible Raspbian have made some other changes which break things, and some packages just don't work.

If you see a large amount of libraries and dependent packages being updated, give it a try but expect it not to work or to break other things. If you see fundamental packages like `libc` or `systemd` or `xorg` or `python` being updated, expect that your OS may not boot or work well again. If you update all packages then your OS almost certainly won't boot again.

I've had more success using Backports on Pi 2/3 with their ARMv7 architecture, than on the Pi 1/0 with their ARMv6 architecture. Some binaries segfault so I guess they use ARMv7 features that ARMv6 doesn't have.

## Installation

Add the two Debian package signing keys:

~~~
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 7638D0442B90D010
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 8B48AD6246925553
~~~

Add the Backports repository:

~~~
echo 'deb http://httpredir.debian.org/debian jessie-backports main contrib non-free' | sudo tee -a /etc/apt/sources.list.d/jessie-backports.list
~~~

Update your package lists:

~~~
sudo apt-get update
~~~

## Using Backports

Package repositories can have priority over one another, and Backports is automatically configured with a lower priority so it shouldn't override the main Raspbian packages.

You need to specifically tell apt to use Backports like:

~~~
sudo apt-get -t jessie-backports install packagename
~~~

## Example - youtube-dl

`youtube-dl` is a YouTube downloader script.

We can see in regular Jessie that the current version [youtube-dl (2014.08.05-1+deb8u1)](https://packages.debian.org/jessie/web/youtube-dl) is very old, but in Backports, it's currently [youtube-dl (2016.02.22-1~bpo8+1)](https://packages.debian.org/jessie-backports/web/youtube-dl) which is much better.

Assuming we've installed this before with:

~~~
sudo apt-get install youtube-dl
~~~

Check the version:

~~~
$ youtube-dl --version
2014.08.05
~~~

Update from Backports:

~~~
sudo apt-get -t jessie-backports install youtube-dl
~~~

Check the version again:

~~~
$ youtube-dl --version
2016.02.22
~~~

Success!

## Alternate Key Install Methods

If you're a GPG nerd or want a different way to install the keys, you can also:

~~~
# Debian Archive Automatic Signing Key (8/jessie) <ftpmaster@debian.org>
gpg --keyserver pgpkeys.mit.edu --recv-key  7638D0442B90D010
gpg -a --export 7638D0442B90D010 | sudo apt-key add -

# Debian Archive Automatic Signing Key (7.0/wheezy) <ftpmaster@debian.org>
gpg --keyserver pgpkeys.mit.edu --recv-key  8B48AD6246925553
gpg -a --export 8B48AD6246925553 | sudo apt-key add -
~~~

## References

* https://www.reddit.com/r/raspberry_pi/comments/4zgcsv/update_libreoffice/