# Tricks
```
# Suspend (must be outside of arch-chroot)
echo mem | tee /sys/power/state
```

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

# disk partition
fdisk /dev/nvme1n1 # 1Tb disk
# partition 1: 1Gb
# partition 2: 1Gb
# partition 3: remaining

# format
mkfs.ext4 /dev/nvme1n1p3
mkswap /dev/nvme1n1p2
mkfs.fat -F 32 /dev/nvme1n1p1

# mount
mount /dev/nvme1n1p3 /mnt
mount /dev/nvme1n1p1 /mnt/boot
swapon /dev/nvme1n1p2

# install packages
pacman-key --init # init keyring
pacstrap -K /mnt base linux linux-firmware

# configure filesystem
genfstab -U /mnt >> /mnt/etc/fstab

# change root
arch-chroot /mnt
pacman -S --noconfirm vim networkmanager

# time
ls -sf /usr/share/zoneinfo/Europe/Berlin /etc/localtime
hwclock -systohc

# localization
sed -i '/en_US.UTF-8 UTF-8/s/^#//g' /etc/locale.gen
locale-gen
echo 'LANG=en_US.UTF-8' > /etc/locale.conf

# hostname
echo 'p1' > /etc/hostname

# root password
passwd

# bootloader (must be inside arch-chroot)
pacman -S --noconfirm grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg

# reboot
<Ctrl-d>
reboot

```

# Inside Archlinux
```
# internet
systemctl start NetworkManager
systemctl enable NetworkManager
nmcli device wifi connect <SSID> password <password>

# pacman update
pacman -Syyu

# X
pacman -S --noconfirm xorg-server xorg-xinit

# add user
useradd -m bach
passwd bach

# AUR helper (run with user bach)
pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay-bin.git
cd yay-bin
makepkg -si

# display manager
yay -S --noconfirm ly
systemctl enable ly.service

# zsh
yay -S zsh
chsh -s /usr/bin/zsh

# polybar
yay -S --noconfirm polybar

# neovim
yay -S --noconfirm neovim cmake fzf ripgrep fd tmux nodejs npm lazygit

# others
yay -S --noconfirm openssh

```
