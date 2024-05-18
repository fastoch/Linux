# Introduction

In computing, a **shell** is a program that exposes an operating system's services to a human user or other programs.

In Linux, any program that you run has 3 important components:
- standard input (StdIn), represented by 0
- standard output (StdOut), represented by 1
- standard error (StdErr), represented by 2

Standard input provides some input to the program. For example, a command that you might type in the terminal.  
The standard output is usually the result that you get when you hit Enter after typing a command.  
Standard error logs any error that is generated when the system is not able to run a command.  

---

# Overwriting & Appending

We can use various redirection techniques to manipulate how these three components are handled.  

For example, we can make a command output its results into a file instead of printing them on screen.  
The basic syntax is: `command > filename`  

A single 'greater than' sign overwrites the existing data.  

If we want to append data rather than overwriting it, we'll use 2 'greater than' signs:  
`command >> filename`  

---

# When can it be useful?

These redirection techniques can be particularly handy when working with applications or scripts.  


---
EOF
