# What is Linux?

- Linux is the **Kernel** that powers a Linux Operating System 
- Kernels are programs that **talk directly to the hardware** and manage resources & processes
- Kernels need a whole operating system (OS) to be useful
- When a Linux Kernel is bundled with OS software, that is called a **Linux distribution** (distro)

>[!important]
>The kernel is the only program allowed to talk to the hardware directly.<br>
>Everything else has to talk to the kernel through things called **system calls**.

# Linux Distributions

- **Red Hat** Family: includes Fedora and CentOS
- **Debian** Family: includes Ubuntu and Kali Linux
- Source Code Distributions: includes **Arch** Linux and **Gentoo**

# System V init vs. SystemD: 

Once the kernel starts up, the next big thing is "how do you start up everything else?  
- This very first process to run right after the kernel boots up is either one of these two:
  - system V (five) init
  - systemd 
- most distros have switched to systemd (Arch did it in 2012) 

# 




---
EOF
