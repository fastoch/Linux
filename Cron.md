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
- "execute this cmd every sunday at 8:32" would be "32 8 * * 7".
- "execute this cmd at 3:00 on the 4th of July" would be "0 3 4 7 * cmd" 

We can also use this notation: "**@hourly** cmd", which means "run this cmd every hour"  
Or we could use **@reboot** to run a cmd on every reboot.  

## Running cron jobs as a specific user

Most of the time, you probably don't want to run a cron job as your current user.  
To edit the crontab for a different user: `sudo crontab -u username -e`  



@10min

---
EOF
