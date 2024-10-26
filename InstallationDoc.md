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

