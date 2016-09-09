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

![Screenshot of opening konsole in live environment](./opening_konsole_in_live_env.png)

### Verify that a network connection is available
A network connection is required for the next step and for following steps. At ZPLAB, getting a network connection typically only requires
plugging in a network cable connected to a working bench or wall port. Not all bench and wall ports work - in particular, open ports in
obscure locations tend not to be "provisioned", a state that can mean many different things, but one that is always resolved by sending
and email to Kyle.

Connectivity can be verified by running `ping www.google.com` in the terminal window opened in the previous step.

### ZFS?
Configuring the installation to use a ZFS root recommended for multi-user production Linux systems. ZFS provides good options for rapidly
escaping bad situations, and it has saved our bacon a number of times. However, a special-purpose system that is fast to configure and
rarely changed wouldn't necessarily benefit from ZFS, and any system with less than 16GiB of RAM is unlikely to receive any benefit unless
a specific need exists.

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

### If you don't need ZFS
___Skip this section if you need a ZFS root volume!___

If you don't need a ZFS root, using the GUI installer will save you some time. Minimize the Konsole window you opened previous and click the GUI
installer icon on the desktop.

![Screenshot of Konsole minimized and mouse cursor on GUI installer icon](./use_gui_installer_instead.png)

The installer default options are fine. Even the correct timezone should be detected. When prompted, create a user with the name and user name
`zplab` and the password `wustlzplab`. Once the installation completes and the system has booted into the new installation, resume following the
intructions in this document at start of the _Configuring NSS_ section.

### Install ZFS Utilities in the Live Environment
Although ZFS is officially supported by Ubuntu and is included in the official Ubuntu package repository, the environment you get
when you boot the Ubuntu/Kubuntu installation media does not have the ZFS utilities installed. Forunately, installing ZFS in the
live environment is no more difficult than installing any package on a fully configured Ubuntu system.

In the Konsole window you opened previously, type `sudo -s` and press enter. Next, type `apt-get update` and press enter. You should see something
like the following:

