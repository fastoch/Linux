# Permission groups

- **owner**: the user that owns the file (u)
- **group**: the group that the file belongs to (g)
- **others**: all other users (o)

# Permission types

- **read** (r) = 4
- **write** (w) = 2
- **execute** (x) = 1

>[!warning]
>To view the contents of a directory, you need execute permission on that directory.

When you run `ls -l`: 
- the first column represents the file type (file, directory, etc.) and permissions for the owner, the group, and other users
- the third column represents the owner of the file
- the fourth column represents the group that this file belongs to

![image](https://github.com/fastoch/Linux/assets/89261095/acbbcf62-7706-4caf-bca5-831502f3892f)

In the first column:
- The file type is represented by one character (-, d, l, b, c, p, s)
- Each set of permissions is represented by 3 characters (rwx), always in that order: owner, group, others
- A dash means that this type of permission is denied. r-x means that you can only read and execute the file

>[!important]
>When you set ACLs for a file, you'll see a + sign at the end of the first column (after permissions)

---

# Commands

## chmod

- to remove the read permission for other users: `chmod o-r filename`
- to add write permission to the group: `chmod g+w filename`
- to add read and write permissions for the owner: `chmod u+rw filename`

Another way to set permissions is to use the **octal notation**:
- 0 means no permission
- 1 means execute permission only
- 2 means write permission only
- 4 means read permission only
- 3 means write and execute permissions (3+1)
- 5 means read and execute permissions (4+1)
- 6 means read and write permissions (4+2)
- 7 means all permissions (4+2+1)

**Example**:  
`chmod 750 filename`  
In this example, the owner has all permissions, members of the group can write and execute, and other users have no permission.  

>[!warning]
>When you apply permissions to a directory, this will not apply to subdirectories nor to files inside that directory.

To apply permissions in a **recursive** manner, we must use the **-R** option: `chmod -R 750 filename` 

## chown & chgrp

- to change the owner of a file: `sudo chown newowner filename`
- to change the owner and the group that a file belongs to: `sudo chown newowner:newgroup filename`
- to only change the group that the file belongs to: `sudo chgrp newgroup filename`
- to do any of that recursively, we'll use the -R option

---

# Special permissions

src = https://linuxconfig.org/how-to-use-special-permissions-the-setuid-setgid-and-sticky-bits  

On a Unix-like OS, the ownership of files is based on the default uid (user-id) and gid (group-id) of the user who created them.  
The same is true when a process is launched: it runs with the effective uid and gid of the user who started it, and with the corresponding privileges. This behavior can be modified by using special permissions.

## setuid

When the setuid bit is used, the behavior described above is modified so that when an executable is launched, it does not run with the privileges of the user who launched it, but with those of the file owner instead.   

If an executable has the setuid bit set on it, and it’s owned by root, when launched by a normal user, it will run with root privileges.  
It should be clear why this represents a potential security risk, if not used correctly.

For example, the command **passwd** is systematically executed as root, no matter who runs it.  
And if we run `ls -l /usr/bin/passwd`, we'll see that the owner permissions contain a **s** which stands for **setuid**:
![image](https://github.com/fastoch/Linux/assets/89261095/c0c8f825-779d-4399-b5fd-57272c10d071)  

Meaning that whoever runs the passwd cmd, it will always be executed as root.  
Which allows any user to change its password without being root.  

- The setuid bit is represented by an **s** in place of the x of the executable bit on the **owner sector** (4th character).  
- The s implies that the executable bit is set, otherwise you would see a capital S.  
- The setuid and setgid bits have no effect if the executable bit is not set. 

>[!important]
>The setuid bit has no effect on directories.

## setgid

Unlike the setuid bit, **the setgid bit has effect on both files and directories**.  

A **file** which has the setgid bit set, when executed, instead of running with the privileges of the group of the user who started it, runs with those of the group which owns the file. In other words, the group ID of the process will be the same as that of the file.  

When used on a **directory**, the setgid bit alters the standard behavior so that the group of the files created inside said directory will not be that of the user who created them, but that of the parent directory itself.  
This is often used to ease the sharing of files (files will be modifiable by all the users that are part of said group).  

This time, the **s** is present in place of the executable bit on the **group sector** (7th character).

## sticky bit

The sticky bit works in a different way.  
While it has **no effect on files**, when used on a directory, all the files in said directory will be **modifiable only by their owners**.  

A typical case in which it is used involves the /tmp directory.  
This directory is writable by all users on the system.  
So, to make impossible for one user to delete the files of another one, the sticky bit is set.  

The sticky bit is identifiable by a **t** which is reported where normally the executable x bit is shown, in the **“others” section**.  
Again, a lowercase t implies that the executable bit is also present, otherwise you would see a capital T.

## How to set special bits

Just like normal permissions, the special bits can be assigned with the **chmod** command, using the numeric or the ugo/rwx format.  
In the former case the setuid, setgid, and sticky bits are represented respectively by a value of 4, 2 and 1

For example, if we want to set the setgid bit on a directory we would execute: `chmod 2775 test`  
With this command we set the setgid bit on the directory, identified by the first of the four numbers,  
and gave full privileges on it to its owner and to the user that are members of the group the directory belongs to,  
plus read and execute permissions for all the other users.  

>[!important]
>Remember that the execute bit on a directory means that a user is able to cd into it or use ls to list its content.

The other way we can set the special permissions bits is to use the ugo/rwx syntax.
- To set the setgid bit: `chmod g+s filename`
- To apply the setuid bit to a file, we would run: `chmod u+s filename`
- to apply the sticky bit: `chmod o+t dirname`

---
EOF
