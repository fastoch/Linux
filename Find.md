- to find all files named "fastoch" inside the root directory: `sudo find / -xdev -name fastoch`

>[!tip]
>we use -xdev to restrict the program to only finding files on the current filesystem

- to return the number of results for the previous cmd: `sudo find / -xdev -name fastoch | wc -l`
- to find all files that end in .c: `sudo find / -xdev -name '*.c'`
- 