```
kubuntu@kubuntu:~$ apt-get update
W: chmod 0700 of directory /var/lib/apt/lists/partial failed - SetupAPTPartialDirectory (1: Operation not permitted)
E: Could not open lock file /var/lib/apt/lists/lock - open (13: Permission denied)
E: Unable to lock directory /var/lib/apt/lists/
W: Problem unlinking the file /var/cache/apt/pkgcache.bin - RemoveCaches (13: Permission denied)
W: Problem unlinking the file /var/cache/apt/srcpkgcache.bin - RemoveCaches (13: Permission denied)
E: Could not open lock file /var/lib/dpkg/lock - open (13: Permission denied)
E: Unable to lock the administration directory (/var/lib/dpkg/), are you root?
kubuntu@kubuntu:~$ sudo -s
root@kubuntu:~# apt-get update
Ign:1 cdrom://Kubuntu 16.04.1 LTS _Xenial Xerus_ - Release amd64 (20160719) xenial InRelease
Hit:2 cdrom://Kubuntu 16.04.1 LTS _Xenial Xerus_ - Release amd64 (20160719) xenial Release
Get:3 http://security.ubuntu.com/ubuntu xenial-security InRelease [94.5 kB]
Hit:5 http://archive.ubuntu.com/ubuntu xenial InRelease
Get:6 http://security.ubuntu.com/ubuntu xenial-security/main amd64 Packages [138 kB]
Get:7 http://archive.ubuntu.com/ubuntu xenial-updates InRelease [95.7 kB]
Get:8 http://security.ubuntu.com/ubuntu xenial-security/main Translation-en [56.6 kB]
Get:9 http://security.ubuntu.com/ubuntu xenial-security/main amd64 DEP-11 Metadata [66.8 kB]
Get:10 http://security.ubuntu.com/ubuntu xenial-security/main DEP-11 64x64 Icons [58.1 kB]
Get:11 http://security.ubuntu.com/ubuntu xenial-security/universe amd64 Packages [41.1 kB]
Get:12 http://security.ubuntu.com/ubuntu xenial-security/universe Translation-en [24.9 kB]
Get:13 http://security.ubuntu.com/ubuntu xenial-security/universe amd64 DEP-11 Metadata [2,328 B]
Get:14 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 Packages [383 kB]
Get:15 http://archive.ubuntu.com/ubuntu xenial-updates/main Translation-en [146 kB]
Get:16 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 DEP-11 Metadata [299 kB]
Get:17 http://archive.ubuntu.com/ubuntu xenial-updates/main DEP-11 64x64 Icons [188 kB]
Get:18 http://archive.ubuntu.com/ubuntu xenial-updates/universe amd64 Packages [324 kB]
Get:19 http://archive.ubuntu.com/ubuntu xenial-updates/universe Translation-en [111 kB]
Get:20 http://archive.ubuntu.com/ubuntu xenial-updates/universe amd64 DEP-11 Metadata [102 kB]
Get:21 http://archive.ubuntu.com/ubuntu xenial-updates/universe DEP-11 64x64 Icons [93.1 kB]
Fetched 2,224 kB in 1s (1,710 kB/s)

** (appstreamcli:4595): CRITICAL **: Error while moving old database out of the way.
AppStream cache update failed.
Reading package lists... Done
root@kubuntu:~# apt-cache search zfs
parted - disk partition manipulator
bzflag-server - 3D first person tank battle game -- server
collectd-core - statistics collection and monitoring daemon (core system)
golang-go-zfs-dev - Go library for ZFS manipulation
libguestfs-zfs - guest disk image management system - ZFS support
libuutil1linux - Solaris userland utility library for Linux
libuutil1linux-dbg - Debugging symbols for libuutil1linux
libzfs2linux - Native OpenZFS filesystem library for Linux
libzfs2linux-dbg - Debugging symbols for libzfs2
libzfslinux-dev - Native OpenZFS filesystem development files for Linux
libzpool2linux - Native OpenZFS pool library for Linux
simplesnap - Simple and powerful network transmission of ZFS snapshots
zfs-dkms - Native OpenZFS filesystem kernel modules for Linux
zfs-doc - Native OpenZFS filesystem documentation and examples.
zfs-fuse - ZFS on FUSE
zfs-initramfs - Native OpenZFS root filesystem capabilities for Linux
zfs-zed - OpenZFS Event Daemon (zed)
zfs-zed-dbg - Debugging symbols for zfs-zed
zfsnap - Automatic snapshot creation and removal for ZFS
zfsutils-linux - Native OpenZFS management utilities for Linux
zfsutils-linux-dbg - Debugging symbols for zfsutils-linux
root@kubuntu:~#
```

