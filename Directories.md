# Useful commands

- to print the current working directory (absolute path): `pwd`    
- to create a directory in the working directory: `mkdir dirname`  
- to create a directory in another directory: `mkdir path/dirname`
- to get into this new directory: `cd dirname`  or `cd path/dirname`  
- to go back to your home directory: `cd` or `cd ~`  

>[!tip]
>your home directory is represented with a tilde ~ in the cmd prompt

- to go one level up (parent directory): `cd ..`  
- to go to the root of the file system: `cd /`  
- to remove an empty directory: `rmdir dirname` or `rm -d dirname`
- to remove a non-empty directory: `rm -rf dirname` (-r = recursive, -f = force)  
- To be prompted before deletion: `rm -rfi dirname`  (type y for yes, default is no)

>[!caution]
>be very careful when using `rm -rf`, as this cmd can destroy your entire system!

# Linux Directory structure

- The start of any file system is a directory called **root**.  
- This root directory is represented with a **slash /**.
- Only the **superuser**, a special user also called **root**, can write to the / directory
- **/root** is the home directory of the **root user**
- The **/bin** directory, short for **binaries**, contains files that are essential to the operating system.
- **/bin** does not contain subdirectories, only **commands** or symbolic links to commands.
- Another executable location is **/sbin**, which stores **executables for system administration**.
- The **/opt** folder is used for installing **optional** or add-on software packages that are not part of the core operating system
- The **/boot** folder contains files required for the boot process except for configuration files not needed at boot time
- The **/dev** directory consists of files that represent devices that are attached to the local system
- The **/etc** directory contains configuration related information for our system

>[!tip]
>/etc contains crontab, a utility that allows us to schedule tasks

- The **/srv** folder contains data for services provided by the system (like ftp or http)
- The **/home** folder is the home directory for all users except the root user
- Everything that is in the **/tmp** directory gets deleted when you reboot your system
- The **/lib** folder contains libraries that support the binaries located in /bin and /sbin
- These library files contain explanations for the OS on how to handle the binary files
- The **/usr** folder contains binaries, libraries and documentation for second-level programs
- if you don't find a command under /bin or /sbin, you can find it under usr/bin or /usr/sbin

>[!important]
>second-level programs are programs that are not essential for the operating system to work properly 

- **/media** & **/mnt** are directories where you may mount a physical device such as a CD-ROM or a USB stick
- The **/var** directory contains files that are expected to grow over time (in particular the cache and log subfolders)

---
EOF
