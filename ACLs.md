# Introduction

Access Control Lists are one of the most basic means of implementing security on a system.  
ACLs are very intertwined with Linux file permissions.  

**Reminder**:  
- r = permission to read
- w = permission to write
- x = permission to execute

When you run `ls -l`, you get the permissions for:
- the file owner (**U**ser)
- users belonging to the same group as the owner (**G**roup)
- all other users (**O**thers)

---

# getfacl & setfacl

To get a file ACL: `getfacl filename`  

Example:  
![image](https://github.com/fastoch/Linux/assets/89261095/6257d182-9d93-4b06-9109-cc5b5d7dc4d3)  

To make changes to a file ACL: `setfacl -m u:username:rx filename`  
https://www.golinuxcloud.com/setfacl-getfacl-command-in-linux/  

To remove all extended ACL entries: `setfacl -b filename`

---

# ACL masks

src = https://www.howtogeek.com/how-acl-masks-let-you-fine-tune-file-permissions-in-linux/  



---
EOF
