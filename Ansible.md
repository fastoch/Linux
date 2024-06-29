src = https://www.youtube.com/watch?v=gIDywsGBqf4

---

# Using Ansible to automate your endpoints' configuration

When I get a brand new computer and want to set it up, I just open up the terminal and run this cmd:
- `curl deploy/bootstrap | sudo bash`

This is the only cmd that I have to run to set up a new computer.

## What exactly is this cmd doing?

- The `curl` cmd allows you to fetch something from a URL
- `deploy` is actually the hostname of a Web **server** on my LAN
- that server is serving one **file** named `bootstrap` (via Apache)
- this `bootstrap` file is a simple **bash script**
- `curl` is going to fetch that `bootstrap` file from the `deploy` server...
- ...and we pipe this file into `sudo bash` in order to run the script it contains

## What my script is doing

- First of all, this script will install all updates.  
- Then, it will install Ansible.
- After that, it will use Ansible to provision my new computer

>[!important]
>My script uses ansible-pull very heavily. Ansible-pull is installed automatically when you install Ansible.

## About Ansible-Pull

**Ansible** runs on an Ansible **server** which uses SSH to connect to all of your computers and configure them (the config is **pushed**).  
**Ansible-Pull** does the opposite. It runs on the **computer** you want to configure and **pulls** the config from the server.  

Ansible-pull will download the changes from a **Git** repository which must contain a special file named `local.yml`.  
Here's an example of a typical ansible-pull command: `ansible-pull -U https://github.com/fastoch/Ansible/mydesktopconfig.git`  

>[!important]
>The Git repo doesn't have to be hosted on Github, it can be hosted on a local Git server, in which case the URL will probably start
>with http instead of https.

**The Git repo has to be structured in a very specific way**.  
We'll get into how to create such a repo and how to populate it with actual configs that will get pulled by your endpoints.

More details:
- https://www.youtube.com/watch?v=sn1HQq_GFNE
- https://www.devopsschool.com/blog/what-is-ansible-pull-and-how-can-we-use-it/

## Create a repository for Ansible-pull

- in our previous `ansible-pull` command, we only provided the repository's URL, we did not specify which file to run
- The file that the `ansible-pull` command will run is `local.yml` and it has to be located **at the root of the repo**

Here's an sample from a typical `local.yml` file:

```yaml
# Tasks to complete before running roles
- hosts: all
  tags: always
  become: true
  pre_tasks:
    - name: pre-run | update package cache (arch)
      tags: always
      pacman: update_cache=yes
      changed_when: False
      when: ansible_distribution == "Archlinux"

    - name: pre-run | update package cache (debian, etc)
      tags: always
      apt: update_cache=yes
      changed_when: False
      when: ansible_distribution in ["Debian", "Ubuntu"]

# Run roles
- hosts: all
  tags: base
  become: true
  roles:
    - base

- hosts: workstation
  tags: workstation
  become: true
  roles:
    - workstation

- hosts: server
  tags: server
  become: true
  roles:
    - server
```

In this sample, you can see that:
- first of all, I'm going to target all hosts
- I'll run some **pre_tasks** against those hosts
  - pre_tasks are basically just tasks you want to run before anything else
    - the first play will run the pacman module, which is the package manager for Arch Linux
    - it will use that module to update the package cache (update the repository index)
    - but it will only do that if the distro it's being run on is actually Archlinux
    - the next play does the same thing but uses the apt module, the package manager for Debian-based distros

This `when` keyword is asking Ansible to check if a condition is true before running a task against a given host.  

- Then we have **roles**. Roles are explained in this video: https://www.youtube.com/watch?v=tq9sCeQNVYc
  - A role allows you to categorize tasks and run those tasks only against machines that are part of that role
  - The base role is for any machine, it's the base configuration I want to apply on all my hosts
  - if a machine is made a member of the workstation role, then it will get workstation tasks run on it
  - and the same logic applies to the mmembers of the server role

Every computer will get the base role first, no matter what, and then will either get the workstation role or the server role.  

**But how does Ansible know which role to apply?**
- This has to do with another important file which must also be present at the root of the Git repo
- Besides the `local.yml`, this other important file is `hosts`. This is an **inventory file**.
- Normally, with ansible-pull, you don't have an inventory file, but you can still use one

Here's a sample from a typical inventory `hosts` file:
```
[workstation]
arch-staging.home-network.io
debian-staging.home-network.io
demo.learn-linux.tv
kirin.home-network.io
```

The structure of my local Git repo looks like this: 
![image](https://github.com/fastoch/Linux/assets/89261095/b3f23227-3ec6-4e5f-9a85-8ad2c0185a30)
![image](https://github.com/fastoch/Linux/assets/89261095/113b9725-cf2e-47e7-a408-f024756b6799)




@13/68

---
EOF
