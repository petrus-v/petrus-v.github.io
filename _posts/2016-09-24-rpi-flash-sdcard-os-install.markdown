---
layout:     post
name:       "Setup hyptiot on RPi"
title:      "How do I setup my Rpi Os?"
date:       "2016-09-24"
author:     "Pierre Verkest"
header-img: "img/bash.png"
---

> **Warning :** As I'll use this post to setup my own RPIs this post may
> change with my running projects!

The goal of this post is to keep trace how I setuped my on RPI from
my Debian Jessie.

First you will needs to choose an Operating System that fit your own
requirements.

Let's list some of my requirements:

* Should be able to run docker (docker swarm)
* Stable enough with a great community (I don't want to battle against
  the operating system)
* Lightweight distribution
* Able to apply [rtk patch](
  https://rt.wiki.kernel.org/index.php/Main_Page)

So I've found 2 distributions that would fit that list [ArchLinux ARM](
https://archlinuxarm.org) which is based on [ArchLinux](
https://archlinux.org) and also [Hypriot](http://blog.hypriot.com/about)
which is a minimal Debian-based operating systems that is optimized to
run Docker.

There are some default configuration that I dislike in Hypriot (like
define a default user `pirate`) however those choices the team made to
build this distributions makes things easy to start and do some Proof Of
Concept (POC) without worries that much with initial setup. So let's
start with Hypriot that offer a great tool ready to use. Later when I
would like more security and mastering all the settings I may switch
on an other distribution like ArchLinux ARM.


## Install hypriot on RPI3

You can follow [this post to install Hypriot on your RaspBerry](
http://blog.hypriot.com/getting-started-with-docker-and-linux-on-the-raspberry-pi/)

I like to follow that path,

* Get the distribution:

```bash
# Choose a version to use on http://blog.hypriot.com/downloads/
$ export version=v1.0.0
# Download the last Hypriot Docker Image for Raspberry Pi
$ wget https://downloads.hypriot.com/hypriotos-rpi-$version.img.zip
# Get the checksum 
$ wget https://downloads.hypriot.com/hypriotos-rpi-$version.img.zip.sha256
# Make sure image is completly download
$ sha256sum -c hypriotos-rpi-$version.img.zip.sha256
# unzip that image
$ unzip hypriotos-rpi-$version.img.zip
```

* Prepare your sdcard

Insert the sdcard into an available slot, to detect where the device
is connected you can use `lsblk`:

```bash
$ lsblk 
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 953,9G  0 disk 
├─sda1   8:1    0   512M  0 part /boot/efi
├─sda2   8:2    0 797,1G  0 part /
└─sda3   8:3    0 156,3G  0 part [SWAP]
sdb      8:16   1  14,9G  0 disk 
└─sdb1   8:17   1  14,9G  0 part /media/usb0
```

here we are: `/sdb`! If you have multiple partition you may have
multiple `sdb1` / `sdb2` ...

* unmount if there are some mounted partition

If like on the above lsblk result you have mountpoints, make sure to
unount all partition before to continue

```bash
$ sudo umount /media/usb0 
$ lsblk 
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 953,9G  0 disk 
├─sda1   8:1    0   512M  0 part /boot/efi
├─sda2   8:2    0 797,1G  0 part /
└─sda3   8:3    0 156,3G  0 part [SWAP]
sdb      8:16   1  14,9G  0 disk 
└─sdb1   8:17   1  14,9G  0 part 
```

* Format sd card

This step is not necessary as long we are using `dd` on the
whole device, I let it there for memories. Also I like to make my sdcard
clean as if it was a new one before start!

```bash
# Start fdisk to partition the SD card
#    Type o. This will clear out any partitions on the drive.
#    Type p to list partitions. There should be no partitions left.
#    Type n, then p for primary, 1 for the first partition on the drive,
#            press ENTER to accept the default first sector,
#            then type ENTER for the last sector.
#    Type t, then c to set the first partition to type W95 FAT32 (LBA).
#    Write the partition table and exit by typing w.me  me  
$ sudo fdisk /dev/sdb

Bienvenue dans fdisk (util-linux 2.25.2).
Les modifications resteront en mémoire jusqu'à écriture.
Soyez prudent avant d'utiliser la commande d'écriture.


Commande (m pour l'aide) : o
Created a new DOS disklabel with disk identifier 0xd0bd8550.

Commande (m pour l'aide) : p
Disque /dev/sdb : 14,9 GiB, 15931539456 octets, 31116288 secteurs
Unités : secteur de 1 × 512 = 512 octets
Taille de secteur (logique / physique) : 512 octets / 512 octets
taille d'E/S (minimale / optimale) : 512 octets / 512 octets
Type d'étiquette de disque : dos
Identifiant de disque : 0xd0bd8550



Commande (m pour l'aide) : n
Type de partition
   p   primaire (0 primaire, 0 étendue, 4 libre)
   e   étendue (conteneur pour partitions logiques)
Sélectionnez (p par défaut) : p
Numéro de partition (1-4, 1 par défaut) : 1
Premier secteur (2048-31116287, 2048 par défaut) : 
Dernier secteur, +secteurs ou +taille{K,M,G,T,P} (2048-31116287, 31116287 par défaut) : 

Une nouvelle partition 1 de type « Linux » et de taille 14,9 GiB a été créée.

Commande (m pour l'aide) : t
Partition 1 sélectionnée
Code Hexa (taper L pour afficher tous les codes) :c
Si vous avez créé ou modifié une partition DOS 6.x, veuillez consulter la documentation de fdisk pour de plus amples renseignements.
Type de partition « Linux » modifié en « W95 FAT32 (LBA) ».

Commande (m pour l'aide) : w
La table de partitions a été altérée.
Appel d'ioctl() pour relire la table de partitions.
Synchronisation des disques.

$ sudo fdisk -l /dev/sdb

Disque /dev/sdb : 14,9 GiB, 15931539456 octets, 31116288 secteurs
Unités : secteur de 1 × 512 = 512 octets
Taille de secteur (logique / physique) : 512 octets / 512 octets
taille d'E/S (minimale / optimale) : 512 octets / 512 octets
Type d'étiquette de disque : dos
Identifiant de disque : 0xd0bd8550

Device     Boot Start      End  Sectors  Size Id Type
/dev/sdb1        2048 31116287 31114240 14,9G  c W95 FAT32 (LBA)
```

* Copy hypriot image on the sdcard

> **Warning** Of course adapt the ``of=`` parameter before running this
> command, you may damage the wrong device!...

```bash
# this copy can take few minutes
$ sudo dd if=hypriotos-rpi-$version.img of=/dev/sdxyz bs=1M
# Make sure all bytes are written on the sdcard bu flushing the file
# system buffer
$ sync

```

notice is you run fdisk again there are two partition, the smallest is
the boot partition, the other is the operating system:

```bash
$ sudo fdisk -l /dev/sdb

Disque /dev/sdb : 14,9 GiB, 15931539456 octets, 31116288 secteurs
Unités : secteur de 1 × 512 = 512 octets
Taille de secteur (logique / physique) : 512 octets / 512 octets
taille d'E/S (minimale / optimale) : 512 octets / 512 octets
Type d'étiquette de disque : dos
Identifiant de disque : 0x00000000

Device     Boot  Start     End Sectors  Size Id Type
/dev/sdb1         2048  133119  131072   64M  c W95 FAT32 (LBA)
/dev/sdb2       133120 2047998 1914879  935M 83 Linux
```

``/dev/sdb2`` looks so tiny why it doesn't use the whole place?

I guess this partition is resize at the first boot:

```bash
pverkest@petrus-v:~/raspberryPi/00_archlinux-vs-hypriot/hypriot$ sudo fdisk -l /dev/sdb

Disque /dev/sdb : 14,9 GiB, 15931539456 octets, 31116288 secteurs
Unités : secteur de 1 × 512 = 512 octets
Taille de secteur (logique / physique) : 512 octets / 512 octets
taille d'E/S (minimale / optimale) : 512 octets / 512 octets
Type d'étiquette de disque : dos
Identifiant de disque : 0x00000000

Device     Boot  Start      End  Sectors  Size Id Type
/dev/sdb1         2048   133119   131072   64M  c W95 FAT32 (LBA)
/dev/sdb2       133120 31116287 30983168 14,8G 83 Linux
```

* Connect to Hypriot

You can connect to you freshly RPI using a keyboard and a screen or I do
prefer connect my RPi to the network using a wierd cable and connect 
through ssh [to setup the wifi](
{% post_url 2016-09-24-rpi-wifi-setup %}).

There are several ways to found the affected address IP:

    * connect to your router where the dhcp is running
    * using nmap ``nmap -sP 192.168.1.10/24``
    * using arp ``sudo arp -a``
    * ...

So to connect to hypriot you have following paameters:

* let's says the affected ip address is ``192.168.1.28``
* Hypriot default user: ``pirate``
* pirate default password: ``hypriot``

```bash
$ ssh pirate@192.168.1.28
pirate@192.168.1.28's password: 

HypriotOS (Debian GNU/Linux 8)

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri Sep 16 22:38:15 2016 from 192.168.1.10
HypriotOS/armv7: pirate@black-pearl in ~
$ 
```

that's it!

# install Arch linux ARM on RPI3

At the moment I only leave this [link to archlinux.org documentation](
https://archlinuxarm.org/platforms/armv8/broadcom/raspberry-pi-3)