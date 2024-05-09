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

# Directory structure

- In a Linux system, the start of a file system is a directory called **root**.  
- This root directory is represented with a **slash /**.
- only a special user called **root** can write to the root directory
- The **/bin** directory, short for **binaries**, contains files that are essential to the operating system.
- **/bin** does not contain subdirectories, only **commands** or symbolic links to commands.
- Another executable location is **/sbin**, which stores **executables for system administration**.
- The **/opt** folder is used for installing **optional** or add-on software packages that are not part of the core operating system
- The **/boot** folder contains files required for the boot process except for configuration files not needed at boot time
- 

---
EOF
