Backup and Restore Your Linux System with rsync  
https://www.youtube.com/watch?v=oS5uH0mzMTg&t=9s  

## Backup

**rsync** is a command line tool available on any Linux system.  
It's a universal way to backup files on Linux.  

The idea is to backup our entire system to a USB flash drive.  
We also need a live USB drive to restore the system, in case it doesn't boot anymore.

It's very important to use some Linux file system on your backup USB drive, because   
if you use FAT file system, rsync won't be able to copy all the files attributes.  
Format your usb drive with ext4 for example.

Once your backup usb drive is ready, run the following cmd:
```
sudo rsync -avAX --delete --dry-run --exclude='/dev/*' --exclude='/sys/*' --exclude='/proc/*' --exclude='/tmp/*' --exclude='/run/*' --exclude='/mnt/*' --exclude='/media/*' --exclude='.cache' --exclude='Downloads' --exclude='lost+found' /source /destination
```

Alternatively, you can group your excluded files, but you must cd to root (`cd /`) before running rsync:
```
sudo rsync -avAX --delete --dry-run --exclude={"~/.cache","/dev/*","/proc/*","/sys/*","/tmp/*","/run/*","/mnt/*","/media/*","lost+found","~/Dowloads"} /source /destination
```

- -a => archive mode
- -A => preserve ACLs (Access Control Lists)
- -X => preserve extended attributes
- -v => increase verbosity
- --delete => incremental backup, only backs up the difference between source and destination
- --dry-run => simulates the backup, which allows you to check excluded files match your need

When your dry run is satisfactory, run the actual backup, source is the root directory (/) and destination is your USB drive:
```
sudo rsync -avAX --delete --exclude='/dev/*' --exclude='/sys/*' --exclude='/proc/*' --exclude='/tmp/*' --exclude='/run/*' --exclude='/mnt/*' --exclude='/media/*' --exclude='.cache' --exclude='Downloads' --exclude='lost+found' / /run/media/fastoch/rsyncBackup
```

Alternative cmd:
```
sudo rsync -avAX --delete --exclude={"~/.cache","/dev/*","/proc/*","/sys/*","/tmp/*","/run/*","/mnt/*","/media/*","lost+found","~/Dowloads"} / /run/media/fastoch/rsyncBackup
```

It takes quite a while to backup the whole system (around 30 minutes for 15 GB). 

## Restore

If your system crashes and doesn't boot anymore, boot from a live USB drive.
Let's say we boot from an Arch Live ISO. Select **Boot Arch Linux** on the first page.  

Now, we need to do two things:
- We need to mount our system and our backup USB drive
- Then we need to restore the backup from our USB drive into our system

To do that, we need to create 2 folders where we're going to mount our devices.  
`mkdir /mnt/system /mnt/usb`

Now, we need to check the names of our devices with `lsblk`

Then, we can mount our devices:   
`mount /dev/sda1 /mnt/system`  
`mount /dev/sdb1 /mnt/usb`

You can enter these folders and check what files are there:
```
cd /mnt/system
ls
cd ../usb
ls
```

Now, let's restore the backup:  
`rsync -avAX --delete /mnt/usb/ /mnt/system`  
It is **important** to remove the slash at the end of the 'system' directory, because otherwise rsync creates a 'usb' subdirectory within the 'system' directory and restores everything there, which is not what we want.

The option `--delete` is there to restore only the files that have been deleted, which saves us a lot of time.
