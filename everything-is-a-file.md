In the Linux ecosystem, everything is considered a file, even directories.  

# Regular files



# Directories



# Special files

## Block files

They allow your system to talk to your hardware components.  
They provide a method of communication with the device drivers for your hardware resources through the file system.  

Since block files are buffered access type files, they can transfer a large block of information at any given time.  

To search for all the buffered files in your system: `ls -l /dev | grep "^b"`  
This command looks for all files in the /dev folder and filters those which name starts with "b".


---
EOF
