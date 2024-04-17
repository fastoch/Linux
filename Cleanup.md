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

## Remove uninstalled packages
