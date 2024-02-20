Backup and Restore Your Linux System with rsync  
https://www.youtube.com/watch?v=oS5uH0mzMTg&t=9s  

## Backup

**rsync** is a command line tool available on any Linux system.  
It's a universal way to backup files on Linux.  

The idea is to backup our entire system to a USB flash drive.  
We also need a live USB to restore the system, in case it doesn't boot anymore.

It's very important to use some Linux file system on your destination USB drive, because   
if you use FAT file system, rsync won't be able to copy all the files attributes.  
Format your usb drive with ext4 for example.

Once your usb drive is ready, cd to your home folder and run the following cmd:
```
sudo rsync -aAXv --delete --dry-run --exclude='/dev/*' --exclude='/sys/*' --exclude='/proc/*' --exclude='/tmp/*' --exclude='/run/*' --exclude='/mnt/*' --exclude='/media/*' --exclude="swapfile" --exclude='.cache' --exclude='Downloads' --exclude='lost+found' /source /destination
```
- -a => archive mode
- -A => preserve ACLs (Access Control Lists)
- -X => preserve extended attributes
- -v => increase verbosity
- --delete => incremental backup, only backs up the difference between source and destination
- --dry-run => simulates the backup, which allows you to check excluded files match your need

When dry run is satisfactory, run the actual backup, source is the root directory (/) and destination is your USB drive:
```
sudo rsync -aAXv --delete --exclude='/dev/*' --exclude='/sys/*' --exclude='/proc/*' --exclude='/tmp/*' --exclude='/run/*' --exclude='/mnt/*' --exclude='/media/*' --exclude="swapfile" --exclude='.cache' --exclude='Downloads' --exclude='lost+found' / /run/media/fastoch/rsyncBackup
```
It takes quite a while to backup the whole system (around 30 minutes for 15 GB). 

## Restore

If your system crashes and doesn't boot anymore, boot from a live USB.
Let's say we boot from an Arch Live ISO. Select **Boot Arch Linux** on the first page.  
- We need to mount our system and our USB drive
- Then we can restore the backup from our USB drive into our system

To do that, we need to create 2 folders where 


