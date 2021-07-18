
## This Repository

Add following repository to `/etc/pacman.conf`

```
[BioArchLinux]
SigLevel = Never
Server = https://malacology.net/repo/
```
## Convert your VPS to ArchLinux

Choose Debian or CentOS as your Linux distribution for VPS and use vps2arch script.

Then you need to input the following command.

```
wget http://tinyurl.com/vps2arch && chmod +x vps2arch && ./vps2arch -mYOUR_MIRROR_URL
```

For example

```
wget http://tinyurl.com/vps2arch && chmod +x vps2arch && ./vps2arch -mhttps://mirrors.neusoft.edu.cn/archlinux/
```

When it finishes running, type the following

```
sync ; reoot -f
```