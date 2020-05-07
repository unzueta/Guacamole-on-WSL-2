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

## To manually start:

`sudo /etc/init.d/dbus start`

`sudo /etc/init.d/avahi-daemon start`

## To run on startup:

`sudo apt install policykit-1 daemonize`

`sudo touch /usr/bin/bash`

`sudo chmod +x /usr/bin/bash`

`sudo nano /usr/bin/bash`

Add the following lines. Replace WSL_USER with your user:

```
#!/bin/bash
# your WSL2 username
UNAME="<WSL_USER>"

UUID=$(id -u "${UNAME}")
UGID=$(id -g "${UNAME}")
UHOME=$(getent passwd "${UNAME}" | cut -d: -f6)
USHELL=$(getent passwd "${UNAME}" | cut -d: -f7)

if [[ -p /dev/stdin || "${BASH_ARGC}" > 0 && "${BASH_ARGV[1]}" != "-c" ]]; then
    USHELL=/bin/bash
fi

if [[ "${PWD}" = "/root" ]]; then
    cd "${UHOME}"
fi

# get pid of systemd
SYSTEMD_PID=$(pgrep -xo systemd)

# if we're already in the systemd environment
if [[ "${SYSTEMD_PID}" -eq "1" ]]; then
    exec "${USHELL}" "$@"
fi

# start systemd if not started
/usr/sbin/daemonize -l "${HOME}/.systemd.lock" /usr/bin/unshare -fp --mount-proc /lib/systemd/systemd --system-unit=basic.target 2>/dev/null
# wait for systemd to start
while [[ "${SYSTEMD_PID}" = "" ]]; do
    sleep 0.05
    SYSTEMD_PID=$(pgrep -xo systemd)
done

# enter systemd namespace
exec /usr/bin/nsenter -t "${SYSTEMD_PID}" -m -p --wd="${PWD}" /sbin/runuser -s "${USHELL}" "${UNAME}" -- "${@}"
```

`sudo editor /etc/passwd`

Replace the line 

```
root:x:0:0:root:/root:/bin/bash
```

with

```
root:x:0:0:root:/root:/usr/bin/bash
```

`sudo touch /etc/rc.local`

`sudo chmod +x /etc/rc.local`

`sudo nano /etc/rc.local`

Copy the following lines:

```
#!/bin/sh -e

/etc/init.d/avahi-daemon start

exit 0
```

In a elevated command prompt in Windows:

```
C:\ wsl --shutdown
```

```
C:\ ubuntu config --default-user root
```
