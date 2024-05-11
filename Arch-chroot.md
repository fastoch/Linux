src = https://www.youtube.com/watch?v=43dpS35Hzq8

---

Changing root is commonly done for recovering an unbootable system.  

- you need a bootable Arch Linux USB stick
- Boot the Arch Linux live medium
- find the disk on which the Linux filesystem is installed: `fdisk -l`
- mount that disk: `mount /dev/nvme0n1p2 /mnt`
- also mount the boot partition: `mount /dev/nvme0n1p1 /mnt/boot`
- change the apparent root directory: `arch-chroot /mnt`

Now, you need to fix whatever prevents the system from booting.  
The following command will show you all logs from the last boot: `journalctl -b` (errors will be highlighted)  
In our example, we'll simply reinstall the kernel.

- reinstall the kernel: `pacman -S linux`
- exit the chroot jail: `exit`
- unmount the partitions: `umount /mnt/boot` and `umount /mnt`
- reboot and unplug the USB stick: `reboot`

---
EOF
