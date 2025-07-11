# Arch-Linux-Installation-Guide 
#### without any automated installation script

#### userfriendly guide to install arch linux - by geeknozy <br />

#### Note : This guide is for x86-64 architecture with UEFI enabled systems only. <br />

##### This guide is made in such a way that users may find it helpfull / userfriendly and can follow along with the installation steps and can install Arch-Linux on thier systems.<br />

#### Warning : Any data-loss or system brick by using this guide I / geeknozy will / should not be held responsible. This guide will only provide confusion free steps to install Arch-Linux and might differ what is shown in other tutorials, so as to say : Read the official Arch wiki, it is one of the greatest documentation ever.<br />
___
#### Step 1: Go to https://archlinux.org <br />
___
#### Step 2: On the right top corner click on download button  which will redirect you to the download page.<br />
___
#### Step 3: Select the mirror closer to your country or the global mirror to download the ISO package.<br />
___
#### Step 4: Note the md5/sha256sum (checksum) provided <br />
___
#### Step 5: check the integrity of the downloaded ISO file with md5/sha256sum. <br />
___
#### Step 6: Use CLI-DD(in Linux) / RUFUS-DD or Balena-Etcher to write the ISO to the USB-Stick (note the data in USB will be lost) <br />
___
- > NOTE: you might have to disable secure boot inorder to boot into arch-live USB
#### Step 7: Goto your system BIOS select te boot from USB (arch-installation-media). <br />
___
#### Step 8: Select installation media. <br />
___
#### Step 9: You will be prompted with CLI as ```root@archiso``` <br />
___
#### Step 10: Follow this link for Partition : https://github.com/geeknozy/disk-partition-guide-on-linux-for-installation <br />
___
#### Step 11: Once done with partition scheme now mount the partitions <br />
> mounting root partition to /mnt of the iso (all packages will be downloaded here). <br />
> here in below mount command the ```X``` in sdX is the letter of your partition depending on your h/w eg : ```sda or sdb or sdc```
```
mount /dev/sdX2 /mnt
```
> creating seperate efi directory to mount efi partition <br />
```
mkdir -p /mnt/boot/efi
``` 
> mounting efi partition <br />
```
mount /dev/sdX1 /mnt/boot/efi
```
___
#### Step 12: type ```lsblk``` -> you should see your mount points.<br />
___
#### Step 13: Now downloading the base packages for the system.<br />
```
pacstrap -K /mnt base base-devel linux linux-headers linux-firmware nano amd-ucode
```
##### NOTE: nano is my preferred editor you can also use other command line based like VIM editor. <br />
##### NOTE: intel-ucode if you have intel processors or else if you have amd processor use amd-ucode. <br />

> This will take some time to download and install base, kernel and included packages based on mirrors / server speed and your internet speed. <br />
___
#### Step 14: Now after getting command prompt. you have to generate fstab file. <br />

```
genfstab -U /mnt >> /mnt/etc/fstab
```

##### NOTE: -U is uuid for devices (will be different for every hardware/disk). <br />
> type below command to check the generated fstab
```
cat /mnt/etc/fstab
```
___

#### Step 15: Enter the chroot environment. <br />
> now you have to see that your command prompt change. <br />
```
arch-chroot /mnt
```
___
#### Step 16: Set Time <br />

> get your timezone if you are not aware how, using below command
```
timedatectl list-timezones | grep your-country-name or timezone name
```
___
#### Step 17: Set timezone. <br />
> using below command get your time zone here the ```YourRegionName``` might be your country name or region depending on where you are
> ```zonename``` will be your City or your locality
```
ln -sf /usr/share/zoneinfo/YourRegionName/zonename /etc/localtime
``` 
> for example below: 
```
ln -sf /usr/share/zoneinfo/Asia/Kolkata /etc/localtime
```
___
#### Step 18: sync you hardware clock and system clock. <br />

```
hwclock --systohc
```
___

#### Step 19: Generating your Locales (Languages and keyboards). <br />

```
nano /etc/locale.gen
```

- > find for line with ```#en_US.UTF-8 UTF-8``` (this is for english) or your locale format <br /> 

- > uncomment that line by removing ```#``` at the begening of the line. <br />

- >  save the file (ctrl+o) and exit nano editor (ctrl+x) <br />

- > type below command to generate your locale
```
locale-gen
```
> once executed type below command to add genrated locale to your conf file. <br />
```
nano /etc/locale.conf
```
> add your locale formate to the config file <br/>

```
LANG=en_US.UTF-8
```
> save and exit the file <br />
___

#### Step 20: Configuring host system. <br />
> type below command to set host name <br/>
```
nano /etc/hostname
```
> add your desired name for the system and save and exit file. <br />

> next to configure localhost and internet.
```
nano /etc/hosts
```
- add these lines after first two heading lines <br />
```
127.0.0.1     localhost
::1           localhost
127.0.1.1     hostname.localdomain     hostname
```
##### NOTE: the hostname is the one you set with above hostname file. <br />
___
#### Optional Step <br />

- mkinitcpio is a Bash script used to create an initial ramdisk environment<br />

```
mkinitcpio -P
``` 
> wait for mkinitcpio execution to complete <br />
___
#### Step 21: Give password for root user. <br />
- this is to set root user password so create strong password and confirm. <br />
```
passwd
```
___
#### Step 22: Downlaod grub boot loader. <br />
```
pacman -S grub efibootmgr networkmanager network-manager-applet git pulseaudio alsa-utils
```

#### alternative if you prefer pipewire which is more mature than pulseaudio <br />
```
pacman -S grub efibootmgr networkmanager git pipewire pipewire-pulse pipewire-alsa alsa-utils
```
> Note: use wireplumber for media session service instead pipewire-media-session (personal preference) <br />
> for other packages accept defaults and download - install packages. <br />
___
#### Step 23: Installing grub. <br />
```
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=arch
```
> once the grub is installed you should check that no erros are reported which will be shown after execution once done then generate config for grub by sending output of grub-mkconfig to the grub.cfg file using below command <br />

```
grub-mkconfig -o /boot/grub/grub.cfg
```
___
#### Step 24: Now Enable the services that are needed for the system. <br />
```
systemctl enable NetworkManager.service
```
___
#### Step 25: Add different user (not root) NOTE: This is wheel group user to get sudo privileges. <br />
```
useradd -mG wheel username
```
> NOTE: here username can be your choice 

- after the above command type below command to set password to created user
```
passwd username
```
> NOTE: here username is the user name which you have given above while creating user
___
#### Step 26: Add User for sudoers file. <br />
```
EDITOR=nano visudo
```
- Find the line named under wheel group. <br />
```
#wheel ALL=(ALL) ALL
```
> uncommment that line (remove # symbol) - save and exit. <br />
___
#### Step 27: Done with the base install now type <br />
```
exit
```
- after exit command type below command to unmount mounted partitions for safer reboot.
```
umount -R /mnt
```
___
#### Reboot your computer <br />
```
reboot
```
- if you get greeted by grub then you successfully installed base Arch Linux. <br />

___

##### We are done with the base install of Arch Linux without graphical user interface. Happy saying 'BTW I use ARCH'. <br />

##### [Link for installing Graphical user interface](https://github.com/geeknozy/Arch-Linux-GUI/) <br />

___

###### create a pull request if you have any issues 
___
