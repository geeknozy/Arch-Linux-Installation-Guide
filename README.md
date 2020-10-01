# Arch-Linux-Installation-Guide-2020-October (will be updated when ISO changes every month)

# modified and easy way to install arch linux - by geeknozy

This is a modified guide on how to install Arch Linux on UEFI enabled systems.

This guide is made in such a way that new users to arch linux may seem helpfull and can follow along the isntallation steps and can install Arch-Linux on thier systems.

# Warning : Any dataloss or system brick by using this guide I/geeknozy will/should not be held responsible. This guide will only provide easier steps to install Arch-Linux not the official way of doing things, so as to say : -> RTFM  -> Arch wiki is one of the greatest documentation ever.


Step 1: Go to https://archlinux.org

Step 2: On the right top corner click on download button  which will redirect you to the download page.

Step 3: Select the mirror closer to your country or the global mirror to download the ISO package.

Step 4: Note the md5/sha256sum (checksum) provided

Step 5: check the integrity of the downloaded ISO file with md5/sha256sum.

Step 6: Use CLI-DD(in Linux) / RUFUS-DD or Balena-Etcher to write the ISO to the USB-Stick (note the data in USB will be lost)

Step 7: Jump into your system BIOS select te boot to USB.

Step 8: Jump to Install media.

Step 9: you will be prompted with CLI as root@archiso

Step 9: Figure out which hard disk you are using by typing command fdisk -l

Step 10: select your HDD (Don't touch the USB) generally it will be /dev/sda

Step 11: type following command for partitioning after checking that you have selected the right HDD

 1. fdisk /dev/sda0
 
 Delete all file partition systems.
 
 2. type d (to delete partition)
 again d until you delete all the partitions
 
 Now create new partitions for the installations to create new partitions type 
 3. n
  ->hit enter for 0-128
  ->hit enter for 34-122142686 default(2048):
  -> now for rest EFI drive type +512M  (NOTE this will take 512MB as EFI partition and rest will be sdb so efi partition is sda)
  -> remove signature ? : yes
  -> change type of EFI partition to EFI.
  |
  |
  -> type L -> this will list all the partition types.
     select 1 (EFI partition)
  -> hit q to quit list 
  -> select 1
  -> type n
  1. default:
  2. default
  3. hit yes.
  
  Once done hit w to write to disk.
  
 Step 12: Now format drives
 
 -> mkfs.fat -F32 /dev/sda1   (Partition 1, This will be formatted with .fat type (Primary partition))
 -> mkfs.ext4 /dev/sda2       (Partition 2, This will be formatted wit ext4 file systems (Primary))
  
  
  # NOW WE ARE DONE WITH FIRST PART THAT IS FILE SYSTEM CREATION NOW WE DO THE INSTALLAION.
  (will update soon)
  Step 13 : Setup internet 
  
  Ethernet is prefered else follow my steps. 
  
 NOTE: Will update soon 
