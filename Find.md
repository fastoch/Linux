- to find all files named "yay" under the root directory: `sudo find / -xdev -name yay`

>[!tip]
>we use -xdev to restrict the program to only finding files on the current filesystem

- to return the number of results for the previous cmd: `sudo find / -xdev -name yay | wc -l`
- to find all files under the root directory that end in .c: `sudo find / -xdev -name '*.c'`
- to find all files under the current working directory that end in .txt: `find . -xdev -name '*.txt'`
