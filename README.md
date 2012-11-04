About
=====

Initscripts used with
[Gentoo OpenRC init system](http://www.gentoo.org/proj/en/base/openrc/).

General Usage
=============

1. Copy, move or link the script into */etc/init.d/*, e.g.:

    ln -s ./etc/init.d/devmon /etc/init.d/devmon

2. Make the script under */etc/init.d/* executable and change its ownership to
user *root*, group *root*:

    chmod 755 /etc/init.d/devmon
    chown root:root /etc/init.d/devmon

3. If existant, copy, move or link the configuration file to */etc/conf.d/*

    ln -d ./etc/conf.d/devmon /etc/conf.d/devmon

4. Change the ownership of the script under */etc/conf.d/* to user *root*,
group *root* and adjust its permissions:

    chmod 644 /etc/conf.d/devmon
    chown root:root /etc/conf.d/devmon

5. Optionally adjust the configuration file to your needs.
6. Start the deamon with:

    /etc/init.d/devmon start
    
7. Add it to the appropriate runlevel:

    rc-update add devmon default

devmon
======

General Information
-------------------

- [devmon](http://igurublog.wordpress.com/downloads/script-devmon) is a
configuration-less bash daemon script which automounts optical discs and
removable drives. devmon is distributed as part of udevil
- [udevil](http://ignorantguru.github.com/udevil/) is a command line Linux
program which mounts and unmounts removable devices without a password, shows
device info, and monitors device changes.
- Both together are useful if you want to automount removable storage devices,
(e.g. SD-Cards, USB flash drive) when plugged in at your headless NAS.

About this initscript
---------------------

Starts devmon as non-root user.

Prerequisites
-------------

1. Install *udevil* (and optionally *eject*), udevil needs to be unmasked:

    echo ~sys-apps/udevil-0.3.1 >> /etc/portage/package.keywords/udevil
    emerge -av sys-apps/udevil sys-block/eject
    
devmon will be installed together with udevil.

2. Add a new user for devmon:

     useradd -c "added by me for devmon" -d "/dev/null" -g plugdev -M -N -r -s /sbin/nologin devmon

3. To enable the users of the group *plugdev* to write to the removable devices,
edit */etc/udevil/udevil.conf* and add *fmask=0002* and *dmask=0002* to the
allowed mount options (allowed_options*:

    allowed_options = nosuid, noexec, nodev, noatime, fmask=0022, dmask=0022, fmask=0002, dmask=0002, uid=$UID, gid=$GID, ro, rw, sync, flush, remoun

4. Follow the instructions in the *General Usage* section above to install the
initscript.

Usage
-----

When a device is plugged in it is automatically mounted by devmon using udevil.

To umount the device by a non-root user use *udevil*:

    udevil umount /media/my-sd-card
