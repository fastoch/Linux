In the Linux ecosystem, everything is considered a file, even directories.  

# The ls command provides the file type

if you run `ls -l` or `ls -al` (shows hidden files), the first column in the ouput displays file type and permissions.  
- if the first character is a dash, the file is a regular file
- if the first character is a b, the file is a block file
- if the first character is a c, the file is a character file
- if the first character is a d, the file is a directory
- if the first character is an l, the file is a symbolic link
- if the first character is a p, the file is a named pipe
- if the first character is a s, the file is a socket file

# Block files

They provide buffered access to hardware devices.  
They communicate with the device drivers through the file system.  

Since block files are buffered access type files, they can transfer a large **block** of data at any given time.  

To search for all the block files in your system: `ls -l /dev | grep "^b"`  or `ls -l /dev | grep ^b`  
This command outputs all files in the /dev folder whose name starts with "b". The caret sign ^ means "starts with".  

# Character files

A character file is also a device file, but it provides unbuffered serial access to your hardware components.  
With unbuffered serial access, we can only transfer data one **character** at a time.  

To search for character files: `ls -l /dev | grep ^c`  
This cmd outputs files in the /dev folder whose name starts with a "c".  

# Symbolic links

They are references to other files on the file system.  
To output symbolic links: `ls -l /dev | grep ^l`  

To create a symbolic link: `ln -s target link_name`  
For instance: `ln -s .bash_history b_hist`  

# Pipes and named pipes

We use the **pipe sign |** to pass the output of a command to the input of another command.  
A pipe allows inter-process communication (IPC), meaning data transfer between processes running on the same system.  
The syntax is: `command1 | command2`  

Pipes can be chained: `command1 | command2 | command3`  

- A **traditional pipe** is "unnamed" and lasts only as long as the process.
- A **named pipe**, however, can last as long as the system is up, beyond the life of the process.

To create a **named pipe**: `mkfifo pipe1`  
you can check pipe creation: `ls -l | grep ^p`  

# Socket files

Just like pipes, they provide a means of inter-process communication.  
Except they transfer the data between processes running on different environments or different machines.  

---
EOF
