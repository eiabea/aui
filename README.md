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




=======
### Project only accepting patches
This project is not actively developed but *will* accept Pull Requests.

<h1 align="center">
  <a href=https://www.archlinux.org/>Archlinux</a> Ultimate Installer
</h1>
<h4 align="center">Installation & Configuration of archlinux has never been much easier!</h4>

<p align="center">
  <img src="https://img.shields.io/badge/Maintained%3F-Yes-green?style=for-the-badge">
  <img src="https://img.shields.io/github/license/helmuthdu/aui?style=for-the-badge">
  <img src="https://img.shields.io/github/issues/helmuthdu/aui?color=violet&style=for-the-badge">
  <img src="https://img.shields.io/github/stars/helmuthdu/aui?style=for-the-badge">
  <img src="https://img.shields.io/github/forks/helmuthdu/aui?color=teal&style=for-the-badge">
</p>

## Note
* You can first try it in a `VirtualMachine`

## Prerequisites

- A working internet connection
- Logged in as 'root'

## Obtaining The Repository
### With git
- Increase cowspace partition: `mount -o remount,size=2G /run/archiso/cowspace`
- Get list of packages and install git: `pacman -Sy git`
- Get the script: `git clone git://github.com/helmuthdu/aui`

### Without git
- Increase cowspace partition: `mount -o remount,size=2G /run/archiso/cowspace`
- Get the script: ` wget https://github.com/helmuthdu/aui/tarball/master -O - | tar xz`
    - an alternate URL (for less typing (github shorten)) is ` wget https://git.io/vS1GH -O - | tar xz`
    - an alternate URL (for less typing) is ` wget http://bit.ly/NoUPC6 -O - | tar xz`
    - super short `wget ow.ly/wnFgh -O aui.zip`

## How to use
- FIFO [System Base]: `cd aui ; ./fifo`
- LILO [The Rest]: `cd aui ; ./lilo`

## Features
### FIFO SCRIPT
- Configure keymap
- Select editor
- Automatic configure mirrorlist
- Create partition
- Format device
- Install system base
- Configure fstab
- Configure hostname
- Configure timezone
- Configure hardware clock
- Configure locale
- Configure mkinitcpio
- Install/Configure bootloader
- Configure mirrorlist
- Configure root password

### LILO SCRIPT
- Backup all modified files
- Install additional repositories
- Create and configure new user
- Install and configure sudo
- Automatic enable services in systemd
- Install an AUR Helper [trizen, yay...]
- Install Base System
- Install systemd
- Install Preload
- Install Zram
- Install Xorg
- Install GPU Drivers
- Install CUPS
- Install Additional wireless/bluetooth firmwares
- Ensuring access to GIT through a firewall
- Install DE or WM [Cinnamon, Enlightenment, FluxBox, GNOME, i3, KDE, LXDE, OpenBox, XFCE...]
- Install Developement tools [Vim, Emacs, Eclipse...]
- Install Office apps [LibreOffice, GNOME-Office, Latex...]
- Install System tools [Wine, Virtualbox, Grsync, Htop...]
- Install Graphics apps [Inkscape, Gimp, Blender, MComix...]
- Install Internet apps [Firefox, Google-Chrome, Jdownloader...]
- Install Multimedia apps [Rhythmbox, Clementine, Codecs...]
- Install Games [Desura, PlayOnLinux, Steam, Minecraft...]
- Install Fonts [Liberation, MS-Fonts, Google-webfonts...]
- Install and configure Web Servers
- And Many More...


## Thank helmuthdu
If you like my work, please consider a small Paypal donation at helmuthdu@gmail.com :)

## License :scroll:
This project is licenced under the GNU General Public License V3. For more information, see the `LICENSE` file or visit https://www.gnu.org/licenses/gpl-3.0.en.html.
