# ZPLAB Production Linux System Installation and Configuration

"ZPLAB production Linux system" refers to any computer in the lab that controls hardware for running scientific experiments and/or is
available to lab users on a shared basis for experiment data storage and analysis. These systems run the latest Kubuntu LTS release and
should be installed and configured as specified in this document.

### Kubuntu vs Ubuntu

Kubuntu is an officially supported variant of Ubuntu, differing only in that KDE rather than Unity is installed by default. The difference
is relatively unimportant, and these instructions should apply just as well to Ubuntu as to Kubuntu.

### A note on installing to a system with an existing RAID array

If the array or arrays were created following the instructions in the [array protocols and procedures document](./array_protocols_and_procedures.md),
no additional steps are required in order to protect the array from damage during Linux installation or reinstallation. The disk to be
used as root should be readily identifiable in the output of the `lsblk` command. If care is taken to partition and format only this
disk, the installation process will have no impact on the array. A configuration step is required to make the array properly visible
to users of the newly installed system; this step is detailed in the _Configuring a pre-existing array_ subsection of the _Configuration_
section.

## Installation

### Ensure that the root drive has the highest BIOS boot drive priority

Although plugging the root drive into motherboard SATA controller port 0 helps to avoid this problem, it may still happen that a RAID
disk is selected as the most preferred boot drive. If, during installation, some drive other than the root is first in the BIOS boot
order, the system becomes dependant on that drive's presence in order to boot. The configuration script you are instructed to run
later in this document will note this condition.

TODO: phoenix bios drive order screenshot

### Find or make a bootable Kubuntu USB flash memory stick

If there is not already a USB flash memory stick available with the latest Kubuntu LTS release,
[download the 64-bit release](http://www.kubuntu.org/getkubuntu/). Once the download completes, follow the instructions for creating a
bootable USB stick on the operating system where you downloaded Kubuntu:
[OS X / macOS](http://www.ubuntu.com/download/desktop/create-a-usb-stick-on-mac-osx),
[Linux](http://askubuntu.com/questions/372607/how-to-create-a-bootable-ubuntu-usb-flash-drive-from-terminal),
[Windows](http://www.ubuntu.com/download/desktop/create-a-usb-stick-on-windows).

### Boot the Kubuntu USB flash memory stick

On a non-Apple computer, just having the USB stick connected when the system is powered-on or rebooted
often works, but it may be necessary to modify BIOS settings in order to place "USB thumb drives" or "USB hard disks" first in the boot order. On an
Apple computer, it may be necessary to hold down the "C" key as the system is powered-on or rebooted in order to be presented with a menu allowing
boot device selection.

### Open a terminal window

Once the system boots into a graphical interface and settles down, click the launcher button in the lower left corner of the screen,
type "konsole", and press enter.

![Screenshot of opening konsole in live environment](./opening_konsole_in_live_env.png)

### Verify that a network connection is available

A network connection is required for the next step and for following steps. At ZPLAB, getting a
network connection typically only requires plugging in a network cable connected to a working bench or wall port. Not all bench and wall ports work.
In particular, open ports in obscure locations tend not to be "provisioned", a state that can mean many different things, but one that is always
resolved by sending and email to Kyle.

Connectivity can be verified by running `ping www.google.com` in the terminal window opened in the previous step.

### Run the GUI installer

Minimize the Konsole window you opened previous and click the GUI installer icon on the desktop.

![Screenshot of Konsole minimized and mouse cursor on GUI installer icon](./run_gui_installer.png)

The default installer options are fine. Even the correct timezone should be detected. When prompted, create a user with the name and user name
`zplab` and the password `wustlzplab`.

### Reboot when prompted

After the installer completes, click reboot when prompted, and then remove the USB flash memory stick when prompted. The system should boot 

## Configuration

### ZFS?

Configuring the installation to use a ZFS root recommended for multi-user production Linux systems. ZFS provides good options for rapidly
escaping bad situations, and it has saved our bacon a number of times. However, a special-purpose system that is fast to configure and rarely
changed wouldn't necessarily benefit from ZFS, and any system with less than 16GiB of RAM is unlikely to receive any benefit unless a specific need
exists.

* Example of a system that **should have a ZFS root**: a custom-built computer controlling a Leica microscope situated in an incubator, hosting
a number of experiments belonging to any number of lab members.

* Example of a system that **may optionally have a ZFS root**: a fully-upgraded Mac Mini that has 16GiB of RAM, run Kubuntu, is connected to 6
scanners, and is primarily used by one lab member. OS X / macos would be a good choice for this role, except that SANE tends to be better behaved
on Linux and large numbers of USB devices are known to be problematic on certain OS X / macos releases.

* Example of a system that **should not have a ZFS root without a specific reason**: a Mac Mini running Linux, used by a single user for analyzing
data, examining images, reading and composing papers, email, presentations, and for other day-to-day tasks. Feel free to use whatever operating
system and configuration is most efficient for you on your assigned desktop system. This may be OS X / macos, Kubuntu, Windows, or some other Linux
flavor. However, any time spent using a ZFS root will probably be wasted unless you know you need it for some specific reason, such as manipulating
snapshots of million-file source trees as you track down a bug in Chromium (snapshot copy-on-write is a big time saver in this scenario - but this
scenario is uncommon at ZPLAB).

## 