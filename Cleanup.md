https://gist.github.com/rumansaleem/083187292632f5a7cbb4beee82fa5031  

## Check cache files and log files
`sudo du -sh ~/.cache/ /var/cache/ /var/log/journal/`

## Remove orphan packages
```
pacman -Qtdq
sudo pacman -R $(pacman -Qtdq)
```

## Cleanup log files
`sudo journalctl --vacuum-size=82M`

## Remove uninstalled packages + same thing for yay (AUR helper)
```
sudo pacman -Sc
yay -Sc
```

## Remove cache files older than 1 month
```
sudo find ~/.cache/ -type f -atime +30 -delete
sudo find /var/cache/ -type f -atime +30 -delete
```

---
EOF
