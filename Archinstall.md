https://www.youtube.com/watch?v=56u5tddLxCI

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
- select mirror region (type / to search for your region)
- check your locales settings (keyboard, language, encoding)
- disk config > manual partitioning > suggest partition layout
- bootloader > Grub
- profile > desktop > kde + all open-source graphics driver + sddm
- audio > pipewire
- kernel > linux-zen
- additional packages: firefox fastfetch fish
- network config > networkManager
- timezone > europe/Monaco
- automatic time sync > true
- optional repo > multilib
- install

---
EOF
