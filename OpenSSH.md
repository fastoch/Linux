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
  - if you get a **warning** stating that the target server's **fingerprint has changed**, you should **avoid confirming** the connection!!!
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








@30/88

---
EOF
