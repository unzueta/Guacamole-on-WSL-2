# Guacamole-on-WSL-2
Setup Guacamole on WSL 2

## Update Ubuntu

`sudo su`

`apt-get update`

`sudo apt ugrade`

## Install Mate Desktop

`apt-get remove blueman`

`apt install ubuntu-mate-desktop`

 Define MATE as your default desktop for all users

`bash -c 'echo PREFERRED=/usr/bin/mate-session > /etc/sysconfig/desktop'`

