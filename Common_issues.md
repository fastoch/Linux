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

# What's wrong with my WiFi?

Linux distributions offer **live mode**. You can use this to demo the distro before you install it.  
And if WiFi doesn't work in live mode, then you shouldn't install this distro in the first place.  

This usually comes down to hardware compatibility.  
Since drivers are built into the Linux distribution itself, usually you'll have drivers for all the noteworthy Wi-Fi cards out there.  
If your Wi-Fi card is not compatible with your distro, the easiest solution is to replace it with one that's known to work with Linux.  
You can also consider buying a USB Wi-Fi card, especially if your manufacturer doesn't allow you to replace the internal WiFi card.

# Gnome apps aren't opening

Go to Settings and make sure the locale (language) settings are set up properly.
Once you set the locale settings, log out, log in, and the applications should open again.

# Poor gaming performance (Nvidia)

Normally, your video card driver is going to be built into your distribution.  
So you shouldn't have to do anything in order to get your video card up and running.  

But Nvidia has a proprietary driver that they don't upstream to Linux.  
As a result of this, **you have to install the Nvidia driver manually**.  
Thankfully, most distributions make this pretty easy nowadays.  

# Extremely Erratic Behavior

You probably have memory issues.  
If you have defective memory chips, your computer might run just fine most of the time, but have some quirks now and then.  
In such a case, it's recommended to **run Memtest86+** for a few hours: https://memtest.org/  

# Narrowing down hardware vs software issues

Download the ISO image of your favorite distro, create a bootable media out of it, and run it in live mode on your hardware.  
If your problem persists in that live environment, then it's likely that there's a hardware issue.  
Otherwise, you might have a configuration issue in your installed environment, something to fix in the settings.  

**Live media is something you should always have around.**  
It helps you troubleshoot things by narrowing down where the problem space is.  
And if you can't boot your OS, a live media will allow you to get some files off of it before reinstalling.

# DisplayLink causes many issues with docking stations

You should ditch DisplayLink. It's a popular technology for USB hubs and docking stations.  
The thing is DisplayLink is very problematic and it has no reason to exist whatsoever, seriously.  
Any operating system knows how to interface with docking stations by itself, it doesn't need DisplayLink.  

With a true docking station, you just plug it in and you're done, nothing to install.  
If you have to install something to make it work, then there's a problem.  

Just get a standard USB-c docking station, and you won't have any problem.

# Your hardware isn't properly supported

Maybe you're runnning a distribution that's not compatible with your hardware.  
But more often than that, the problem is that you're using too old of a Linux kernel.  

The Linux kernel has most of the drivers that Linux needs built right in.  
If you're using a kernel that's older than your hardware, then it might not include the proper drivers.

