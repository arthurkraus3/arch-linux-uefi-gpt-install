# arch-linux-uefi-gpt-install
the process I went through for direct booting arch linux off my Thinkpad X1 Yoga (UEFI/GPT drive)
FOR UEFI BOOTING GPT
Thinkpad X1 Yoga 1st Gen
IN BIOS: ENABLE INTEL VIRTUAL TECHNOLOGY BUT NOT THE VT-D 


#### connect to internet using wifi-menu or just verifying that ping archlinux.org works

##### Get your mirror list setup with local priority
> nano /etc/pacman.d/mirrolist 

##### Use gdisk to partition the drive
> gdisk /dev/sda

> o 

> n <blank> <blank> +512MB<enter> EF00<enter>
	
> n <blank>*4
	
> w

> y

##### Format your drives
> mkfs.vfat /dev/sda1 (Don't have to but I did)

> mkfs.ext4 /dev/sda2

##### Mounting
>mount /dev/sda2 /mnt

>mkdir /mnt/boot

>mount /dev/sda1 /mnt/boot

##### Pacstrap
>pacstrap -i /mnt base base-devel

##### Access mount
>arch-chroot /mnt

##### Installing Bootloader
>bootctl install

>cd /boot
##### if vmlinuz-linux is there then proceed to next step (if not troubleshoot why it's not)
  >cd loader
  
  >pacman -S nano


##### Loader.conf
>nano loader.conf

  > default arch
  
  > timeout 4


##### Entries 
>cd entries 

	>nano arch.conf
	"
		title Arch Linux
		linux /vmlinuz-linux
		initrd /initramfs-linux.img
		options root=/dev/sda2 rw
	"

> exit

> umount -R /mnt

> reboot

#### after you boot into the system you'll want to set up all the essentials, such as the passwords and other such things

