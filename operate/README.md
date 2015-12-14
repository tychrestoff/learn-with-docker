# 2 Operate

### Debian

![Debian logo](https://www.openmodelica.org/images/debian_splash.png "Debian logo")

#### About Debian
Debian is a Unix-like computer operating system and a Linux distribution composed of free and open-source software. The operating system was announced in 1993 by Ian Murdock and the first stable release made in 1996.

#### Debian Directory Tree

Debian's directory and file naming use the Filesystem Hierarchy Standard. The root level directory is represented by a slash `/`. Additional Debian directories are:
```
Directory   Content
bin         Essential command binaries
boo         Static boot loader files
dev         Device files
etc         Host system configuration
home        User home directories
lib         Essential shared libraries and kernel modules
lib64       64-bit lib
media       Contains mount points for replaceable media
mnt         Mount point for mounting a file system temporarily
opt         Add-on application software packages
proc        Virtual directory for system information
root        Home directory for root user
run         Files with runtime information
sbin        Essential system binaries
srv         Data for services provided by the system
sys         Virtual directory for system information
tmp         Temporary files
usr         Secondary Hierarchy
var         Variable data
```

#### Apt

Debian uses [**`apt`**](https://wiki.debian.org/Apt), the Advanced Package Tool, to handle the installation and removal of software. It automates the retrieval, configuration, and installation of software packages from precompiled files or by compiling source. [**`apt-get`**](https://wiki.debian.org/apt-get) is a CLI that works with **`apt`** to fetch, install, and update packages in Debian.
