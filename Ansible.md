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
>My script uses ansible-pull very heavily. <br>
>Ansible-pull is installed automatically when you install Ansible.

## About Ansible-Pull

**Ansible** runs on an Ansible **server** which uses SSH to connect to all of your computers and configure them (the config is **pushed**).  
**Ansible-Pull** does the opposite. It runs on the **computer** you want to configure and **pulls** the config from the server.  

Ansible-pull will download the changes from a **Git** repository which must contain a special file named `local.yml`.  
Here's an example of a typical ansible-pull command: `ansible-pull -U https://github.com/fastoch/Ansible/mydesktopconfig.git`  

>[!important]
>The Git repo doesn't have to be hosted on Github. <br>
>It can be hosted on a local Git server, in which case the URL will probably start with http instead of https.

**The Git repo has to be structured in a very specific way**.  
We'll get into how to create such a repo and how to populate it with actual configs that will get pulled by your endpoints.

More details:
- https://www.youtube.com/watch?v=sn1HQq_GFNE
- https://www.devopsschool.com/blog/what-is-ansible-pull-and-how-can-we-use-it/

## How my repository looks like

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

---

- Then we have **roles**. Roles are explained in this video: https://www.youtube.com/watch?v=tq9sCeQNVYc
  - A role allows you to categorize tasks and run those tasks only against machines that are part of that role
  - The base role is for any machine, it's the base configuration I want to apply on all my hosts
  - if a machine is made a member of the workstation role, then it will get the workstation tasks run on it
  - and the same logic applies to the members of the server role

Every computer will get the base role first, no matter what, and then will either get the workstation role or the server role.  

---

### But how does Ansible know which role to apply?

- That has to do with another important file which also has to be present at the root of the Git repo
- Besides the `local.yml`, this other important file is `hosts`. This is an **inventory file**.
- Normally, with ansible-pull, you don't need an inventory file, but you can still use one

Here's a sample from a typical inventory `hosts` file:
```
[workstation]
arch-staging.home-network.io
debian-staging.home-network.io
demo.learn-linux.tv
kirin.home-network.io
```
All of the machines that are listed underneath [workstation] are part of that role.  
**That's how Ansible knows which role to apply.**  

---

### How does my local Git repo looks like
 