The `** (appstreamcli:4595): CRITICAL **: Error while moving old database out of the way.` error is expected and should be
ignored. (apt failed to move the old database because there was no old database, so that's OK.)

Next, type `apt-get install zfsutils-linux` and press enter. You should see something like the following:

```
apt-get install zfsutils-linux
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
  libnvpair1linux libuutil1linux libzfs2linux libzpool2linux zfs-doc zfs-zed
Suggested packages:
  default-mta | mail-transport-agent nfs-kernel-server zfs-initramfs
The following NEW packages will be installed:
  libnvpair1linux libuutil1linux libzfs2linux libzpool2linux zfs-doc zfs-zed zfsutils-linux
0 upgraded, 7 newly installed, 0 to remove and 98 not upgraded.
Need to get 896 kB of archives.
After this operation, 2,897 kB of additional disk space will be used.
Do you want to continue? [Y/n]
Get:1 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 zfs-doc all 0.6.5.6-0ubuntu12 [49.5 kB]
Get:2 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libuutil1linux amd64 0.6.5.6-0ubuntu12 [27.5 kB]
Get:3 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libnvpair1linux amd64 0.6.5.6-0ubuntu12 [23.5 kB]
Get:4 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libzpool2linux amd64 0.6.5.6-0ubuntu12 [385 kB]
Get:5 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 libzfs2linux amd64 0.6.5.6-0ubuntu12 [106 kB]
Get:6 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 zfsutils-linux amd64 0.6.5.6-0ubuntu12 [276 kB]
Get:7 http://archive.ubuntu.com/ubuntu xenial-updates/main amd64 zfs-zed amd64 0.6.5.6-0ubuntu12 [29.8 kB]
Fetched 896 kB in 0s (1,134 kB/s)
Selecting previously unselected package zfs-doc.
(Reading database ... 161341 files and directories currently installed.)
Preparing to unpack .../zfs-doc_0.6.5.6-0ubuntu12_all.deb ...
Unpacking zfs-doc (0.6.5.6-0ubuntu12) ...
Selecting previously unselected package libuutil1linux.
Preparing to unpack .../libuutil1linux_0.6.5.6-0ubuntu12_amd64.deb ...
Unpacking libuutil1linux (0.6.5.6-0ubuntu12) ...
Selecting previously unselected package libnvpair1linux.
Preparing to unpack .../libnvpair1linux_0.6.5.6-0ubuntu12_amd64.deb ...
Unpacking libnvpair1linux (0.6.5.6-0ubuntu12) ...
Selecting previously unselected package libzpool2linux.
Preparing to unpack .../libzpool2linux_0.6.5.6-0ubuntu12_amd64.deb ...
Unpacking libzpool2linux (0.6.5.6-0ubuntu12) ...
Selecting previously unselected package libzfs2linux.
Preparing to unpack .../libzfs2linux_0.6.5.6-0ubuntu12_amd64.deb ...
Unpacking libzfs2linux (0.6.5.6-0ubuntu12) ...
Selecting previously unselected package zfsutils-linux.
Preparing to unpack .../zfsutils-linux_0.6.5.6-0ubuntu12_amd64.deb ...
Unpacking zfsutils-linux (0.6.5.6-0ubuntu12) ...
Selecting previously unselected package zfs-zed.
Preparing to unpack .../zfs-zed_0.6.5.6-0ubuntu12_amd64.deb ...
Unpacking zfs-zed (0.6.5.6-0ubuntu12) ...
Processing triggers for libc-bin (2.23-0ubuntu3) ...
Processing triggers for initramfs-tools (0.122ubuntu8.1) ...
update-initramfs is disabled since running on read-only media
Processing triggers for systemd (229-4ubuntu7) ...
Processing triggers for ureadahead (0.100.0-19) ...
Processing triggers for man-db (2.7.5-1) ...
Setting up zfs-doc (0.6.5.6-0ubuntu12) ...
Setting up libuutil1linux (0.6.5.6-0ubuntu12) ...
Setting up libnvpair1linux (0.6.5.6-0ubuntu12) ...
Setting up libzpool2linux (0.6.5.6-0ubuntu12) ...
Setting up libzfs2linux (0.6.5.6-0ubuntu12) ...
Setting up zfsutils-linux (0.6.5.6-0ubuntu12) ...
zfs-import-cache.service is a disabled or a static unit, not starting it.
zfs-import-scan.service is a disabled or a static unit, not starting it.
zfs-mount.service is a disabled or a static unit, not starting it.
Processing triggers for initramfs-tools (0.122ubuntu8.1) ...
update-initramfs is disabled since running on read-only media
Setting up zfs-zed (0.6.5.6-0ubuntu12) ...
zed.service is a disabled or a static unit, not starting it.
Processing triggers for libc-bin (2.23-0ubuntu3) ...
Processing triggers for systemd (229-4ubuntu7) ...
Processing triggers for ureadahead (0.100.0-19) ...
root@kubuntu:~#
```

## Installation

## Configuring NSS

## Configuring a user Python environment