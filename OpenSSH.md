## Sources  

- https://www.youtube.com/watch?v=YS5Zh7KExvE
- https://wiki.archlinux.org/title/OpenSSH
- https://wiki.archlinux.org/title/SSH_keys

---

# What is OpenSSH?

- It's a remote management tool that gives you access to run commands on another machine.
- It was developed by the OpenBSD project
- It's the closest thing to a standard that we Linux people have when it comes to remote access
- It's a suite of utilities (multiple binaries), the most important of which are the server and client components
- By default, openSSH uses port 22

>[!note]
>OpenBSD est un système d'exploitation libre de type Unix, dérivé de 4.4BSD

# Connecting to a server via OpenSSH

- syntax: `ssh user@hostURL -p port` or `ssh user@hostIPaddress - port`
- then type the password en hit Enter
- you can also connect via an SSH key: `ssh -i sshkey_path user@host -p port`
- By default, ssh keys are stored in the `~/.ssh/` directory
- in this directory, there's a file named `known_hosts`
  - the first time you connect to a server, it's going to ask you "are you sure you want to connect to that machine?"
  - when you say "yes", it's going to save the remote server's fingerprint in this file, so it won't ask you to confirm next time
  - these fingerprints are also there for security reasons
  - if you get a **warning** stating that the target server's **fingerprint has changed**, you should **avoid confirming** the connection!
-  to view ssh logs: https://www.strongdm.com/blog/view-ssh-logs

# Configuring an OpenSSH client

In this section, we'll see how to create a config file that we can use with an SSH client to greatly simplify our future connections.  

In our ~/.ssh directory, we have our ssh keys and our know_hosts file.  
But there's another file that we can create that's not created by default: `touch ~/.ssh/config`  

Then we can edit our file as in the following example: `vim ~/.ssh/config`
```
Host server1
  Hostname 172.105.7.26
  Port 22
  User root
```
The line specifying the port is not necessary here, since 22 is the default port for SSH connections.  

Then, instead of the usual ssh command, I can run this: `ssh server1`  

We can include multiple servers in our config file:
```
Host server1
  Hostname 172.105.7.26
  Port 22
  User root

Host server2
  Hostname 172.16.249.6
  Port 2222
  User toto
```

# Using public/private keys

Using SSH keys allows for greater **security** and additional **convenience**.  
This allows us to avoid having to type a password.  

The downside to that though is if we fail the key or we don't have it, it will still fail over to ask us for the pwd.  
Which means a hacker can still try to guess the pwd.
In a later section, we'll see how to **disable pwd authentication**.  

## Create an SSH key 

>[!warning]
>Always check your keys before creating a new one, so you don't overwrite an existing key!
>If you overwrite a key that is your only way into a server, you won't be able to access this host anymore!

- Run `ssh-keygen`
- specify where to save the file
- enter a passphrase if you want to further improve the security (makes the key useless without the passphrase)

When you generate a new ssh key, it actually creates a key pair: a private key and its associated public key.  
We can now add the public key on the server end, which will allow us to connect to this server by providing the associated private key.  

## Add the key pair to the remote server

- display the public key `cat id_rsa.pub` (the key name depends on the type of algorithm)
- copy the key (select the output and ctrl + shift + c)
- connect to the server: `ssh servername`
- create an .ssh directory if needed: `mkdir .ssh`
- `cd .ssh`
- `vim authorized_keys`
- paste your public key (ctrl + shift + v)
- save the file (:wq + Enter)

When I use the ssh command to connect to the server, it checks my private key against the public key on the remote end.  
And if the mathematical link between those keys is correct, I'll be allowed to log in.  
If I've set a passphrase when creating my key, I'll have to enter it.

There's a better way to copy the public key to the target server. The syntax is:
- `ssh-copy-id -i <input_file> username@servername`
- example: `ssh-copy-id -i ~/.ssh/id_rsa.pub root@server1`
- Then it confirms that your key has been added to the remote server
- And you can now log into this server by running `ssh username@servername`

Note that the `ssh-copy-id` command will create the `.ssh` directory on the remote server if it's not already there.  
And it will also create a copy of the `authorized_keys` file, populated with the public key I've just copied.

