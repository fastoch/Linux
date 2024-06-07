src = https://www.youtube.com/watch?v=7cbP7fzn0D8

# Scheduling tasks with cron

## Setting up cron jobs

- Every user on the system will have their own set of cron jobs, which we call a **crontab**
- To show the crontab for the current user: `crontab -l`
- To set up a cron job:
  - `crontab -e`
  - then select your favorite text editor
  - go to the very end of the file and add a new cron job

A cron job consists of 6 fields:
- minute (m)
- hour (h)
- day of month (dom)
- month (mon)
- day of the week (dow)
- command

Setting the value of a field to an asterisk * means "every" or "any".  
For example:  
- "execute this cmd every day at 9:05" would be "5 9 * * * cmd".
- "execute this cmd every sunday at 8:32" would be "32 8 * * 7 cmd".
- "execute this cmd at 3:00 on the 4th of July" would be "0 3 4 7 * cmd" 

We can also use this notation: "**@hourly** cmd", which means "run this cmd every hour".  
Or we could use **@reboot** to run a cmd on every reboot.  

## Running cron jobs as a specific user

Most of the time, you probably don't want to run a cron job as your current user.  
- To edit the crontab for a different user: `sudo crontab -u username -e`  
- To check the cron jobs of another user: `sudo crontab -u username -l`

## Running cron jobs as a Web server

Let's see a more practical example. For this, we will install Apache: `sudo pacman -S apache`  
https://archlinux.org/packages/?name=apache  

To check it installed successfully, open a Web browser and browse to **localhost**.  
You should get the default Apache start page.  

Now that I have Apache installed, I should have a Web server user as well.  
To check that: `cat /etc/passwd | grep www`  
This command should return a user named **www-data**.  

### Example

When you deploy a Website or a Web application, there might be a cron job that you'd like to run at regular intervals.  
- maybe you're backing up the Website every night
- or the Website itself has some sort of PHP script that should run once a week

There's all kinds of reasons why you might want a cron job when running a Web server.  
So, we're going to add a cron job to the **www-data** user.  

If you try to switch to this user with `su www-data`, you will get a message telling you that this account is not available.  
That's because the shell for that specific user is set to '**nologin**'.  

A system user, by default, can't log in to the system.  
**You really don't want anyone to log in as www-data.**  
If someone was able to break in and to log in as that user, they could actually delete our entire Website, they could  
insert some malware, they could do all kinds of nasty things.  

But even though I can't log in to this account, I can still add a cron job to it.  
`sudo crontab -u www-data -e`  

Then I scroll all the way down and add a cron job to my crontab.  
I could decide to run a script every night at 3:00 AM: `0 3 * * * /usr/local/bin/website_backup.sh`  

### Always include the full path 

In my previous example, notice that I've included the **full path** to the script. That's very **important**.  
Any time you are using cron, you should **always include the full path to the executable**.  

On the command line, to execute this script manually, I don't have to type out the entire path.  
That's because your **environment variable** named **PATH** includes /usr/local/bin by default, so your shell already knows  
to look there for commands.

>[!tip]
>you can check the PATH variable for the current user with `echo $PATH`

But when it comes to cron, the user you are running your cron job with may not have a full PATH available, maybe  
/usr/local/bin is not part of that user's PATH, maybe their shell is not even checking the PATH variable.  

## Useful Websites

- https://crontab-generator.org/
- 




---
EOF
