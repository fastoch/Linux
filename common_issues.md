source = https://www.youtube.com/watch?v=xsdFNpThetE  

# SSH failures

**symptom** = the user is not able to log in to a server via SSH  

First thing is to check if the user is the only one who cannot connect to the server.  
If he's the only one, try tailing the authentication logs: `tail -f /var/log/auth.log`.  
Depending on your distro, logs might be saved in a different file.  
Once you've found the correct log file, tail it and ask the user to try again.  

# Slow performance in Gnome 

Gnome is the default desktop environment for Ubuntu, Fedora, and many other distributions.  
The slowness might come from **tracker**, an application which scans files on your system to allow you to find them faster.  
When you have **network shares** mounted to your desktop, tracker is going to index everything, causing your system to run slow.  

**solution**: create a brand new file at the root of your file share. For instance:  
```
cd /mnt/truenas/archive
touch .trackerignore
```
This will stop tracker from scanning and indexing that directory.  
You should systematically include this file in your file shares.  

# The disk is full (but it isn't)

`df -h` might show you that you have plenty of free space.  
Try running `df -i` instead. This will show you how many inodes you're using. Maybe you're out of inodes.  

**Inodes** store metadata for every file on your system in a table-like structure usually located near the beginning of a partition.  
They store all the information except the file name and the data.  

If you run out of inodes, you cannot create new files, even if you have space left on the given partition.  

For example, if you run a mail server, you might have a mail that won't leave the server 

# 
