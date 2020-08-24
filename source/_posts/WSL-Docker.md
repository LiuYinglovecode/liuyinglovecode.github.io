---
title: configure apache in docker while docker is in WSL
date: 2020-1-3 09:04:21
tags:
- WSL
- SSH
- apache
categories:
- WSL
---
### Apache
``` bash
$ mkdir -p  mkdir -p  ~/apache/www ~/apache/logs ~/apache/conf
$ cd apache
~/apache$ touch Dockerfile
~/apache$ vi Dockerfile
~/apache$ touch httpd-foreground
~/apache$ vi httpd-foreground
~/apache$ chmod +x httpd-foreground
...service docker start
...service docker status
~/apache$ docker build -t httpdliuying .
```
Then you'll get this error: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?

### SSH
``` bash
 sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
 sudo vim /etc/ssh/sshd_config
 sudo service ssh start # excute it when you login Xftp 6
 * Starting OpenBSD Secure Shell server sshd                                                                                                     /etc/ssh/sshd_config: line 12: Bad configuration option: service
 /etc/ssh/sshd_config: line 13: Bad configuration option: service
 /etc/ssh/sshd_config: terminating, 2 bad configuration options

 [fail]
# sudo service ssh status
 * sshd is not running
 ```
 Then, you should uninstall it & reinstall it:
 ``` bash
# sudo apt-get purge openssh-server
# sudo apt-get install openssh-server
# sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
# sudo vim /etc/ssh/sshd_config
# sudo service ssh start
 * Starting OpenBSD Secure Shell server sshd                                                                                                     [ OK ]
# sudo service ssh status
 * sshd is running
# sudo systemctl enable ssh
Synchronizing state of ssh.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable ssh
 ...
```
