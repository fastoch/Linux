source = https://www.youtube.com/watch?v=xsdFNpThetE  

# SSH failures

**symptom** = the user is not able to log in to a server via SSH  

First thing is to check if the user is the only one who cannot connect to the server.  

If he's the only one, try tailing the authentication logs: `tail -f /var/log/auth.log`.  
Depending on your distro, logs might be saved in a different file. Once you've found the correct log file, tail it and ask the user to try again.  

# Slow performance in Gnome 

Gnome is the default desktop environment for Ubuntu, Fedora, and many other distributions.  



