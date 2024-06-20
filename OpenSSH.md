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
  - if you get a warning stating that the target server's fingerprint has changed, you should avoid confirming the connection!!!
-  to view ssh logs: https://www.strongdm.com/blog/view-ssh-logs





@20/88

---
EOF
