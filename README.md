# Arch-Linux-Installation-Guide

--------------------------------------------------------------------------------------------------------------------------------<br />

##### modified and userfriendly way guide to install arch linux - by geeknozy <br />

##### Note : x86-64 architecture with UEFI enabled systems only.

This guide is made in such a way that new users to arch linux may seem helpfull and can follow along the isntallation steps and can install Arch-Linux on thier systems.<br />

#### Warning : Any data-loss or system brick by using this guide I/geeknozy will/should not be held responsible. This guide will only provide easier steps to install Arch-Linux not the official way of doing things, so as to say : Read the official Arch wiki, it is one of the greatest documentation ever.<br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 1: Go to https://archlinux.org <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 2: On the right top corner click on download button  which will redirect you to the download page.<br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 3: Select the mirror closer to your country or the global mirror to download the ISO package.<br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 4: Note the md5/sha256sum (checksum) provided <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 5: check the integrity of the downloaded ISO file with md5/sha256sum. <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 6: Use CLI-DD(in Linux) / RUFUS-DD or Balena-Etcher to write the ISO to the USB-Stick (note the data in USB will be lost) <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 7: Goto your system BIOS select te boot from USB (arch-installation-media). <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 8: Select installation media. <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 9: You will be prompted with CLI as ```root@archiso``` <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 10: Follow this link for Partition : https://github.com/geeknozy/Disk-partition-guide-on-linux-for-installation <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 11: Once done with partition scheme now mount the partitions <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

```mount /dev/sda2 /mnt``` -> mounting root partition to /mnt of the iso (all packages will be downloaded here). <br />

```mkdir -p /mnt/boot/efi``` -> creating seperate efi directory to mount efi partition <br />

```mount /dev/sda1 /mnt/boot/efi``` -> mounting efi partition <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 12: type ```lsblk``` -> you should see your mount points.<br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 13: Now downloading the base packages for the system.<br />

```pacstrap /mnt base base-devel linux linux-headers linux-firmware nano intel-ucode``` <br />

##### alternative option I prefer lts kernel with arch linux

```pacstrap /mnt base base-devel linux-lts linux-lts-headers linux-firmware nano intel-ucode``` <br />

#### poweruser/gaming oriented tweaked kernel

```pacstrap /mnt base base-devel linux-zen linux-zen-headers linux-firmware nano intel-ucode``` <br />

##### NOTE: nano is my preferred editor you can also use other command line based like VIM editor. <br />
##### NOTE: intel-ucode if you have intel processors or else if you have amd processor use amd-ucode. <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

This will take little time to download and install base packages based on your mirrors and internet speed. <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 14: Now after getting command prompt. you have to generate fstab file. <br />

```genfstab -U /mnt >> /mnt/etc/fstab``` <br />

##### NOTE: -U is uuid for devices (will be different for every hardware/disk. <br />

```cat /mnt/etc/fstab``` -> to see the generated fstab. <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 15: Enter the chroot environment. <br />

```arch-chroot /mnt``` -> now you have to see that your command prompt change. <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 16: Set Time <br />

```timedatectl list-timezones | grep your-country-name``` <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 17: you have get your timezone as output. <br />

```ln -sf /usr/share/zoneinfo/Region name/zonename /etc/localtime``` <br />

example : ```ln -sf /usr/share/zoneinfo/Asia/Kolkata /etc/localtime``` <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 18: sync you hardware clock and system clock. <br />

```hwclock --systohc``` <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 19: Generating your Locales (Languages and keyboards). <br />

```nano /etc/locale.gen``` <br />

find for line with ``` #en_US.UTF-8 UTF-8``` or your locale file <br /> 

uncomment that line by removeing # at the begening of the line. <br />

save the file (ctrl+o) exit editor (ctrl+x) note: on nano <br />

type - ```locale-gen``` <br />

type - ```nano /etc/locale.conf``` -> to add genrated locale to your conf file. <br />

add these on the first line <br />

```LANG=en_US.UTF-8``` - save and exit. <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 20: Configuring host system. <br />

```nano /etc/hostname``` <br />

add your desired name for the system. - save and exit. <br />

```nano /etc/hosts``` -> to configure localhost and internet. <br />

add these lines after first 2 lines with # comment <br />

```127.0.0.1     localhost```<br />
```::1           localhost```<br />
```127.0.1.1     hostname.localdomain     hostname```<br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Optional Step <br />

mkinitcpio <br />

```mkinitcpio -P``` <br /> 

wait for this to complete <br />

##### NOTE: the hostname is the one you set with above hostname file. <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 21: Give password for root user. <br />

```passwd``` - set strong password and confirm. <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 22: Downlaod grub boot loader. <br />

```pacman -S grub efibootmgr networkmanager network-manager-applet git pulseaudio alsa-utils xdg-utils xdg-user-dirs``` <br />

#### Optional (I use pipewire which is more mature than pulseaudio) <br />

```pacman -S grub efibootmgr networkmanager network-manager-applet git pipewire pipewire-pulse pipewire-alsa alsa-utils xdg-utils xdg-user-dirs``` <br />

Note: use wireplumber for media session service instead pipewire-media-session (personal preference) <br />

accept defaults and download - install packages. <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 23: Installing grub. <br />

```grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=anyname``` <br />

after this generating then generate config for grub. <br />

```grub-mkconfig -o /boot/grub/grub.cfg``` -> sending output of grub-mkconfig to the grub.cfg file <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 24: Now Enable the services that are needed for the system. <br />

```systemctl enable NetworkManager``` <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 25: Add different user (not root) NOTE: This is wheel group user to get sudo privileges. <br />

```useradd -mG wheel username``` <br />

```passwd username``` <br />

Give strong password for new user. <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 26: Add User for sudoers file. <br />

```EDITOR=nano visudo``` <br />

Find the line named <br />
```#wheel ALL=(ALL) ALL``` under wheel group. <br />
uncommment that line - save and exit. <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Step 27: Done with the base install now type <br />

```exit``` <br />

```umount -a``` - to unmount iso. <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

#### Reboot your computer <br />

```reboot``` -> if you get greeted by grub then you successfully installed base ARCH_LINUX. <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

##### We are done with the base install of Arch Linux without graphical user interface. Happy saying 'BTW I use ARCH'. <br />

##### [Link for installing Graphical user interface] : (https://github.com/geeknozy/Arch-Linux-GUI/) <br />

--------------------------------------------------------------------------------------------------------------------------------<br />

##### note : if you find any steps confusing or want to contribute or have any feed back <br />
contact me via <br />

[Email] (```geeknzoy.protonmail.com```) <br />

[Discord server] (```https://discord.gg/UkbVeqF3aE```) <br />
