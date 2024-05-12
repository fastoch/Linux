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
- The file type is represented by one character (-, d, l, b, c, p, s, S, T)
- Each set of permissions is represented by 3 characters (rwx), always in that order: owner, group, others
- A dash means that this type of permission is denied. r-x means that you can only read and execute the file

>[!important]
>When you set ACLs for a file, you'll see a + sign at the end of the first column (after permissions)

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

# Special permissions

When running `ls -l`, we've seen that the first character of the first column indicates the type of file.  
This also indicates special permissions:
- d = directory
- \- = no special permission (simple file)
- l = symbolic link (file pointing to some other file)
- S = setuid/setgid permissions

---
EOF
