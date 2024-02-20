 Make a bootable USB drive on any Linux distro  
 https://www.youtube.com/watch?v=rpGgTTFKwiU  

 `dd bs=4M if=~/Downloads/archlinux.iso of=/dev/sdX status=progress && sync`

Replace sdX with the name assigned to the device by your Linux system.  
To find out the name Linux assigned to your flash drive, press Ctrl+Alt+T to open a new terminal and run `sudo fdisk -l`

**sync** is a second command chained to **dd** via **&&**.  
It makes sure that all the data is written to the flash drive and nothing is left in the cache.  

### Be aware that this procedure wipes all the data from your USB flash drive.
### You should make a copy of it before proceeding.
---
After you have used the bootable USB flash drive to install your Linux system, you need to   
restore your flash drive back to its normal not-bootable state.  
- To do so, you need to remove the bootable file system from it: `sudo wipefs --all /dev/sdX`  
- After that, create a new partition on it: `sudo cfdisk /dev/sdX`  
- Select **dos** and press **Enter** to create a new partition. Keep it at its maximum size.  
- Press Enter twice to create this partition and make it primary.  
- Navigate with arrow keys to **Write** and press Enter to write the changes.  
- Type **Yes** to confirm and Quit the program.  
- Run `sudo fdisk -l` to check if the new partition has been created.
