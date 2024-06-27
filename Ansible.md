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
Here's an example of an ansible-pull command: `ansible-pull -U https://github.com/fastoch/Ansible/mydesktopconfig.git`  

>[!important]
>The Git repo doesn't have to be hosted on Github. It can be hosted on a private Git server, in which case the URL will probably start with http instead of https.

The Git repo has to be structured a very specific way. We'll get into how to create such a repo and how to populate it with actual configs.

More details:
- https://www.youtube.com/watch?v=sn1HQq_GFNE
- https://www.devopsschool.com/blog/what-is-ansible-pull-and-how-can-we-use-it/



@10/68

---
EOF
