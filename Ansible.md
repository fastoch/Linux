src = https://www.youtube.com/watch?v=gIDywsGBqf4

---

# Using Ansible to automate your endpoints configuration

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
**Ansible-Pull** does the inverse. It runs on the **computer** you want to configure and pulls the config from the server to the computer.  

Ansible-pull will download the changes from a **Git** repository, and will then run everything in that repo against itself.  
Here's an example of a typical ansible-pull command: `ansible-pull -U https://github.com/fastoch/Ansible/mydesktopconfig.git`  

>[!important]
>The Git repo doesn't have to be hosted on Github. It can be hosted on a private Git server, in which case the URL will probably start with http instead of https.

The Git repo has to be structured a very specific way. We'll get into how to create such a repo and how to populate it with actual configs.

More details:
- https://www.youtube.com/watch?v=sn1HQq_GFNE
- https://www.devopsschool.com/blog/what-is-ansible-pull-and-how-can-we-use-it/

## Create a repository for Ansible-pull

- in our previous `ansible-pull` command example, we only specified the repository's URL, we did not specify which file to run
- the file that the `ansible-pull` command will run must be named `local.yml` and has to be located at the root of the repository

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
```

In this sample, you can see that:
- first of all, I'm going to target all hosts
- I'll run some pre_tasks against those hosts
  - pre_tasks are basically just tasks you want to run before anything else
    - the first one will run the pacman module (the package manager for Arch Linux)
    - it will use that module to update the cache
    - but it will only do that if the distribution it's being run on is actually Archlinux

So my Ansible config is actually going to check the distro before "deciding" if a task should be run or not.



@12/68

---
EOF
