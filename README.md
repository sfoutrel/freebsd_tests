# freebsd_tests
Bits and tricks for FreeBSD tests

# Installation
## How to FreedBSD
System components install:
 - lib32

Partition: 
- Auto ZFS
- Encrypt disk (optional)

Network:
- ipv4 ok
- ipv6 ok

Time & Timezone:
- up to you

Services:
- sshd
- ntpd
- ntp on boot

Hardening:
- random_pid
- clear_tmp

Users:
- Add a user for you !
Add your user to group wheel

Install:
- Let install and reboot

First boot:

login as root.

First config is easier if made from root.
FreeBSD usage is different from GNU/Linux
you leave the base system intact and you put your version of config files in /usr/local/etc
updating packages informations
`pkg update`

## Creating our pkg config
### https://docs.freebsd.org/en/books/handbook/ports/#ports
`mkdir -p /usr/local/etc/pkg/repos;
cp /etc/pkg/FreeBSD.conf /usr/local/etc/pkg/repos/`

edit FreeBSD.conf and replace quarterly by latest (we love taking risks)

`pkg update;pkg install doas # (replacement for sudo)`

`echo "permit nopass keepenv :wheel" >/usr/local/etc/doas.conf`

we will use fish as login sheel because it's cool.

`doas pkg install fish`

## setting fish as our login shell
`doas pw usermod <loginname> -s /usr/local/bin/fish`

## Installing smart to monitor our disks

`doas pkg install smartmontools;
doas sysrc smartd_enable="YES";
cd /usr/local/etc;
doas cp smartd.conf.sample smartd.conf;
doas service smartd start`

# Basic install done !
## We can continue to boot environment with bectl
`doas bectl list
doas bectl create before-upgrade-fbsd
doas bectl activate before-upgrade-fbsd
doas reboot
doas freebsd-upgrade fetch
doas freebsd-upgrade install`

# Network
## Wired
`doas sysrc ifconfig_em0="DHCP"`

`doas sysrc background_dhclient="YES"`

## Wifi

`echo "network={ ssid="myssid" psk="mypsk" }" > /etc/wpa_supplicant.conf`

`doas sysrc wlans_iwn0="wlan0"`

`doas sysrc ifconfig_wlan0="WPA DHCP"`

`doas service netif restart`

# XWindow !
### https://docs.freebsd.org/en/books/handbook/x11/#x11-synopsis
`doas pkg install xfce xorg xdm drm-kmod;
doas pkg install xf86-video-scfb;
doas pw groupmod video -m sfoutrel`

pkg install virtualbox-ose-additions
doas sysrc vboxservice_enable=YES
doas sysrc vboxguest_enable=YES
