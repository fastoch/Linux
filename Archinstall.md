Source = https://www.youtube.com/watch?v=WksxVLrALhg

---

# Change Keyboard Layout

https://wiki.archlinux.org/title/Linux_console/Keyboard_configuration

- List all available keymaps: `localectl list-keymaps`
- Permanently change the keymap: `localectl set-keymap fr-pc`

Test the keyboard layout to make sure you've selected the right one.  

# Connect to the Internet

https://wiki.archlinux.org/title/iwd

- `iwctl`
- `device list`
- `station wlan0 scan`
- `station wlan0 connect *SSID*`
- enter your Wi-Fi password
- `exit`

# Edit pacman.conf

`sudo vim /etc/pacman.conf`  

- Go to Misc options
- Enter insert mode by pressing i and remove the # (comment) to activate:
  - color
  - VerbosePkgLists
  - ParallelDownloads (increase installation speed) 
- Write and quit (hit Esc to quit insert mode, then type :wq and press Enter)

# Running the archinstall script

- `archinstall`
- select mirror region 
- check your locales settings (keyboard fr-pc, language en_GB, encoding utf-8)
- disk config > manual partitioning > suggest partition layout
- bootloader > Grub
- profile > desktop > kde (desktop environment) + all open-source graphics driver + sddm (login manager)
- audio > pipewire
- kernel > linux-zen
- additional packages: firefox fish
- network config > networkManager
- timezone > europe/Monaco
- automatic time sync > true
- optional repo > multilib
- install

---

# Troubleshooting archinstall

If the ISO on your USB drive does not contain an up-to-date version of archinstall:
- update the keyring: `pacman -Sy archlinux-keyring`
- if the keyring update fails: https://bbs.archlinux.org/viewtopic.php?id=293529

- update archinstall: `pacman -Sy archinstall`
- check version by trying to reupdate archinstall (should be 2.8 or later)

- if python version on the ISO is outdated, run: 
`pacman -S python-simple-term-menu python-pyparted python archinstall`

- if you want to clean the disk before installing Arch, run `lsblk -a` to find your disk name,
  and then `cfdisk /dev/disk_name` . Delete all partitions and write changes before exiting cfdisk.

Then you can run archinstall.

---

# Post-install tweaks

- install kcalc
- install spectacle
- make fish the default shell
- install neofetch
- install qBittorrent
- install ufw or firewalld


---
EOF
