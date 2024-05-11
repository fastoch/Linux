**sources**: 
- https://wiki.archlinux.org/title/Users_and_groups#User_management  
- https://wiki.archlinux.org/title/Users_and_groups#Group_management  

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
- free-text information
- the home directory
- the default login shell

---

To create a new user: `sudo useradd -m -G additional_groups -s login_shell newuser`  

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

- A group is a collection of users with similar permissions.
- Each group has a unique group id.  

When a user is created, they're automatically added to a new group with the same name as the username.  

- to display all existing groups on the system: `cat /etc/group` (first column = group name, third column = group id)
- to check all the groups a specific user belongs to: `groups username`
- to create a group: `sudo groupadd groupname`
- to manually assign a group id to our new group: `sudo groupadd -g 1001 groupname` 
- to modify an already existing group: `sudo groupmod -g 1002 -n newname oldname`
- to delete a group: `sudo groupdel groupname`

---

## sudo privileges

When using `sudo` before a command, I'm telling the system to run this command as an administrator.  
Users that are part of the **wheel** group are called **sudoers** and are able to run commands as administrators.  

>[!note]
>On some Linux systems (Debian), the sudoers group is called **sudo** instead of **wheel**.  

To display all sudoers: `cat /etc/group | grep wheel`  

To add a user to the wheel group: `sudo usermod -aG wheel username`  
The `-a` option appends the new group to the list of other groups the user is already a member of.  

>[!warning]
>Without the -a option, you add the user to the specified group but you remove it from all other groups.



---



