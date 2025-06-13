---
title: "Installation and Configuration of Arch Linux"
date: 2024-12-19
categories: [Linux, Arch Linux]
tags: [Linux, Arch Linux, Configuration]
description: My Arch Linux installation and configuration process.
---

To be updated.

## Pre-installation

### Prepare an installation medium

Download the ISO file from the [Download](https://archlinux.org/download/) page and prepare a USB flash drive for installation.

### Boot the live environment

Select Arch Linux install medium and enter the installation environment.

### Connect to the internet

Make sure you have connected to the internet during the installation process.

### Verify the boot mode

```sh
cat /sys/firmware/efi/fw_platform_size
```

### Partition the disk

```sh
fdisk -l
cfdisk /dev/the_disk_to_be_partitioned
```

### Format the partitions

```sh
# efi
mkfs.fat -F 32 /dev/efi_system_partition

# swap
mkswap /dev/swap_partition

# root and home
mkfs.btrfs /dev/root_partition -f
mount /dev/root_partition /mnt
btrfs subvolume create /mnt/@
btrfs subvolume create /mnt/@home
umount /dev/root_partition
```

### Mount the file systems

```sh
# root and home
mount /dev/root_partition /mnt -o subvol=@
mount /dev/root_partition /mnt/home -o subvol=@home --mkdir

# efi
mount /dev/efi_system_partition /mnt/boot/efi --mkdir

# swap
swapon /dev/swap_partition
```

## Installation

### Select the mirrors

```sh
vim /etc/pacman.d/mirrorlist

# add
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch

# update
pacman -Sy
```

### Install essential packages

```sh
pacstrap /mnt base base-devel linux linux-firmware linux-headers networkmanager dhcpcd openssh git neovim intel-ucode ntfs-3g btrfs-progs
```

## Configure the system

### Fstab

Generate an fstab file:

```sh
genfstab -U /mnt >> /mnt/etc/fstab
```

### Chroot

Change root into the new system:

```sh
arch-chroot /mnt
```

### Time

Set the time zone and run hwclock to generate `/etc/adjtime`:

```sh
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
hwclock --systohc
```

To prevent clock drift and ensure accurate time, set up time synchronization using a Network Time Protocol(NTP) client such as systemd-timesyncd.

### Localization

Edit /`etc/locale.gen` and uncomment `en_US.UTF-8 UTF-8` and other needed UTF-8 locales. Generate the locales by running:

```sh
nvim /etc/locale.gen

# uncomment
en_US.UTF-8
zh_CN.UTF-8

locale-gen
echo LANG=en_US.UTF-8 > /etc/locale.conf
```

### Network hostname configuration

Create the hostname file:

```sh
nvim /etc/hostname
```

### Initramfs

Creating a new initramfs is usually not required, because mkinitcpio was run on installation of the kernel package with pacstrap.

For LVM, system encryption or RAID, modify `mkinitcpio.conf` and recreate the initramfs image:

```sh
mkinitcpio -P
```