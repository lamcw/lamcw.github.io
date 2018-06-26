---
layout: post
title: "XPS 13 Arch Linux Installation"
date: 2017-11-17 00:00:00
categories:
- tutorial
- XPS 13
author: lamcw
---

The purpose of this gist is to install vanilla [Arch Linux](https://www.archlinux.org/) to an USB storage device for XPS 13 (9350). This also act as a "note-to-self" in case I accidentally nuke my system and need a reinstallation. Most of the configuration here can actually be used in any UEFI-enabled pc, but I have added some detail points to optimize settings for my lovely XPS 13. I wrote this from my memory so there might be something that I missed. I will update this gist once I remember it.


## What you need
* Dell XPS 13 (9350) ...well obviouly
* An USB stick/HDD/SSD
* A bootable drive containing a live arch linux image, which can be downloaded [here](https://www.archlinux.org/download/)
* Internet connection

## Configuration

Let's begin our installation after you prepared all the stuff.

### Disabling Secure Boot

The computer should have "Secure Boot" enabled by deafult. Restart the computer and press F12 during boot and disable Secure Boot. Then boot with the bootable drive you have prepared.

### Connecting to Internet

If successfully booted, you will be greeted with a console interface. We need to set up an Internet connection first.

If you have **wired** connection, `ping archlinux.org` to check for internet connection.

For **wireless** connection, type in `wifi-menu` and follow the on-screen intructions.

### System clock
Update system clock
```bash
timedatectl set-ntp true
timedatectl status		#check service status
```
### Disk partition

You will need at least **2** partition for Arch Linux to work with: an **EFI System Partition(ESP)** partition and a **root** partition. If you have an older machine with not much RAM, you can also add a swap partition for better performance. To boot UEFI on XPS 13, you need a GPT partition scheme on you USB key.

To list out device letter (usually in form of */dev/sdX*) of the USB key, `lsblk` or `fdisk -l`.

After you acquired the device letter, wipe the drive with zero.
```bash
dd if=/dev/zero of=/dev/sdX bs=1k count=2048
```
Create partition for EPS **(Remember to replace sdX with your device letter)**
```bash
parted /dev/sdX
(parted) mklabel gpt												#create a gpt partition table
(parted) mkpart ESP fat32 1MiB 201MiB				#200MB should be enough for ESP
(parted) set 1 boot on
(parted) mkpart primary ext4 201MiB 50GiB		#Root partition. Change whatever the size you want.
(parted) quit
```
`lsblk` again to check if the partitions has been created.

### Formatting the partition
Now format the partitions we just created
```bash
mkfs.fat -F32 /dev/sdX1
mkfs.ext4 -O "^has_journal" /dev/sdX2
```
The ^has_journal option [reduce disk reads/writes](https://wiki.archlinux.org/index.php/Improving_performance#Reduce_disk_reads.2Fwrites) by disabling journal writing to the disk, thus reducing wear on the storage device, especially useful on USB stick/SSD. See also [here](https://wiki.archlinux.org/index.php/Installing_Arch_Linux_on_a_USB_key#Installation_tweaks).

### Mount the system
First mount the root partition to /mnt
```bash
mount /dev/sdX2 /mnt
```
...then the ESP to /boot
```bash
mkdir /mnt/boot
mount /dev/sdX1 /mnt/boot
```
## Installation
### Selecting the mirrors
We are ready to download the base system to the USB key. But before that, you may want to select a mirror closest to you first.
```bash
vi /etc/pacman.d/mirrorlist
```
Cut and paste your preferred mirror address to the top of the list. `dd` to cut the whole line, `p` to paste.

### Pacstrap
```bash
pacstrap /mnt base wpa_supplicant dialog
```
## Configuring the system
### Fstab
Generate an fstab file (use `-U` to define by UUID)
```bash
genfstab -U -p /mnt >> /mnt/etc/fstab
```
Check the resulting file in /mnt/etc/fstab afterwards, and edit it in case of errors.

Now chroot into the system
```bash
arch-chroot /mnt
```
### Create the initial RAM disk
```bash
vi /etc/mkinitcpio.conf
```
Press `i` to enter INSERT mode. Navigate to the line HOOKS=... , then move `block` hook to the hooks array right after `udev`. Press `Esc` to exit INSERT mode and enter `:wq` to "write file and quit".
Then
```bash
mkinitcpio -p linux
```
### Time zone
Set your time zone, for example:
```bash
ln -sf /usr/share/zoneinfo/Asia/Hong_Kong /etc/localtime
hwclock --systohc #Set UTC time
```

### Locale
```bash
vi /etc/locale.gen
```
Uncomment `en_US.UTF-8 UTF-8` and other needed localizations. Generate with 
```bash
locale-gen
```

### Hostname
```bash
echo lcwarch > /etc/hostname  #use any name you want
passwd  #set password for root user
```

## Bootloader
If anything goes well, you can then install booloader of your choice. Here we use [systemd-boot](https://wiki.archlinux.org/index.php/systemd-boot) for its ease of installation. If you use a bootloader other than systemd-boot, please make sure the bootloader support UEFI boot. For the list of supported bootloader, visit https://wiki.archlinux.org/index.php/Category:Boot_loaders.

To install the bootloader (systemd-boot)
```bash
bootctl --path=/boot install
vi /boot/loader/loader.conf
```
Configure the bootloader like this
```
default  arch
timeout  2
editor   0
```
Find the root partition UUID with
```bash
blkid -s PARTUUID -o value /dev/sdX2
```
Then append these lines to `/boot/loader/entries/arch.conf`
```
title          Arch Linux
linux          /vmlinuz-linux
initrd         /initramfs-linux.img
options        root=PARTUUID=THE_PARTUUID_YOU_RETRIEVED_ABOVE rw
```
Update the bootloader
```
bootctl update
```
Unmount all partitions
```
exit
umount -R /mnt
```
Now that we have set up bootloader and base system. You should be able to boot the system without the live OS. Try `reboot` with the bootable drive unplugged. If all goes well, proceed to the next step.

## Post installation
### Add user
```
pacman -S sudo
visudo
groupadd sudo
useradd -G sudo -m lcw  #change lcw to your preferred username
passwd lcw
```

### Install packages and desktop environment
Pick a [desktop environment](https://wiki.archlinux.org/index.php/desktop_environment) and install with pacman. I recommend GNOME as it has great support for high DPI screen, which XPS 13 has. If you want a lightweight DE, I recommend LXDE.

Here we install gnome, drivers, network, vim, fonts, browser and touchpad support.
```
pacman -S gnome vim iw wpa_supplicant dialog network-manager-applet networkmanager xf86-input-libinput xorg-xinit chromium zip unzip ttf-dejavu mesa-libgl
```
To start GNOME and network service after reboot
```
systemctl enable NetworkManager.service
systemctl enable gdm.service
reboot
```
Voilà! You now have Arch Linux installed on your USB key! Have fun tweaking your system!

## TODO
- [ ] Add XPS 13 specific settings

## Links

This installation guide is largely based on 
* https://wiki.archlinux.org/index.php/Installation_guide
* https://wiki.archlinux.org/index.php/Installing_Arch_Linux_on_a_USB_key
* https://wiki.archlinux.org/index.php/Dell_XPS_13_(9350)<Paste>
