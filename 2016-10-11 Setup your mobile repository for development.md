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

## 1 Maven with Nexus



## 3 ubuntu mirror (pc site on ARM server) 

### 3.1 setup `/etc/apt/sources.list`

```
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ wily main universe multiverse restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ wily-security main universe multiverse restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ wily-updates main universe multiverse restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ wily-backports main universe multiverse restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ wily-proposed main universe multiverse restricted
```

on cubieboard A10

```
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ trusty main universe multiverse restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ trusty-security main universe multiverse restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ trusty-updates main universe multiverse restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ trusty-backports main universe multiverse restricted
deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ trusty-proposed main universe multiverse restricted

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

## 4 NPM local Repository  - sinopia

Why we need a local repository?


> Use private packages.

If you want to use all benefits of npm package system in your company without sending all code to the public, and use your private packages just as easy as public ones.

> Cache npmjs.org registry.

If you have more than one server you want to install packages on, you might want to use this to decrease latency (presumably "slow" npmjs.org will be connected to only once per package/version) and provide limited failover (if npmjs.org is down, we might still find something useful in the cache).

> Override public packages.

If you want to use a modified version of some 3rd-party package (for example, you found a bug, but maintainer didn't accept pull request yet), you can publish your version locally under the same name.

### install ,

```
npm install -g verdaccio --registry=https://registry.npm.taobao.org --verbose
```

### start up , 

```
$ verdaccio
 warn  --- config file  - /Users/fung/.config/verdaccio/config.yaml
 warn  --- http address - http://localhost:4873/
```

### when it's request by others

```
 http  --> 200, req: 'GET https://registry.npmjs.org/@angular%2Fplatform-browser-dynamic', bytes: 0/32396
 http  <-- 200, user: undefined, req: 'GET /@angular%2fplatform-browser-dynamic', bytes: 0/3190
 http  --> 200, req: 'GET https://registry.npmjs.org/@angular%2Fforms', bytes: 0/17605
 http  <-- 200, user: undefined, req: 'GET /@angular%2fforms', bytes: 0/2069
 http  --> 200, req: 'GET https://registry.npmjs.org/@angular%2Fhttp', bytes: 0/27087
 ```

