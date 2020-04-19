# Guacamole-on-WSL-2
Setup Guacamole on WSL 2

Update the WSL 2 Linux kernel

https://docs.microsoft.com/en-us/windows/wsl/wsl2-kernel

Install Ubuntu

Convert to WSL 2

`wsl --set-version Ubuntu 2`

wsl --set-default-version 2

Set default Distribution to Ubuntu

`wsl --set-default Ubuntu`

Install Docker Desktop

## Update Ubuntu

`sudo su`

`apt-get update`

`sudo apt ugrade`

## Run Guacamole container

`docker run --name some-guacd -e ACCEPT_EULA=Y -d glyptodon/guacd`

`docker run --name some-guacamole -e ACCEPT_EULA=Y -e GUACD_HOSTNAME=some-guacd -e USER_MAPPING='cat user-mapping.xml' -p 8080:8080 -d glyptodon/guacamole`

apt install task-mate-desktop

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

## Download Guacamole client
`wget https://downloads.apache.org/guacamole/1.1.0/binary/guacamole-1.1.0.war`

`sudo mv guacamole-1.1.0.war /var/lib/tomcat8/webapps/guacamole.war`

 ## Configure Guacamole
 
 `sudo mkdir /usr/share/tomcat8/.guacamole`
 
 `ln -s /etc/guacamole/guacamole.properties /usr/share/tomcat8/.guacamole/` 
   
 
