Backup and Restore Your Linux System with rsync  
https://www.youtube.com/watch?v=oS5uH0mzMTg&t=9s  

## Backup

**rsync** is a command line tool available on any Linux system.  
It's a universal way to backup files on Linux.  

The idea is to backup our entire system to a USB flash drive.  
This flash drive will then allow us to restore our Linux system if it crashes.  
```
sudo rsync -aAXv --delete --dry-run --exclude='/sys/*' --exclude='/dev/*' --exclude='/mnt/*' --exclude='/media/*' --exclude="swapfile" --exclude='~/.cache' --exclude='/lost+found' /source /destination
```
- -a => archive mode
- -A => preserve ACLs (Access Control Lists)
- -X => preserve extended attributes
- -v => increase verbosity
- --delete => incremental backup, only backs up the difference between source and destination
- --dry-run => simulates the backup


## Restore

