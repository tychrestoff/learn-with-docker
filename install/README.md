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

```bash
debian@n000:~$ ls
debian@n000:~$ docker daemon
FATA[0000] Error starting daemon: open /var/run/docker.pid: permission denied
```

Might need sudo.

```bash
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

```bash
debian@n000:~$ ls
debian@n000:~$ ps -a
...
...
17990 pts/1   00:00:00  docker
18022 pts/26  00:00:00  ps
debian@n000:~$
```

And now we can run Docker commands.

```bash
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

```bash
debian@n000:~$ ls
debian@n000:~$ docker run -it debian
root@86de93c0392f:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@86de93c0392f:/# rm -rf bin
root@86de93c0392f:/# ls
bash: /bin/ls: No such file or directory
root@86de93c0392f:/# exit
exit
debian@n000:~$ docker run -it debian
root@95e86d35f113:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@95e86d35f113:/# exit
exit
debian@n000:~$
```

### Some basic docker commands

Let's go over a few commands that are essential to **`docker`**'s typical usage. They are:

* **`docker daemon`** - Start the **`docker`** daemon
* **`docker build`** - Build a container image
* **`docker run`** - Run an image in a container over the daemon
* **`docker ps`** - List currently running containers
* **`docker pause`** - Pause a running container
* **`docker unpause`** - Unpause a paused container
* **`docker stop`** - Stop a running container
* **`docker start`** - Start a new container
* **`docker attach`** - Attach to a running container
* **`docker exec`** - Execute a command within a container

We can run through a quick example using all of these commands. We will start without the **`docker`** daemon running and get one running the same as before. This example assumes the daemon service needs **`sudo`** privledges.

```bash
debian@n000:~$ ls
debian@n000:~$ docker ps
Cannot connect to the Docker daemon. Is the docker daemon running on this host?
debian@n000:~$ ps -a
  PID TTY          TIME CMD
 4670 pts/10   00:00:00 ps
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

Start up another **`bash`** process without closing this one. We can see the **`docker`** daemon running and start using the CLI provided by **`docker`**.

```bash
debian@n000:~$ ps -a
  PID TTY          TIME CMD
 4702 pts/10   00:00:00 sudo
 4703 pts/10   00:00:00 docker
 4755 pts/0    00:00:00 ps
debian@n000:~$ cd install; ls
basic-cmds  README.md
debian@n000:~/install$ cd basic-cmds; ls
Dockerfile
debian@n000:~/install/basic-cmds$ docker build -t tychrestoff/learn:basic .
Sending build context to Docker daemon 2.048 kB
Step 1 : FROM tychrestoff/debian:jessie-min
 ---> 59041e6183de
Step 2 : MAINTAINER tychrestoff
 ---> Using cache
 ---> 59041e6183de
Successfully built 59041e6183de
debian@n000:~/install/basic-cmds$ docker run -it tychrestoff/learn:basic
root@dc0852114aed:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@dc0852114aed:/#
```
We list all processes to see Docker running and move to `~/install/basic-cmds` to see the `Dockerfile`. This `Dockerfile` is extremely simplified. It inherits from a minimal **`debian`** image stored in my Docker Hub. This image is about 125 MB. Above, you'll see that my particular machine has the image stored in cache, so my build will be nearly instant. It will take a few minutes on your machine depending on your bandwidth.

Next, open up yet another **`bash`** process and we'll see how the Docker CLI can manipulate running images.

```bash
debian@n000:~ docker ps
CONTAINER ID        IMAGE                     COMMAND             CREATED             STATUS              PORTS               NAMES
dc0852114aed        tychrestoff/learn:basic   "/bin/bash"         10 minutes ago      Up 10 minutes                           dreamy_tesla
debian@n000:~ docker pause dc0852114aed
dc0852114aed
debian@n000:~ docker ps
CONTAINER ID        IMAGE                     COMMAND             CREATED             STATUS                  PORTS                 NAMES
dc0852114aed        tychrestoff/learn:basic   "/bin/bash"         10 minutes ago      Up 10 minutes (Paused)                        dreamy_tesla
debian@n000:~ docker unpause dc0852114aed
dc0852114aed
debian@n000:~ docker stop dc0852114aed
dc0852114aed
debian@n000:~ docker ps
CONTAINER ID        IMAGE                     COMMAND             CREATED             STATUS                  PORTS        
debian@n000:~ docker start dc0852114aed
dc0852114aed
debian@n000:~ docker ps
CONTAINER ID        IMAGE                     COMMAND             CREATED             STATUS              PORTS               NAMES
dc0852114aed        tychrestoff/learn:basic   "/bin/bash"         16 minutes ago      Up 28 seconds                           dreamy_tesla
debian@n000:~ docker attach dc0852114aed
root@dc0852114aed:/#
root@dc0852114aed:/# exit
exit
debian@n000:~ docker start dc0852114aed
dc0852114aed
debian@n000:~ docker exec -it dc0852114aed /bin/bash
root@dc0852114aed:/# exit
exit
debian@n000:~ docker stop dc0852114aed
dc0852114aed
```

Whew. That was simple enough.
