src = https://www.youtube.com/watch?v=43dpS35Hzq8

---

Changing root is commonly done for recovering an unbootable system.  

- you need a bootable Arch Linux USB stick
- Boot the Arch Linux live medium
- find the partition you want to fix: `lsblk`
- mount that partition: `sudo mount /dev/nvme0n1p2 /mnt`
- change the apparent root directory for the broken file system: `sudo arch-chroot`
- reinstall the kernel: `sudo pacman -S linux`
- exit the chroot jail: `exit`
- unmount the partition: `umount /mnt`
- reboot the machine and unplug the USB stick: `reboot`
