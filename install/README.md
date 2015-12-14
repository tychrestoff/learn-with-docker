# 1 Starting out

### Install

![Docker Engine logo](https://www.docker.com/sites/default/files/products/product%20-%20engine.png "Docker Engine logo")

To begin, we'll need to create an environment where we can run [**Docker Engine**](https://www.docker.com/docker-engine). Docker Engine is a runtime for Docker containers. The engine runs on Linux, so non-Linux (OS X, Windows) operating systems will run Docker Engine in a Linux virtual machine. See [**here**](https://docs.docker.com/engine/installation/) for official Docker Engine installation documentation.

##### Docker runs almost everywhere.
  * [**Mac**](https://docs.docker.com/engine/installation/mac/) and [**Windows**](https://docs.docker.com/engine/installation/windows/)
  * **Linux**
    * [**Arch**](https://docs.docker.com/engine/installation/archlinux/)
    * [**CentOS**](https://docs.docker.com/engine/installation/centos/)
    * [**CRUX**](https://docs.docker.com/engine/installation/cruxlinux/)
    * [**Debian**](https://docs.docker.com/engine/installation/debian/)
    * [**Fedora**](https://docs.docker.com/engine/installation/fedora/)
    * [**FrugalWare**](https://docs.docker.com/engine/installation/frugalware/)
    * [**Gentoo**](https://docs.docker.com/engine/installation/gentoolinux/)
    * [**Oracle**](https://docs.docker.com/engine/installation/oracle/)
    * [**Red Hat**](https://docs.docker.com/engine/installation/rhel/)
    * [**openSUSE**](https://docs.docker.com/engine/installation/SUSE/)
    * [**Ubuntu**](https://docs.docker.com/engine/installation/ubuntulinux/)
  * **Cloud**
    * [**Amazon EC2**](https://docs.docker.com/engine/installation/amazon/)
    * [**Google Cloud Platform**](https://docs.docker.com/engine/installation/google/)
    * [**IBM SoftLayer**](https://docs.docker.com/engine/installation/softlayer/)
    * [**Joyent Public Cloud**](https://docs.docker.com/engine/installation/joyent/)
    * [**Microsoft Azure**](https://docs.docker.com/engine/installation/azure/)
    * [**Rackspace Cloud**](https://docs.docker.com/engine/installation/rackspace/)
    
### Getting it to work
    
On Linux, Docker Engine runs locally, so those using a Linux machine will need to start the daemon manually.

It's easy.

```
debian@n000:~$ ls
debian@n000:~$ docker daemon
FATA[0000] Error starting daemon: open /var/run/docker.pid: permission denied
```

Might need sudo.

```
debian@n000:~$ ls
debian@n000:~$ sudo docker daemon
[sudo] password for debian:
INFO[0000] API listen on /var/run/docker.sock
INFO[0000] [graphdriver] using prior storage driver "aufs"
INFO[0000] Firewalld running: false
INFO[0000] Default bridge (docker0) is assigned with an IP address 172.17.0.1/16. Daemon option 
--bip can be used to set a preferred IP address
INFO[0000] Loading containers: start.

...

INFO[0000] Loading containers: done.
INFO[0000] Daemon has completed initialization
INFO[0000] Docker daemon commit=a39484d execdriver=native-0.2 graphdriver=aufs version=1.9.1

```

Should be able to see a `docker` process running now.

```
debian@n000:~$ ls
debian@n000:~$ ps -a
...
...
17990 pts/1   00:00:00  docker
18022 pts/26  00:00:00  ps
debian@n000:~$
```

And now we can run Docker commands.

```
debian@n000:~$ ls
debian@n000:~$ docker ps
CONTAINER ID    IMAGE     COMMAND     CREATED     STATUS      PORTS     NAMES
debian@n000:~$ docker images debian
REPOSITORY    TAG     IMAGE ID      CREATED     VIRTUAL SIZE
debian        wheezy  3ba3ba0cdebd  9 days ago  84.89 MB
debian        7.9     3ba3ba0cdebd  9 days ago  84.89 MB
debian        8.2     23cb15b0fcec  9 days ago  125.1 MB
debian        jessie  23cb15b0fcec  9 days ago  125.1 MB
debian        latest  23cb15b0fcec  9 days ago  125.1 MB
debian@n000:~$
```

And to demonstrate the sandboxed environment that containers provide, here we run the `debian` image, recursively force the removal of the `/bin` folders contents, show that the `/bin` files have been destroyed, exit the corrupt image, and start a brand-new `debian` image with a brand-new `/bin` folder.

```
debian@n000:~$ ls
debian@n000:~$ docker run -it debian
root@86de93c0392f:/# ls
bin boot dev etc home lib lib64 media mnt opt proc root run sbin srv sys tmp usr var
root@86de93c0392f:/# rm -rf bin
root@86de93c0392f:/# ls
bash: /bin/ls: No such file or directory
root@86de93c0392f:/# exit
exit
debian@n000:~$ docker run -it debian
root@95e86d35f113:/# ls
bin boot dev etc home lib lib64 media mnt opt proc root run sbin srv sys tmp usr var
root@95e86d35f113:/# exit
exit
debian@n000:~$
```
