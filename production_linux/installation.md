# ZPLAB Production Linux System Installation and Configuration

"ZPLAB production Linux system" refers to any computer in the lab that controls hardware for running scientific experiments and/or is
available to lab users on a shared basis for experiment data storage and analysis. These systems run the latest Kubuntu LTS release and
should be installed and configured as specified in this document.

## Kubuntu vs Ubuntu
Kubuntu is an officially supported variant of Ubuntu, differing only in that KDE rather than Unity is installed by default. The difference
is relatively unimportant, and these instructions should apply just as well to Ubuntu as to Kubuntu.

## Preparation
### Find or make a bootable Kubuntu USB flash memory stick
If there is not already a USB flash memory stick available with the latest Kubuntu LTS release,
[download the 64-bit release](http://www.kubuntu.org/getkubuntu/). Once the download completes, follow the instructions for creating a
bootable USB stick on the operating system where you downloaded Kubuntu:
[OS X / macOS](http://www.ubuntu.com/download/desktop/create-a-usb-stick-on-mac-osx),
[Linux](http://askubuntu.com/questions/372607/how-to-create-a-bootable-ubuntu-usb-flash-drive-from-terminal),
[Windows](http://www.ubuntu.com/download/desktop/create-a-usb-stick-on-windows).

### Boot the Kubuntu USB flash memory stick
On a non-Apple computer, just having the USB stick connected when the system is powered-on or rebooted often works, but it may be
necessary to modify BIOS settings in order to place "USB thumb drives" or "USB hard disks" first in the boot order. On an Apple
computer, it may be necessary to hold down the "C" key as the system is powered-on or rebooted in order to be presented with a
menu allowing boot device selection.

### Open a terminal window
Once the system boots into a graphical interface and settles down, click the launcher button in the lower left corner of the screen,
type "konsole", and press enter.

[Screenshot of opening konsole in live environment]("./opening_konsole_in_live_env.jpg" Opening konsole in live environment)

### Verify that a network connection is available
A network connection is required for the next step and for following steps. At ZPLAB, getting a network connection typically only requires
plugging in a network cable connected to a working bench or wall port. Not all bench and wall ports work - in particular, open ports in
obscure locations tend not to be "provisioned", a state that can mean many different things, but one that is always resolved by sending
and email to Kyle.

Connectivity can be verified by running `ping www.google.com` in a terminal such as Konsole. 

### Install ZFS Utilities in the Live Environment
Although ZFS is officially supported by Ubuntu and is included in the official Ubuntu package repository, the environment you get
when you boot the Ubuntu/Kubuntu installation media does not have the ZFS utilities installed. Forunately, installing ZFS in the
live environment is no more difficult than installing any package on a fully configured Ubuntu system. 

## Installation
