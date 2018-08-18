Most RetroPie BIOS files go in the `/home/pi/RetroPie/BIOS/` directory.

The specific BIOS files are listed on each system page on the [RetroPie wiki](https://github.com/RetroPie/RetroPie-Setup/wiki).

## Neo-Geo (and other MAME BIOSes)

The exception to the above rule is the Neo-Geo BIOS `neogeo.zip` and other BIOSes like for arcade emulators like MAME.

These go in the same folder as the arcade ROMs for the arcade emulator in use.

These BIOSes do not go in the `BIOS` directory.

For example, if you have `mslug.zip` in the `/home/pi/RetroPie/roms/fba/` directory then you should also have `neogeo.zip` in that same directory.

The BIOS files may be different for different versions of the emulator in use, so always verify with the DAT files as explained at: https://github.com/RetroPie/RetroPie-Setup/wiki/Managing-ROMs

## Known Good BIOS Details

### Game Boy Advance

~~~
gba_bios.bin
16384 bytes
md5sum - a860e8c0b6d573d191e4ec7db1b1e4f6
sha1sum - 300c20df6731a33952ded8c436f7f186d25d3492
sha256sum - fd2547724b505f487e6dcb29ec2ecff3af35a841a77ab2e85fd87350abd36570
~~~

### Sony PlayStation

~~~
scph1001.bin
524288 bytes
md5sum - 924e392ed05558ffdb115408c263dccf
sha1sum - 10155d8d6e6e832d6ea66db9bc098321fb5e8ebf
sha256sum - 71af94d1e47a68c11e8fdb9f8368040601514a42a5a399cda48c7d3bff1e99d3
~~~