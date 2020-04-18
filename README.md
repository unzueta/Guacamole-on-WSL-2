# Guacamole-on-WSL-2
Setup Guacamole on WSL 2

Update the WSL 2 Linux kernel (optional)

https://docs.microsoft.com/en-us/windows/wsl/wsl2-kernel

Install Ubuntu

Convert to WSL 2

`wsl --set-version Ubuntu 2`

## Update Ubuntu

`sudo su`

`apt-get update`

`sudo apt ugrade`

## Install Vnc Server

apt-get install vnc4server

## Install xfce4

apt-get install xfce4

## Setup Vnc password

Exit root 

`exit`

`vncpasswd`

Configure the Vnc startup script

`cd`

`nano ~/.vnc/xtartup`

Paste the following lines:

```
#!/bin/sh
xrdb $HOME/.Xresources
xsetroot -solid grey
startxfce4 &
```
## Install Apache2

`sudo apt-get install apache2`

`sudo apt-get install libxml2-dev`

## Install Guacamole

`sudo add-apt-repository ppa:guacamole/stable`

`sudo apt-get install guacamole-tomcat`

# Configure Apache

`sudo a2enmod`

`proxy proxy_ajp proxy_http rewrite deflate headers proxy_balancer proxy_connect proxy_html`

`sudo nano /etc/apache2/sites-available/000-default.conf`

Paste the following lines:

```
<VirtualHost *:80>
# /guacamole settings
ProxyPass /guacamole http://localhost:8080/guacamole
ProxyPassReverse /guacamole http://localhost:8080/guacamole
<Location /guacamole>
    Order allow,deny
    Allow from all
</Location>
</VirtualHost> 
```
