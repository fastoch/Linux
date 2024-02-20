Format a USB Drive in Linux  
https://www.baeldung.com/linux/usb-drive-format

---

Backup and Restore Your Linux System with rsync  
https://www.youtube.com/watch?v=oS5uH0mzMTg  

**rsync** is a command line tool available on any Linux system.  

# Backup

cd to root: `cd /`  

Then, run the following cmd after replacing `<source>` and `<destination>` with their actual locations:
  
```bash
sudo rsync -aAXv --delete --dry-run --exclude={/dev/*,/proc/*,/sys/*,/tmp/*,/run/*,/mnt/*,/media/*,"swapfile",/lost+found,~/.cache} /<source> /<destination>
```

I've specified some directories to exclude because they're populated and used only after the system boots up.  
rsync will backup these folders, but not their content.  
    
-a => archive mode  
-A => preserve ACLs (access control lists)  
-X => preserve extended attributes  
-v => increase verbosity  

--delete => make an incremental backup  
if this is your first backup, this option does nothing  
if this is this not your first backup, only the difference between your source and destination will be backed up
  
--dry-run => simulate the backup  
 A dry run allows you to check if your forgot to exclude some files from the backup.

When you're ready to make an actual backup, simply run the same cmd after removing the `--dry-run` option.  
For example, to backup the whole system, specify the root folder (/) as the source, and your USB drive as the destination:
```bash
sudo rsync -aAXv --delete --exclude={/dev/*,/proc/*,/sys/*,/tmp/*,/run/*,/mnt/*,/media/*,"swapfile",/lost+found,~/.cache} / /run/media/fastoch/BootableBackup/
```

To get the path to your USB drive, open it in your file browser when it's mounted, and copy its path from the address bar at the top.

# Restore


