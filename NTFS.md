# Use NTFS on Linux

ntfs-3g is an open-source version of Microsoft Windows NTFS partition system.  
- To install ntfs-3g: `sudo pacman -S ntfs-3g`  

Now I can easily mount NTFS-based partitions on my Linux machine.  

---

# Mount point 

Under the root directory, I create a media directory, followed by a folder with the name of my choice.  
```
cd /
sudo mkdir -p /media/drive
```
I will use this folder later as a mount point for a USB drive or an external hard drive.  

---

# fstab

After that, I'm going to edit fstab (File Systems TABle) so I can easily mount an NTFS-based drive later.  

- First, run `sudo blkid | grep ntfs` to get the name of your NTFS-based drive. In my case it's /dev/sda1.  
- Then run `sudo vim /etc/fstab` to edit the file.

Here's the lines I've added to my fstab file:  
```
# /dev/sda1
/dev/sda1        /media/drive     ntfs-3g      rw,uid=1000,gid=1000,umask=0022,fmask=0022    0 0
```

1000 is my user id (uid). It's also my group id (gid).  
You can find the uid and gid of a given user by running `cat /etc/passwd username`.  

noauto option makes sure that your OS won't automatically try to mount the drive until you want to mount it.

What is umask? https://www.cyberciti.biz/tips/understanding-linux-unix-umask-value-usage.html  
What isfmask? no idea

More on fstab: https://linuxconfig.org/how-fstab-works-introduction-to-the-etc-fstab-file-on-linux

---

# reload the daemon

**daemon** acts as our operating system's supervisor, telling us what we can and cannot do.  
It needs to be reloaded in order to apply the modifications we made to fstab.  
`systemctl daemon-reload`  

We can now mount our drive by simply opening the file explorer and clicking on the drive, like we would do on a Windows OS.


---
EOF
