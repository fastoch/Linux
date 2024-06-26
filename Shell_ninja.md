My favorite shell is fish (**F**riendly **I**nteractive **SH**ell).  
The default shell for most Linux distributions is Bash (**B**ourne-**A**gain **SH**ell).

---

# Changing default shell

`chsh -l` will list all the shells currently installed on your machine  
`cat /etc/shells` also displays the currently available shells

`chsh -s /bin/fish` will make fish your default shell, reboot to apply the change  
`su root` to switch user to root and apply the same cmd to change default shell for root

`grep username /etc/passwd` shows the current default shell  

---

# Basic commands

## touch

create a file:  
`touch filename`  
or  
`touch filepath`

## vim 

edit a file:  
`vim filename`  
or  
`vim filepath`

## head

display the first 10 lines of a file:  
`head .bashrc`  

display the first 5 lines of a file:  
`head -5 .bashrc` 

## tail

display the last 10 lines of a file:  
`tail .bashrc`  

display the last 5 lines of a file:  
`tail -5 .bashrc`  

output appended data as the file grows (very handy when working with log files):  
`tail -f filename`

## cat

display the entire contents of a file:  
`cat .bashrc`  

display the entire contents of a file and show line numbers:  
`cat -n .bash_history`

concatenate files to standard output:  
`cat file1 file2` 

## more

display the contents of a file one page at a time:  
`more filename`  
- press Space to display next page
- press Q to quit  
- you can also use arrow keys to navigate through the file

## less

- less is an improvement over more
- more only had forward navigation and limited backward navigation
- less has both forward and backward navigation, as well as search options
- you can go to the beginning or the end of a file instantly
- one line navigation

---

# Intermediate commands

## rm

remove a file:  
`rm filename`  
or  
`rm filepath`

>[!warning]
>The rm command is very dangerous, it can bring an entire system down!!!

force removal:  
`sudo rm -f filename`  

force removal + recursive removal:  
`sudo rm -rf filename`

## redirection


## piping


## grep

---

# Advanced commands

---
EOF
