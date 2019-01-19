Read-only root filesystem for Raspbian Stretch
============================================
This repository contains some useful files that allow you to use a Raspberry PI using a readonly filesystem.
After running install.sh everything will be set up and the system will reboot into read-only mode.

See instructions below to see how to switch to permanent or temporary write-mode.

This script is tested with a freshly deployed Raspbian image with "desktop and recommended software", specifically with the img file dated 2018-11-13, kernel 4.14. (Tested on a Rpi 3B+)

This files contains some ideas and code of the following projects:
- https://github.com/josepsanzcamp/root-ro
- https://gist.github.com/niun/34c945d70753fc9e2cc7
- https://github.com/chesty/overlayroot

Congratulate the original authors if these files works as expected. Too, you can congratulate to me to join all files in a one repository and do some changes allowing to use the root and boot filesytem in readonly mode.

Why use a read-only root filesystem
=====
There can be many reasons to configure a read only root filesystem. In my case I use it on Raspberry Pi's which are used for narrowcasting, kiosk installations and dashboard applications. I have the read-only filesystem enabled for three reasons:
- Extend microSD card lifespan.
- Make sure the filesystem isn't corrupted by random power cut shut downs (the Rpi's get their power from USB ports on flatscreen TV's).
- Undo any user changes and fix any user-induced errors by simply rebooting the Raspberry Pi.

Setup
=====
To enable the read-only filesystem, you can execute the follow commands:

```
sudo su
apt-get -y install git
cd /home/pi
git clone https://github.com/JasperE84/root-ro.git
cd root-ro
chmod +x install.sh
./install.sh
```

Rebooting to permanent write-mode (disabling the overlay fs)
============
Execute 
```
sudo /root/reboot-to-writable-mode.sh
```

Rebooting to permanent read-only mode (enabling the overlay fs)
============
Execute 
```
sudo /root/reboot-to-readonly-mode.sh
```

Enabling temporary write access mode:
============
Write access can be enabled using following command.
```
sudo mount -o remount,rw /mnt/root-ro
chroot /mnt/root-ro
```


Exiting temporary write access mode:
===============
Exit the chroot and re-mounting the filesystem:
```
eit
sudo mount -o remount,ro /mnt/root-ro
```

Original state
==============
To return to the original state to allow easy apt-get update/upgrade and rpi-update, you need to add a comment mark to the "initramfs initrd.gz" line to the /boot/config.txt file.