# Managing SSH keys

## Create a key pair for a specific server

- Run `ssh-keygen -t ed25519 -C "some comment"`  
- The -t flag is for choosing the type of algorithm
- The -C flag is for adding a comment so you can remember what this key will be used for
- This type of key (ed25519) is actually more secure than the default RSA
- not only is it more secure, but the public key is going to be noticeably shorter as well
- when it's asking you where to save the key, give the file a name that will help you remember what this key is for
  - for example: `/home/fastoch/.ssh/server16_id_ed25519`
- then enter a passphrase so that you can disable password authentication later on
- Keep in mind that the key name won't appear once it gets copied to the `authorized_keys` file, only the comment will be visible

>[!important]
>As of OpenSSH version 9.5 (current version is 9.7), the `ssh-keygen` cmd will generate keys using the Ed25519 algorithm by default.
  >https://www.youtube.com/watch?v=tdfBbpJPTGc

## Caching the key

To avoid having to enter the passphrase every single time you want to connect, you can cache the key via the **ssh agent**.  
The key will remain cached until you exit the terminal session or close the terminal window.  

- Start the ssh agent: `eval "$(ssh-agent)"`
- the previous cmd gives you the PID (process id) of the ssh agent
- add a key to the ssh agent: `ssh-add <path to the private key>`
- this cmd will ask for the passphrase

>[!important]
>if you're using a desktop Linux distro, the ssh agent is automatically running in the background and will unlock your key as soon
>as you log in to the desktop. But if you're logged into a server, you don't have a GUI and you need to activate the ssh agent manually.

# SSH server configuration

So far we've been using the ssh client, but now it's time to focus a bit more on the server.  

- First of all, we need to ensure the ssh server component is running on the remote machine
  - most Linux distros have the sshd binary to represent the server side (ssh daemon)
  - run `which sshd` to know which binary is used on your machine, most probably /usr/bin/sshd or /usr/sbin/sshd
  - to check if the ssh daemon is running: `systemctl status sshd`
  - if it's not running: `systemctl start sshd`
  - and to make sure that sshd gets started at boot: `systemctl enable sshd`
- to configure the server side: `sudo vim /etc/ssh/sshd_config`
  - a # symbol in front of a line means it's commented out and therefore not applied
  - in this file, you can **change the port number**, which defaults to port 22
  - you can set **PermitRootLogin** to **no** if you already have another user that you can login with via ssh
  - you can also set **PasswordAuthentication** to **no** if you already configured a key-based authentication
    - **!!!make sure that you can log in with your ssh key before you actually set this to no!!!**
- after you modified the config file, restart the ssh service: `systemctl restart sshd`
  - make sure it didn't fail: `systemctl status sshd`
  - NOTE that existing connections are not terminated when you restart sshd
- then we need to test the config changes and verify the ssh connections still work
  - DO NOT disconnect (ctrl + d) from your current session on the server until you've checked the new config
  - open a new terminal window and try to ssh into the server, specify the port if you've changed it
  - if the connection succeeds, then you can disconnect without risk
- finally, **backup your ssh keys** somewhere safe, because if you lose them, you won't be able to access your servers anymore

## Host keys

Inside the /etc/ssh folder, we have other important files = **host keys**: https://www.ssh.com/academy/ssh/host-key
  - **DO NOT delete the host keys** stored on a server, or you won't be able to reconnect to this server
  - if you need to clone a server, be aware that those host keys need to be changed
    - your ssh client will be very confused if all your servers share the same host keys
    - it will think that same host keys = same server, then won't understand why the IP address & other things are different
    - then it will suspect a man-in-the-middle attack

>[!important]
>no matter what, I highly recommend that you use keys and disable pwd authentication on all of your servers.

# Troubleshooting

## Check the network layer

- first, you need the openSSH client installed on your local computer
- then, port 22 (or whichever port you've configured on the remote server) needs to be allowed on:
  - the remote router
  - the remote firewall
  - the remote switch
- you also need to **enable port forwarding** on these network devices

## invalid permissions

- if permissions are too open, then ssh will not allow 



---
EOF
