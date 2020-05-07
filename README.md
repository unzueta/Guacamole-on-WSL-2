# Guacamole-on-WSL-2
Setup Guacamole on WSL 2

Update the WSL 2 Linux kernel

https://docs.microsoft.com/en-us/windows/wsl/wsl2-kernel

Set default to WSL 2

`wsl --set-default-version 2`

Install Ubuntu

Set default Distribution to Ubuntu

`wsl --set-default Ubuntu`

Install Docker Desktop

## Update Ubuntu

`sudo su`

`apt-get update`

`sudo apt ugrade`

## Create firewall rule to allow WSL ip port 8080 on public profile network 

## Create docker-compose.yml

`nano docker-compose.yml`

```
version: "3"
services:

    guacd:
        image: glyptodon/guacd
        environment:
            ACCEPT_EULA: Y

    guacamole:
        image: glyptodon/guacamole
        ports:
            - "8080:8080"
            - "5901:5901"
            - "3389:3389"
        environment:
            ACCEPT_EULA: Y
            GUACD_HOSTNAME: guacd
            USER_MAPPING: |
                <user-mapping>
                    <authorize username="user" password="userPassword">
                        <connection name="UbuntuRDP">
                          <protocol>rdp</protocol>
                          <param name="hostname">172.18.25.37</param>
                          <param name="port">3389</param>
                          <param name="console">true</param>
                        </connection> 
                        <connection name="VNC 2">
                            <protocol>vnc</protocol>
                            <param name="hostname">172.18.2.7</param>
                            <param name="port">5901</param>
                            <param name="password">vncPassword</param>
                        </connection>
                    </authorize>
                </user-mapping>
```
Run docker-compose

`docker-compose up`

# Set the hostname of your WSL machine to WSL.local

`sudo apt-get install -y avahi-daemon`

`sudo nano /etc/avahi/avahi-daemon.conf`

```
[server]
host-name=WSL
```

To manually start:

`sudo /etc/init.d/dbus start`

`sudo /etc/init.d/avahi-daemon start`
