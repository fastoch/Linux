# Users

Linux is a multi-user operating system, meaning multiple users can log in to the OS at the same time.  

Users in Linux can be of two types: 
- **human** users: the people who log in to the system.  
- **system** users: used to start non-interactive background services.

From the perspective of the OS, there is no distinction between human users and system users.  
However, each user is assigned a unique user id. And the user id range is different for human users and system users.  
To see those ranges: `grep UID /etc/login.defs`  

---

The information about users is stored in /etc/passwd. To display its contents: `cat /etc/passwd`  
This file contains seven fields for each user: 
- the username
- a placeholder for the password (password is stored in the /etc/shadow file)
- the user id
- the group id
- the user's full name
- the home directory
- the default login shell

---

To create a new user: `sudo useradd -m -G additional_groups -s login_shell newuser`  
Explanation: https://wiki.archlinux.org/title/Users_and_groups#User_management  

There's another way of adding a user: `sudo adduser username`  

**useradd** is native binary compiled with the system.  
**adduser** is a perl script which uses useradd binary in back-end.  
**adduser** is more user friendly and **interactive** than its back-end useradd.  

Although it is not required to protect a newly created user with a password, it is highly recommended to do so:  
`sudo passwd newuser`  

---

To modify the username of an existing user: `sudo usermod -l newname oldname`  

When changing the username, you may also want to change the user’s home directory to reflect the new username.  
To change the home directory: `sudo usermod -d /home/newname -m newname `  
The -m option in usermod command is used to move the content of the old home directory to the new home directory.  

--- 

To lock a user account: `sudo usermod -L username`  
To unlock a user account: `sudo usermod -U username`  

To switch to a different user: `su username`  
When trying to switch to a locked user, you'll get an authentication failure.  

To switch to the root user: `su root`. This will prompt you for the root password.  

If the current user is a **sudoer**, you can use this cmd to switch to root: `sudo -i`  
This will prompt you for the sudo password of the current user instead of the root password.  

---

To delete a user: `sudo userdel -rf username`  
-f forces removal of files, even if not owned by user  
-r removes home directory and mail spool (if existing)

---

# Groups

- A group is a collection of all users that perform a similar function.
- Each group has a unique group id.  
- Each user is part of a group.

https://wiki.archlinux.org/title/Users_and_groups#Group_management
