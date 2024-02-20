Format a USB Drive in Linux  
https://www.baeldung.com/linux/usb-drive-format

---

Backup and Restore Your Linux System with rsync  
https://www.youtube.com/watch?v=oS5uH0mzMTg  

**rsync** is a command line tool available on any Linux system.  

cd to root: `cd /`  

Then, run the following:
  
```bash
sudo rsync -aAXv
```
-a = archive mode  
-A = preserve ACLs (access control lists)  
-X = preserve extended attributes  
-v = increase verbosity  

--dry-run simulates the backup  

