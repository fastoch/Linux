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
- This very first process that runs right after the kernel boots up is either one of these two:
  - system V (five) init
  - systemd 
- most distros have switched to systemd (Arch did it in 2012) 

# Root

- Root is the **superuser** account in Unix and Linux.  
- It's the user account which has the **highest access rights** on the system.

## Root vs. admin account

- root is not the same as an administrative (admin) account
- admin users are usually members of a special group called "**wheel**" (or "**sudo**")
- when you're part of that special group, you can run commands **as the root user** using a program called **sudo**

## Why not use the root account for everything?

https://dev.to/despider/why-not-to-use-root-user-317a 

# Boot process

## BIOS & UEFI

- First, the BIOS or UEFI program kicks in once the system powers up
- BIOS and UEFI are firmware interfaces that computers use to boot up the operating system (OS)
- The two programs differ in how they store metadata on and about the drive:
  - **BIOS** uses the Master Boot Record (**MBR**)
  - **UEFI** uses the GUID Partition Table (**GPT**)
- Next, the BIOS or UEFI runs the power-on self-test (**POST**). The POST does a series of tasks:
  - verify the hardware components and peripherals
  - carry out tests to ensure that the computer is in proper working condition
- Finally, if the system passes the POST, it signals the start-up process to the next stage

## Boot Loader

- The boot loader is a small program that loads the operating system.
- The main job of the boot loader is to perform three actions with the kernel:
  - locate on the disk,
  - insert into memory,
  - execute with the supplied options
- After the POST has completed successfully, the BIOS/UEFI selects a boot device depending on the system configuration 
- a BIOS system has the boot loader located in the **first sector** of the boot device; this is the MBR (first 512 bytes on the disk)
- a UEFI system stores all startup data in an **.efi file**. The file is on the EFI System Partition, which contains the boot loader

The following are some of the available boot loaders for a Linux system:
- LILO
- SYSLINUX
- **GRUB2**

### GRUB2

Despite the many choices, almost all (non-embedded) modern Linux distributions use GRUB (**GRand Unified Boot Loader**),  
because it’s very feature-rich: 
- ability to boot multiple operating systems
- boots both a graphical and a text-based interface
- strong command line interface for interactive configuration
- network-based diskless booting

Presently, GRUB2 has replaced its past version (GRUB), which is now known as GRUB Legacy.  
Importantly, we can check for the GRUB version in our system using the following command:  
`sudo grub-install --version`  

Now, let’s see what GRUB2 does in the boot process:
- takes over from BIOS or UEFI
- loads itself
- inserts the Linux kernel into memory
- turns over execution to the kernel

At this point, GRUB2 inserts the kernel into memory and turns control of the system over to the kernel.

## Kernel

After going through BIOS or UEFI, POST, and using a boot loader to initiate the kernel, the operating system  
now controls access to our computer resources.  

Here, the Linux kernel follows a predefined procedure: 
- decompress itself in place
- perform hardware checks
- gain access to vital peripheral hardware
- run the init process

Next, the init process continues the system startup by running init scripts for the parent process.  
Also, the init process inserts more kernel modules (like device drivers).  

## Systemd

To reiterate, the kernel initiates the **init** process, which starts the **parent process**.  
Here, **the parent of all Linux processes is Systemd**, which r**eplaces the old SysVinit process**.  

Following the booting steps, Systemd performs a range of tasks:
- probe all remaining hardware
- mount filesystems
- initiate and terminate services
- manage essential system processes like user login
- run a desktop environment

Lastly, Systemd uses the /etc/systemd/system/default.target file to decide the state the Linux system boots into.

## Run levels

In Linux, the run level stands for the **current state of the operating system**.  
Run levels define **which system services are running**.  

Previously, SysVinit identified run levels by number.  
However, .target files now replace run levels in Systemd.  

Now, let’s see the link between run level numbers and targets:
- poweroff.target, run level 0: turn off (shut down) the computer
- rescue.target, run level 1: initiate a rescue shell process
- multi-user.target, run level 3: configure the system as a non-graphical (console) multi-user environment
- graphical.target, run level 5: establish a graphical multi-user interface with network services
- reboot.target, run level 6: restart the machine

In addition, we can change the target (run level) while the system runs.  
This change entails that only services and other units defined under that target will now run on the system.  

For example, to switch to run level 3 from run level 5, we can run the following command:  
`sudo systemctl isolate multi-user.target`  

Then, to take the system to run level 5, let’s run the command:  
`sudo systemctl isolate graphical.target`  
This command returns the run level to graphical.target, equivalent to level 5 for GUI.

# 



@30min

---
EOF
