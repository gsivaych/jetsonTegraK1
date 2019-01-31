# Ubuntu 16.04 and ROS Kinetic Kame
Host : Machine running Ubuntu 16.04, any other Linux system will do as well.

## Flash L4T R21.7 (Ubuntu 14.04)
Download and save, Linux for Tegra (L4T) Driver package and Sample Root Filesystem for TK1 from [Jetson Download Centre](https://developer.nvidia.com/embedded/downloads#?tx=$product,jetson_tk1), latest being version 21.7

Untar release package as
```
    $ sudo tar xpf Tegra124_Linux_R21.7.0_armhf.tbz2
```
Move to `rootfs` directory
```
    $ cd Linux_for_Tegra/rootfs/
```
Untar and assemble the sample root fileystem
```
    $ sudo tar xpf ../../Tegra_Linux_Sample-Root-Filesystem_R21.7.0_armhf.tbz2
```
Move back a directory and apply binaries
```
    $ cd ../
    $ sudo ./apply_binaries.sh
```
Verify available ext4 space >= 25GiB
```
    $ df -H -T .
```
Power and plug-in TK1 board, put it in reset recovery mode by holding down `recovery` button and pressing `reset` button once

Ensure connectivity
```
    $ lsusb | grep -i "ID 0955:7140 NVidia Corp."
```
Have a look at which loop device is available
```
    $ losetup --find
```
if displayed `/dev/loop0` then we're good to go, else change loop device in 'flash.sh' at line 450

Flash the rootfs onto the system's internal eMMC
```
    $ sudo ./flash.sh -S 14580MiB jetson-tk1 mmcblk0p1
```
System will reboot automatically or press `reset` button to reboot.

## Upgrade to Ubuntu 16.04
Enable `universe` and `multiverse` repositories
```
    $ sudo add-apt-repository universe
    $ sudo add-apt-repository multiverse
```
Put on hold `xorg packages`, also i didn't want firefox to upgrade
```
    $ sudo apt-mark hold xserver-xorg-core
    $ sudo apt-mark hold firefox
```
Update and upgrade the system, then do release upgrade
```
    $ sudo apt-get update
    $ sudo apt-get upgrade -y
    $ sudo do-release-upgrade
```
```
Respond to prompts
1. Proceed with installation ?                              type `y`, hit enter
2. Restart services without asking ?                        choose `Yes`, hit enter
3. Configuration file /etc/pulse/daemon.conf ?              hit enter (keep current version)
4. Configuration file /etc/apt/apt.conf.d/*upgrades ?       hit enter (keep local vesion)
5. Remove obsolete packages ?                               type `y`, hit enter
6. Restart required, restart system ?                       type `y`, hit enter
```


## Install [ROS Kinetic Kame](http://wiki.ros.org/kinetic)
Follow detailed [installation instructions from ROS wiki](http://wiki.ros.org/kinetic/Installation/Ubuntu).

OR use `installKineticKame.sh`.

Test your installation..
```
    $ roscore
```
