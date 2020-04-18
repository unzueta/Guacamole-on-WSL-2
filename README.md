# Guacamole-on-WSL-2
Setup Guacamole on WSL 2

#Update the WSL 2 Linux kernel (optional)

https://docs.microsoft.com/en-us/windows/wsl/wsl2-kernel

## Update Ubuntu

`sudo su`

`apt-get update`

`sudo apt ugrade`

## Install the Gnome Desktop

`apt-get remove blueman`

`dpkg-reconfigure dbus && service dbus restart`

Install Gnome and select gdm3

`apt install ubuntu-gnome-desktop`

## Install Tiger Vnc

`apt-get install tigervnc-standalone-server tigervnc-xorg-extension tigervnc-viewer tigervnc-scraping-server`

## Setup Vnc password

Exit root 

`exit`

`vncpassword`

Rename the existing .Xauthority file (if exists) by running the following command

mv .Xauthority old.Xauthority 

xauth with complain unless ~/.Xauthority exists

touch ~/.Xauthority

Create a file name ~/.vnc/xstartup

`nano ~/.vnc/xstartup`

Paste the following lines

```
#!/bin/sh
# Start Gnome 3 Desktop 
[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
vncconfig -iconic &
dbus-launch --exit-with-session gnome-session &
```

