# Beginnings
1. Go to archlinux.org/downloads to grab the iso
2. In this case I happened to download the torrent file
3. I used the Oracle Virtual box manager to attatch to my ISO

### Setting up the VM
1. I allocated exactly 2 gb (2048mb) of memmory
2. I also allocate 2 cores from my cpu, as that is whats recommended for general use.
3. I went ahead at dedicated 20gb of virtual storage to the VM (this will allow me more freedom in choosing my DE later)

# Booting up Arch Linux
After getting the VM running now comes the list of infinite setup
### Wi-FI
1. I run the inital iwctl as instructed by the wiki
2. However it appears that Virtual Box emulates an ethernet connection automatically
3. I tested this by pinging archlinux.org and recieved and transmitted 70 packts each !
4. on to the next steps!
### Updating the System Clock
just to make things easier and prettier later I updated the System Clock by using
  "timedatectl set-ntp true"
### Disk Partitions
1. in order to partition the disk, I need to know the name of the disk. This can be done by using "lsblk"
showing me that the disk containg the allocated 20gb is called "sda"
2. To start creating partitions I use "cfdisk /dev/sda" and when prompted I attatch the "gpt" label type
3. The first partition I create will be for the root, this is file path /dev/sda1 and is about 5.5gb
4. I guess for now I'll just stick to the one partition but I might come back
5. I write a partition table to the disk and exit cfdisk
6. I format the new partition using the command "mkfs.ext4 /dev/sda1"
7. Last step is mounting the partition using "mount /dev/sda1 /mnt"

### OPTIONAL FONT CHANGE
"setfont ter-132b"

# Installing the Base System
**to install the base system I used** "pacstrap /mnt base linux linux-firmware"
-my next goal is generate an fstab file for some arbitraty reason the wiki won't tell me :D

1. I generate an fstab file by using "genfstab -U /mnt >> /mnt/etc/fstab"
2. Time to Chroot into the system by using "arch-chroot /mnt"
3. Root mode activated B)
### Time Zone & Localization
1. Now in root mode, I can set the time zone by using "ln -sf /usr/share/zoneinfo/America/Tulsa /etc/localtime"
2. Then I can use "hwclock --systohc" to synchronize the hardware clock.
3. I need to make some file adjustments with a text edditor so I decided to install nano by using "pacman -Sy nano" (I dont like Vim)
4. Using nano I open the locale file "nano /etc/locale.gen" because I need to uncomment out my prefered localization
5. Preffered localization is "en_US.UTF-8 UTF-8"
6. When running "locale-gen" it shows our "en_US.UTF-8 UTF-8" localle so we know it worked and is saved.
7. Now in order to set the system default locale we ues "echo "LANG=en_US.UTF-8" > /etc/locale.conf"
8. We can cat the /etc/locale.conf filepath to verify that the command worked :D
9. localization and time done :D

### Network configuration
1. First step is to set a host name to be used for identification for a given network I guess
2. I will create the host name by using "echo "awesome-a-tron" > /etc/hostname"
3. Now I need to configure the /etc/hosts file so I can map the hostname to the local Ip addresses :D
4. This was done by using "echo -e "127.0.0.1\tlocalhost\n::1\tlocalhost\n127.0.1.1\tarchvm.localdomain awesome-a-tron" >> /etc/hosts"
5. That would have been way easier to type as three seperate commands ngl
6. but if I made a mistake I can re-edit it with nano!

## Final Phases
1. I need to set the root password by using "passwd"
2. I chose to make the root password "wizardFart11"
### Installing essential packages and configuring boot loader
1. For the network manager I used "pacman -S networkmanager sudo vim" and enable it using "systemctl enable NetworkManager"
2. For the boot loader I installed grub using "pacman -S grub"
3. Tried to install grub using "grub-install --target=i386-pc /dev/sda" but realized I don't have a boot partition so the install fails
4. created a BIOS boot partition thats probably too large (200mb) but it'll do the job
5. now "grub-install --target=i386-pc /dev/sda" works :D
6. made quick little config file with "grub-mkconfig -o /boot/grub/grub.cfg"
7. exiting and rebooting to begin the fun stuff >:)

# Customization
the OS is now functional and I can sign into the root user account using the password I made earlier

### Adding Users
1. I created users codi, justin, and oliver "useradd -m -G wheel 'name' "
2. I set codi and justins passwords to "GraceHopper1906"
3. I used "passwd --expire justin" to require a password change for just apon next login, same for codi
4. "EDITOR=vim visudo" will allow me to edit the sudoers file to give members of wheel sudo perms (aka justin and codi)
5. This was a little challenging because i'm not used to editing with VIM so I had to look up how to even edit the text and then how to save and quit
### Adding a DE (desktop enviroment)
I chose to use LXQt because it was the smallest
1. 3 commands for that
     1. "pacman -S xorg-server xorg-xinit lxqt"
     2. "pacman -S sddm"
     3. "systemctl enable sddm"

I ran into a problem using the first command, it turns out the lxqt mirrors are just completly broken
so I instead opted to use a different DE XFCE

the steps I used were as follows
  1.pacman -S xfce4 - installs xfce
  2.pacman -S lightdm lightdm-gtk-greeter - creates a login screen
  3.systemctl enable lightdm - makes the login screen apear at boot
   
