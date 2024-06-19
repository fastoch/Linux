src = https://www.youtube.com/watch?v=YS5Zh7KExvE

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

- syntax: `ssh user@hostURL -p port` or `ssh user@IPaddress - port`
- then type the password en hit Enter
- you can also connect via an SSH key: `ssh -i sshkey_path user@host -p port`
- 




@15/88

---
EOF
