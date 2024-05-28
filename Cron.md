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
- 
