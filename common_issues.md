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