![image](https://github.com/fastoch/Linux/assets/89261095/b3f23227-3ec6-4e5f-9a85-8ad2c0185a30)  
As you can see, the **root** of the repo contains our `local.yml` and `hosts` files.  

![image](https://github.com/fastoch/Linux/assets/89261095/113b9725-cf2e-47e7-a408-f024756b6799)  
Inside the **roles** folder, we have the 3 roles that we've mentioned earlier.  

![image](https://github.com/fastoch/Linux/assets/89261095/f880ed37-ec1b-4a32-baed-e90066e9ffa1)  
Only the **tasks** folder is actually required in an Ansible role.  
This is the folder where you put the tasks that you want to be run when that role is being applied.  

And inside the tasks folder, we have a `main.yml` file, which is the only file that's required in a role.  
![image](https://github.com/fastoch/Linux/assets/89261095/9e126b14-ce2b-4f61-9af5-a3350dba9cb2)   

This `main.yml` file actually imports and runs a lot of other **.yml** files.  
And each of those .yml files corresponds to a specific task.  

For example: 
- `desktop_environments/gnome/keybindings.yml` is where I set all of my keyboard shortcuts
- `desktop_environments/gnome/appearance.yml` is where I set my wallpaper and other stuff
- `desktop_environments/gnome/terminal.yml` contains all settings for customizing my terminal
- `software/firefox.yml` will install firefox
- `software/docker.yml` will install Docker
- ...

You can pretty much configure anything using Ansible automation capabilities.  

---

## Setting up the Git repo

Now that you've seen how my config looks like, it's time for me to show you how to create your own.  

For this example, I will use Github, but you don't have to.  
It really doesn't matter where you host your code, so long as you have a Git repo that you can access from a terminal.  

- connect to your Github account
- create a brand new repository
- give it a meaningful name and a description
- decide whether or not you want to make this repo public
- add a README file
- create the repo

Now, you want to pull this repo down to your local machine.  
But before you can do that, you need to **create an OpenSSH key pair and then install Git** on your computer.  
This key pair won't be necessary when it comes to applying your Ansible code to your new device, but it does  
make it a lot easier **to interact with the repo itself** such as making commits and things like that.

To do that:
- open a terminal
- run `ssh-keygen -t ed25519`
  - as of OpenSSH version 9.5, Ed25519 keys are now the default type, so `ssh-keygen` is enough)
  - be careful where you save the key, as it might overwrite any existing key 
    - the default path being `~/.ssh/id_ed25519`, you can name your key differently if you already have one with this name
  - add a passphrase (recommended) and save it to your favorite password manager
- this will tell you where the key (pair) has been saved

Now, we need to give Github our public key (default name is id_ed25519.pub):
- first, install Git with `sudo pacman -S git`
- before we start using Git, we need to configure it:
  - `git config --global user.email "fastoch@ik.me`
  - `git config --global user.name "fastoch"`
- you can check your Git config with `cat ~/.gitconfig`
- now, go to your Github account, click on your profile icon at the top right and click on Settings
- in the left pane, go to SSH and GPG keys and click on New SSH key
- give your key a title and copy-paste the output of `cat ~/.ssh/id_ed25519.pub`
- click Add SSH key to associate it with your Github account

>[!note]
>The key starts with its type (ssh-ed25519) and ends with the comment (default is username@hostname). <br>
>When generating the key pair with `ssh-keygen`, you can use the -C option to customize the comment.

Now, to pull the repo from Github to your local machine (to clone the repo):
- go back to your Github repo home page
- click the Code button
- copy the SSH URL
- in your terminal, create a master folder to host all of your Git repositories: `mkdir ~/dev`
- `cd ~/dev`
- now you can clone the repo from Github to your local dev environment with `git clone <SSH_URL>` (paste the SSH URL)
- if it's the first time you're cloning a Github repo, you'll have to say yes and hit Enter to confirm the connection

Now, inside the dev folder, you should see a new folder with the same name as your repo.

## Making your first commit

- `cd ~/dev/ansible_repo/`
- `vim README.md`
- add some relevant information to describe the purpose of this repo
- write and quit with `:wq`
- if you run `git status`, it will show you that you have unstaged changes
- if your run `git diff README.md`, it will show you exactly what has changed since the last commit
- run `git add README.md` to stage the changes
- run `git commit -m "updated the readme file"` to commit and add some message to your commit
- another `git status` will tell you "nothing to commit, working tree clean"
- but you still need to publish your local commit to your Github repo with `git push origin main`
- now if you refresh the Github home page of your repo, you will see the changes 

>[!note]
>Staged means that you have marked a modified file in its current version to go into your next commit snapshot.

---

## Creating a playbook

Previously, we set up Git and we also created the repository that will host our Ansible files.  
Now we will see how to install Ansible and write some Ansible code to configure our computers.  

- install Ansible (on Arch Linux): `sudo pacman -S ansible`
- this will install 2 important binaries that you can find with:
  - `which ansible-playbook`
  - `which ansible-pull` 
- while being at the root of your repo: `vim local.yml`

Here's a basic example of a `local.yml` file:  
```yaml
---
- hosts: localhost
  connection: local
  become: true

  tasks:
  - name: install htop
    package:
      name: htop  
```

Commenting this `local.yml` file:
- the **indentation is very important** in a .yml file
- Every .yml file begins with three **dashes**.
- when you run ansible-playbook, you're targeting specific `hosts`, but since we're using ansible-pull, what we're essentially doing is
  pulling the repository down and running it against `localhost` (the computer we're currently using)
- line 3: Ansible uses OpenSSH by default, but since we're running this `locally` and pulling the repository down, we don't need SSH.
- `become` is enabling `sudo`, which is required to get admin privileges and make changes to our system

Now that we've created a basic `local.yml` file, we have to stage and commit this new file to our local repo, then push it to our Github repo.  
- `git add local.yml`
- `git commit -m "added local.yml"`
- `git push origin main`
- refresh your Github repo page to see the changes

>[!tip]
>If you're not familiar with Git yet, use `git status` as often as possible to show and understand how version control works.

### Why using ansible-pull instead of ansible-playbook?

When it comes to desktop and laptop configs, ansible-pull works a lot better than having an Ansible server.  
Because, for example, if it's a laptop that you're configuring, you could have the lid closed, it could be in your bag, which means it's not available
on the network. So an Ansible server wouldn't be able to reach it and would error out.  
But with ansible-pull, the machine will just pull down the config whenever it's online.  

## Testing my playbook

- run `which htop` to make sure it's not already installed on your computer
  - if it is, just replace the package name in your local.yml file with an application that is not already installed on your machine
- copy the HTTPS URL from Github
- run your ansible config with `sudo ansible-pull -U <HTTPS_URL>`

As previously explained, we don't need to provide the playbook name, as ansible-pull assumes that it's going to find a playbook named `local.yml`.

>[!important]
>We used the Github SSH URL previously to pull the repository down locally so we can work with it (`git clone`). <br>
>But to actually use this repo with `ansible-pull`, we should use the HTTPS URL.

## Improving our playbook

Here's our new `local.yml`:
```yaml
---
- hosts: localhost
  connection: local
  become: true

  tasks:
  - name: install htop, tmux & neofetch
    package:
      name:
        - htop
        - tmux
        - neofetch
```

- after modifying this file, write and quit
- stage it: `git add local.yml`
- commit: `commit -m "added additional packages"`
- push changes to Github: `git push origin main`

Now, you can run `sudo ansible-pull -U <HTTPS_URL>` to run the new playbook, which will install the 3 packages on your device.

---

Let's modify our `local.yml` file once again:
```yaml
---
- hosts: localhost
  connection: local
  become: true

  tasks:
  - name: install htop, tmux & neofetch
    package:
      name:
        - htop
        - tmux
        - neofetch

  - name: copy wallpaper file
    copy:
      src: files/wallpaper.png
      dest: /usr/share/backgrounds/ansible-wallpaper.png
      owner: root
      group: root

  - name: set wallpaper
    become_user: fastoch
    dconf:
      key: "/org/gnome/desktop/background/picture-uri"
      value: "'file:///usr/share/backgrounds/favorite-wallpaper.png'"

  - name: set wallpaper position
    become_user: fastoch
    dconf:
      key: "/org/gnome/desktop/background/picture-options"
      value: "'zoom'"
```

>[!note]
>URI = Uniform Resource Identifier <br>
>file:// is the prefix for the file protocol <br>
>file:/// is the prefix for the file protocol, plus a leading / pointing to the root directory <br>
>https://linuxconfig.org/introduction-to-the-dconf-configuration-system

Ansible has a ton of modules available:
- In our example, we use the `package` module to install 3 packages
- then, we use the `copy` module to copy a file
- finally, we use the `dconf` module twice
  - the dconf module needs to know which user to change the keys & values for, hence the `become_user` line
  - the dconf module depends on psutil Python library. To install it on Arch: `sudo pacman -S python-psutil`  

---

In order to make our new config works, we need to create a `files` directory and add our `wallpaper.png` in it:
- `cd ~/dev/ansible_repo/`
- `mkdir files`
- download a wallpaper you like and save it into `~/dev/ansible_repo/files/`
- stage those changes, commit and push to Github:
  - `git add files/`
  - `git commit -m "added wallpaper file"`
  - `git add local.yml`
  - `git commit -m "added wallpaper config"`
  - `git push origin main`

---

To stage & commit all changes: `git commit -am "some descriptive message"`  
This command will not add & commit new files though. See https://git-scm.com/docs/git-commit  

---

Once we have pushed the last changes to our Github Ansible repo, we can test our playbook against a new laptop:  
`sudo ansible-pull -U https//github.com/fastoch/ansible_laptop.git`  

You should be able to see the wallpaper change while the command is running.

---

Let's add one more thing to our Ansible playbook (at the very end of the file):  
```yaml
---
- hosts: localhost
  connection: local
  become: true

  tasks:
  - name: install htop, tmux & neofetch
    package:
      name:
        - htop
        - tmux
        - neofetch

  - name: copy wallpaper file
    copy:
      src: files/wallpaper.png
      dest: /usr/share/backgrounds/ansible-wallpaper.png
      owner: root
      group: root

  - name: set wallpaper
    become_user: fastoch
    dconf:
      key: "/org/gnome/desktop/background/picture-uri"
      value: "'file:///usr/share/backgrounds/favorite-wallpaper.png'"

  - name: set wallpaper position
    become_user: fastoch
    dconf:
      key: "/org/gnome/desktop/background/picture-options"
      value: "'zoom'"

  - name: copy .bashrc file
    copy:
      src: files/.bashrc
      dest: /home/fastoch/.bashrc
      owner: fastoch
      group: fastoch
```

We need to add a copy of our custom .bashrc file to the files/ folder:  
`cp ~/.bashrc ~/dev/ansible_repo/files/.bashrc`  

Then, we need to stage all changes: `git add .`  
Now, we can commit: `git commit -am "added .bashrc config"`  
Finally, publish that to Github: `git push origin main`  
And run the playbook: `sudo ansible-pull -U https://github.com/fastoch/ansible_laptop.git`  

---

Let's add some additional plays (=tasks):  
```yaml
---
- hosts: localhost
  connection: local
  become: true

  tasks:
  - name: install htop, tmux & neofetch
    package:
      name:
        - htop
        - tmux
        - neofetch

  - name: copy wallpaper file
    copy:
      src: files/wallpaper.png
      dest: /usr/share/backgrounds/ansible-wallpaper.png
      owner: root
      group: root

  - name: set wallpaper
    become_user: fastoch
    dconf:
      key: "/org/gnome/desktop/background/picture-uri"
      value: "'file:///usr/share/backgrounds/favorite-wallpaper.png'"

  - name: set wallpaper position
    become_user: fastoch
    dconf:
      key: "/org/gnome/desktop/background/picture-options"
      value: "'zoom'"

  - name: copy .bashrc file
    copy:
      src: files/.bashrc
      dest: /home/fastoch/.bashrc
      owner: fastoch
      group: fastoch

  - name: add ansible user
    user:
      name: velociraptor
      system: yes

  - name: set up sudo for ansible user
    copy:
      src:
      dest:
      owner:
      group:
      mode: 

  - name: add ansible-pull cron job
    cron:
      
```

Commenting the 3 new tasks:
- we add a system user (velociraptor) to run Ansible automatically any time we commit changes to the repo



@60/68

---
EOF
