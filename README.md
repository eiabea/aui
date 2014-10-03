Installation Guide for Arch Linux
=================================
---

* Create USB Stick
```
dd bs=4M if=arch.img of=/dev/sdX && sync
```

* Set Keymap
```
loadkeys de
```

* Generate Locales
```
locale-gen
```

---


#### Establish Internetconnection
##### Wireless

* Find name of Wifi Card
```
iw dev
```

* Open wifi-menu
```
wifi-menu wlp2s0
```

* Connect to Wifi

#### Prepare Disk
##### Create Partitions
* Delete Partitiontable
```
sgdisk --zap-all /dev/sda
```
* Start cgdisk
```
cgdisk /dev/sda
```
* Create Swap Partition (Hex: 8200)
* Create Root Partition (Hex: 8300)
* Create Home Partition (Hex: 8300)
* Write Partitions to Disk
* Quit cgdisk

##### Create Filesystems

* Swap
```
mkswap /dev/sda1
swapon /dev/sda1
```

* Other Partitions
```
mkfs.ext4 /dev/sda2
mkfs.ext4 /dev/sda3
```

* Mount Root Partition
```
mount /dev/sd2 /mnt
```

* Create Home Directory
```
mkdir /mnt/home
```

* Mount Home Partition
```
mount /dev/sd3 /mnt
```

#### Install Base System

```
pacstrap -i /mnt base base-devel
```

#### Generating Fstab
```
genfstab -U -p /mnt >> /mnt/etc/fstab
```

#### Chroot into installed System
```
arch-chroot /mnt /bin/bash
```

* Generate Locale
```
nano /etc/locale.gen
// Uncomment en_US.UTF-8
locale-gen
```

* Set default Language
```
echo LANG=en_US.UTF-8 > /etc/locale.conf
export LANG=en_US.UTF-8
```

* Set Keymap and Font
```
nano /etc/vconsole.conf
KEYMAP=de-latin1-nodeadkeys
FONT=lat9w-16
```

* Set Timezone
```
ln -s /usr/share/zoneinfo/Europe/Vienna /etc/localtime
```

* Set Clock
```
hwclock --systohc --utc
```

* Set Hostname
```
echo arch-e330 > /etc/hostname
```

#### Configure Wifi
* Install iw, wpa_supplicant and dialog
```
pacman -S iw wpa_supplicant dialog
```

* Connect to Wifi with wifi-menu
```
wifi-menu _wifi-adapter_
```

#### Create Ramdisk
```
mkinitcpio -p linux
```

#### Set Root Password
```
passwd
```

#### Install Bootloader (SysLinux)
```
pacman -S gptfdisk
pacman -S syslinux
syslinux-install_update -iam
```

* Make sure Syslinux is mounting the correct parition
```
nano /boot/syslinux/syslinux.cfg
--> change TIMEOUT to 10
```


#### Exit Installed System
```
exit
```

#### Unmount all Partitions
```
umount /mnt/home
umount /mnt
```

#### Reboot
```
reboot
```

#### Setup System

* Install git
```
pacman -S git
```

* Clone Install Script
```
git clone https://github.com/eiabea/aui.git
```

* Run Install Script
```
cd aui
./lilo
```

#### Post Installation

* Install packages
```
pacman -S wget
yaourt -S android-studio gnome-screenshot gnome-terminal-transparent net-tools
```

* Set LightDM Keymap
```
nano /etc/X11/xorg.conf.d/20-keyboard.conf
Section "InputClass"
    Identifier "keyboard"
    MatchIsKeyboard "yes"
    Option "XkbLayout" "de"
    Option "XkbVariant" "nodeadkeys"
EndSection
```

#### Settings
* Disable Touchpad
* Enable MiddleClick Scroll http://askubuntu.com/questions/2557/thinkpad-middle-button-scrolling
* Themes - Numix-Cinnamon
* Other Settings - Numix
* Power Management - When lid closed - Do nothing
* Keyboard - Keyboard Shortcuts - remove Bindings for Toggle Scale / Toggle Expo
* Keyboard - Keyboard Shortcuts - create Ctrl + Alt + L for bin/lockScreen
* Set background




