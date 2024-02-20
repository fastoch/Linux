 Make a bootable USB drive on any Linux distro  
 https://www.youtube.com/watch?v=rpGgTTFKwiU  

 `dd bs=4M if=~/Downloads/archlinux.iso of=/dev/sdX status=progress && sync`

Replace sdX with the name assigned to the device by your Linux system.  
To find out the name Linux assigned to your flash drive, press Ctrl+Alt+T to open a new terminal and run `sudo fdisk -l`

**sync** is a second command chained to **dd** via **&&**.  
It makes sure that all the data is written to the flash drive and nothing is left in the cache.  

### Be aware that this procedure wipes all the data from your USB flash drive.
---
After you have used
