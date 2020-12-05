# Arch-Linux-Installation-Guide (Will make changes when arch-wiki guide makes changes)

# modified and easy way to install arch linux - by geeknozy

This is a modified guide on how to install Arch Linux on UEFI enabled systems.

This guide is made in such a way that new users to arch linux may seem helpfull and can follow along the isntallation steps and can install Arch-Linux on thier systems.

# Warning : Any dataloss or system brick by using this guide I/geeknozy will/should not be held responsible. This guide will only provide easier steps to install Arch-Linux not the official way of doing things, so as to say : Read the official Arch wiki, it is one of the greatest documentation ever.


Step 1: Go to https://archlinux.org

Step 2: On the right top corner click on download button  which will redirect you to the download page.

Step 3: Select the mirror closer to your country or the global mirror to download the ISO package.

Step 4: Note the md5/sha256sum (checksum) provided

Step 5: check the integrity of the downloaded ISO file with md5/sha256sum.

Step 6: Use CLI-DD(in Linux) / RUFUS-DD or Balena-Etcher to write the ISO to the USB-Stick (note the data in USB will be lost)

Step 7: Jump into your system BIOS select te boot to USB.

Step 8: Jump to Install media.

Step 9: You will be prompted with CLI as root@archiso

Step 10: Follow this link for Partition : https://github.com/geeknozy/Disk-partition-guide-on-linux-for-installation

Step 11: Once done with partition scheme now mount the partitions

```mount /dev/sda2 /mnt``` -> mounting root partition to /mnt of the iso (all packages will be downloaded here).

```mkdir -p /mnt/boot/efi``` -> creating seperate efi directory to mount efi partition

```mount /dev/sda1 /mnt/boot/efi``` -> mounting efi partition 

Step 12: type ```lsblk``` -> you should see your mount points.

Step 13: Now downloading the base packages for the system

```pacstrap /mnt base base-devel linux linux-firmware nano intel-ucode```

NOTE: nano is my preferred editor you can also use other command line based like VIM editor.
NOTE: intel-ucode if you have intel processors or else if you have amd processor use amd-ucode.

This will take little time to download and install base packages.

Step 14: Now after getting cmd prompt. you have to generate fstab file. 

```genfstab -U /mnt >> /mnt/etc/fstab```

NOTE: -U is uuid for devices.

```cat /mnt/etc/fstab``` -> to see the generated fstab.

Step 15: Enter the chroot environment.

```arch-chroot /mnt``` -> now you have to see the command prompt change.

Step 16: Set Time

```timedatectl list-timezones | grep your-country-name```

Step 17: you have get your timezone as output.

```ln -sf /usr/share/zoneinfo/yourcountryname/zonename /etc/localtime```

Step 18: sync you hardware clock and system clock.

```hwclock --systohc```

Step 19: Generating your Locales (Languages and keyboards).

```nano /etc/locale.gen```
find for line with ``` #en_US.UTF-8 UTF-8 ``` or your locale 

uncomment that line by removeing # at the begening of the line.

save the file exit editor 

type - ```locale-gen```

type - ```nano /etc/locale.conf``` -> to add genrated locale to your conf file.
add these on the first line - ```LANG=en_US.UTF-8``` - save and exit.

Step 20: Configuring host system.

```nano /etc/hostname```
add your desired name for the system. - save and exit.

```nano /etc/hosts``` -> to configure localhost and internet.

add these lines after first 2 lines with # comment

```127.0.0.1     localhost```
```::1           localhost```
```127.0.1.1     hostname.localdomain     hostname```

NOTE: the hostname is the one you set with above hostname file.

Step 21: Give password for root user.

```passwd``` - set strong password and confirm.

Step 22: Downlaod grub boot loader.

```pacman -S grub efibootmgr networkmanager network-manager-applet linux-headers git reflector pulseaudio alsa-utils xdg-utils xdg-user-dirs```

accept defaults and download - install packages.

Step 23: Installing grub.

```grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=anyname```

after this generating then generate config for grub.

```grub-mkconfig -o /boot/grub/grub.cfg``` -> sending output of grub-mkconfig to the grub.cfg file

Step 24: Now Enable the services that are needed for the system.

```systemctl enable NetworkManager```

Step 25: Add different user (not root) NOTE: This is wheel group user to get sudo privileges.

```useradd -mG wheel username```

```passwd username```

Give strong password for new user.

Step 26: Add User for sudoers file.

```EDITOR=nano visudo```

Find the line named
```#wheel ALL=(ALL) ALL``` under wheel group.
uncommment that line - save and exit.

Step 27: Done with the base install now type 

```exit```

```umount -a``` - to unmount iso.
REBOOT
```reboot``` -> if you get greeted by grub then you successfully installed base ARCH_LINUX.

# We are done with the base install of Arch Linux without graphical user interface. GUI manual will be made soon. Happy saying 'BTW I use ARCH'.
