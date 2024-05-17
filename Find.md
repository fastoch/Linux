src: 
- https://www.man7.org/linux/man-pages/man1/find.1.html
- https://sarata.com/linux_find.html

---

- to find all files named "yay" under the root directory: `sudo find / -xdev -name yay`

>[!tip]
>we use -xdev to restrict the program to only finding files on the current filesystem

- to return the number of results for the previous cmd: `sudo find / -xdev -name yay | wc -l`
- to find all files under the root directory that end in '.c': `sudo find / -xdev -name '*.c'`
- to find all files under the current working directory that end in '.txt': `find . -xdev -name '*.txt'`
- to ignore case, use `-iname` instead of `-name`
- to find only files, use `-type f`
- to find only directories, use `-type d`
- to find all directories in the current folder starting with 'Do': `find . -type d -name 'Do*'`

---

to find the number of directories under the current directory and exclude subdirectories:  
`find . -maxdepth 1 -type d | wc -l`

to find all directories under ~/Documents/ for which current user has all permissions while other users can read and execute:  
`find ~/Documents -perm 755 -type d`  

to restrict the previous command to directories named 'origin':  
`find ~/Documents -perm 755 -type d -name origin` 

to find all files in ~/.cache that haven't been accessed for 30 days and delete them:  
`find ~/.cache/ -type f -atime +30 -delete`

to find all files in ~/Documents that do not end in '.txt':  
`find ~/Documents ! -name '*.txt'`  



@60%
