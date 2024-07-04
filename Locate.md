src: https://youtu.be/xg9GcuxeRwk

---

# Quickly find any file with the locate command

- run `which locate` to see if you have the locate command available
- if you need to install it, you have a choice between `mlocate` and `plocate`
  - actually, `mlocate` is now obsolete and `plocate` is much faster
  - to install it: `sudo pacman -S plocate`
- before using the `locate` command for the first time, run `sudo updatedb`
- then try to locate a random file with `locate <filename>`
- if your system uses the `plocate` package, then there's a cron job that will update the database periodically 



@7/10
