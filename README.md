# Installation

```
# open another console
alt + arrow key

# list fonts
ls /usr/share/kbd/consolefonts

# bigger font size
setfont ter-120b

# verify boot mode
cat /sys/firmware/efi/fw_platform_size # should output 64

# connect to internet using wifi
iwctl
device list
device wlan0 set-property Powered on
station wlan0 scan
station wlan0 get-networks
station wlan0 connect <SSID> 
<Ctrl-D>

# test internet
ping archlinux.org

# update timezone
timedatectl set-timezone <Tab>



```
