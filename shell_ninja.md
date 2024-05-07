My favorite shell is fish (**F**riendly **I**nteractive **SH**ell).  
The default shell for most Linux distributions is Bash (**B**ourne-**A**gain **SH**ell).

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

>[!note]
>cat was created to concatenate files to standard output, run `cat --help` to know more

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

# Advanced commands

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
EOF
