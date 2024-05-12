# Permission groups

- **owner**: the user that owns the file
- **group**: the group that the file belongs to
- **others**: all other users 

# Permission types

- **read** (r) = 4
- **write** (w) = 2
- **execute** (x) = 1

>[!warning]
>To view the contents of a directory, you need execute permission on that directory.

When you run `ls -l`: 
- the first column represents the file type (file, directory,) and permissions (owner, group, others)
- the third column represents the owner of the file
- the fourth column represents the group that this file belongs to

![image](https://github.com/fastoch/Linux/assets/89261095/acbbcf62-7706-4caf-bca5-831502f3892f)

>[!important]
>When you set ACLs for a file, you'll see a + sign at the end of the first column (after permissions)

# Commands

## chmod

## chown

## chgrp


---
EOF
