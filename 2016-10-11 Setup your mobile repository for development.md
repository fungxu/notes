Set up your local respository server
===
Fung.Xu 2016.10.11


## Hardware

FriendlyARM NanoPi Neo, with a 1T USB Harddriver.

## Server List

1. Maven Repo
2. Docker Registry
3. Ubuntu mirror / backports
4. npm registry
5. Bower Javascript lib
6. Gogs - git respository

### 1 Maven with Nexus



### 3 ubuntu mirror (pc site on ARM server) 

### 3.1 setup `/etc/apt/sources.list`

```
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ wily main universe multiverse restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ wily-security main universe multiverse restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ wily-updates main universe multiverse restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ wily-backports main universe multiverse restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ wily-proposed main universe multiverse restricted
```

### 3.2 install `apt-mirror`

```
apt-get update && apt-get install -y apt-mirror
```

### 3.3 setup `/etc/apt/mirror.list`

```
set base_path /media/ubuntu
set nthreads 20
set _tilde 0
set defaultarch amd64
deb http://cn.archive.ubuntu.com/ubuntu wily main restricted universe multiverse
deb http://cn.archive.ubuntu.com/ubuntu wily-security main restricted universe multiverse
deb http://cn.archive.ubuntu.com/ubuntu wily-updates main restricted universe multiverse
deb http://cn.archive.ubuntu.com/ubuntu wily-backports main restricted universe multiverse

deb http://cn.archive.ubuntu.com/ubuntu xenial main restricted universe multiverse
deb http://cn.archive.ubuntu.com/ubuntu xenial-security main restricted universe multiverse
deb http://cn.archive.ubuntu.com/ubuntu xenial-updates main restricted universe multiverse
deb http://cn.archive.ubuntu.com/ubuntu xenial-backports main restricted universe multiverse

clean http://cn.archive.ubuntu.com/ubuntu
```





