# Terrible bash script to do USB Armory related things

This is a bash script I use to automate doing things for my USB
Armory. It's not really intended to be nice or complete. It has pretty
output, however. Linux only.

To use, just drop the `usb-armory` script in your `$PATH`.

```
$ git clone git@github.com/thoughtpolice/usb-armory usb-armory
$ ln -s $PWD/usb-armory/usb-armory $HOME/bin/usb-armory
```

To build a new, clean Debian Jessie image for your USB Armory:

```
$ sudo usb-armory --device /dev/mmcblk0
```

This might take a while. On my Sandy Bridge i5-2520M CPU @ 2.50GHz, it
took a little less than an hour to do everything including downloads,
compiling, etc.

If that works, first off, you're lucky. After congratulating yourself,
try putting the SD card into your USB Armory, and plug it in. You
should see a blinking light once the kernel is booted. Now try this:

```
$ sudo usb-armory --link
$ ssh -o PreferredAuthentications=password usbarmory@10.0.0.1
```

The first command sets up some iptables routes, so you can SSH in and
it can talk to the internet. The second command SSHs in (and ignores
any keys you might have, since it doesn't install any).

Also try `--help`.

**NOTE**: There is no confirmation and the device is immediately
wiped, so, you know, be careful.

**NOTE 2**: The current device name layout assumes an SDHC adapter for
your MicroSD card, because partition names on these devices use a
different layout than USB based devices (`/dev/sda1` vs
`/dev/mmcblk0p1` for example, note the `p`). The current device name
layout is hardcoded. Brave souls can hack the source to fix this for
USB based devices (check out `sdcard_prep` in the script).

The default login from the image creaion is `usbarmory/usbarmory`; it
doesn't copy an SSH key by default, but it probably should.

Things it does:

  - Builds a stock kernel and uboot
  - Bootstraps a Debian image (Ubuntu maybe coming soon)
  - Obliterates the data on your SD card, without mercy.
  - Installs a new image.

It can also:

  - Set up the proper adapter and `iptables` rules to talk with your Armory.
  - Shut off your armory, or whatever computer is located at `10.0.0.1`.

One day it might:

  - Build grsecurity into the kernel.
  - Have cleaner output (not everything is piped/silenced appropriately).
  - Have error reporting (currently: no).
  - Not be written in Bash.

But it will never:

  - Be a suitable replacement for a buildroot or something.
  - Be your friend (it might become your enemy).

Run with care. Use the source. Do not taunt Happy Fun Ball.
