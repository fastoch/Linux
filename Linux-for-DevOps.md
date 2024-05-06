src = https://www.youtube.com/watch?v=3L0HDLh_QMI&list=PLtPuNR8I4TvkwU7Zu0l0G_uwtSUXLckvh

# What is Linux?

In the fast-paced world of DevOps, proficiency in Linux is not just a skill but a necessity.  
Linux is a **kernel**. A kernel is what sits in between the hardware and the software.  

An **operating system** is a complete software package that includes a kernel and other system-level components such as  
device drivers, system libraries, and utilities. https://www.geeksforgeeks.org/difference-between-operating-system-and-kernel/  

GNU/Linux is an OS which uses Linux as its kernel. In fact, there are many versions of GNU/Linux, we call them **distributions**.  
GNU/Linux is **free and open-source** software (FOSS), while Windows and MacOS are not free and are closed-source.  

# History of Linux

Linus Torvalds created the Linux kernel in 1991. He also created Git in 2005.  
But Linux is not the starting point. Before Linux, there was **UNIX**.  

Unix was developed in the 1970s at Bell Labs, and it was the property of AT&T, the American Telephone & Telegraph company.  
**Ken Thompson** and **Dennis Ritchie** codeveloped Unix using a new programming language called C (created by Dennis Ritchie).  
Actually, they weren't backed by anyone on this project.  

The name "UNIX" is derived from "**UNICS**", which means **Uniplexed information and Computing Service**.  
"UNICS" itself is a project derived from a more complex project called "**MULTICS**" (Multiplexed).  
MULTICS was a failure, which led to this new unofficial project called UNICS.  

Unix was very expensive and closed-source. For those reasons, Linus Torvalds decided to make a clone of Unix that he named **Freax**.  
Linus uploaded the source code for Freax to the FTP server of the University of Helsinki. And the administrator of this server  
just decided to name the destination folder "Linux", hence the final name of this kernel.

To exist as part of an OS, the Linux kernel needed **GNU**, an extensive collection of free software created by Richard Stallman in 1983.  
GNU stands for GNU is Not Unix. Together GNU and Linux form what most people call "Linux", when in reality it's **GNU/Linux**.  
Facebook, Amazon, the NASA, IBM, Google, NYSE (New York Stock Exchange)... they all run Linux servers.  

The main core distributions of Linux include Red Hat (Fedora and CentOS), Debian (Ubuntu), Suse, Arch, and Gentoo.

# Using an EC2 instance as your virtual server

- Go to aws.amazon.com and create a new account.  
- Navigate to My account and click on AWS management console.
- Select root user and enter your credentials (email and pwd).  
- Type ec2 in the search box and select the matching result. That will take you to the EC2 console.  

>[!note]
>EC2 = Elastic Compute Cloud

Each virtual machine (VM) is called an **instance**.  

- From the EC2 console, go to Instances and click on Launch an instance to create a new VM.  
- Choose an Amazon Machine Image (AMI),
- choose an instance type,
- storage: make sure that "delete on termination" is checked
- select an existing security group (default or custom)
- review your settings and launch the instance



