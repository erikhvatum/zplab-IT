# ZPLAB Production Linux System Installation and Configuration

"ZPLAB production Linux system" refers to any computer in the lab that controls hardware for running scientific experiments and/or is
available to lab users on a shared basis for experiment data storage and analysis. These systems run the latest Kubuntu LTS release and
should be installed and configured as specified in this document.

## Preparation
### Find or make a bootable Kubuntu USB flash memory stick
If there is not already a USB flash memory stick available with the latest Kubuntu LTS release,
[download the 64-bit release](http://www.kubuntu.org/getkubuntu/). Once the download completes, follow the instructions for creating a
bootable USB stick on the operating system where you downloaded Kubuntu:
[OS X / macOS](http://www.ubuntu.com/download/desktop/create-a-usb-stick-on-mac-osx),
[Linux](http://askubuntu.com/questions/372607/how-to-create-a-bootable-ubuntu-usb-flash-drive-from-terminal),
[Windows](http://www.ubuntu.com/download/desktop/create-a-usb-stick-on-windows).

### Boot the Kubuntu USB flash memory stick
On a non-Apple computer, just having the USB stick connected when the system is powered-on
or rebooted often works, but it may be necessary to modify BIOS settings in order to place "USB thumb drives" or "USB hard disks" first
in the boot order. On an Apple computer, it may be necessary to hold down the "C" key as the system is powered-on or rebooted in order
to be presented with a menu allowing boot device selection.

## Installation