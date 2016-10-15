Setup a shareport (airplay) server on linux ubuntu arm server
===

Fung 2016.10

# 1. Install build tools




apt-get install build-essential git – these may already be installed.
apt-get install autoconf automake libtool libdaemon-dev libasound2-dev libpopt-dev libconfig-dev
apt-get install avahi-daemon libavahi-client-dev  - if you want to use Avahi (recommended).
apt-get install libssl-dev - if you want to use OpenSSL and libcrypto, or use PolarSSL otherwise.
apt-get install libpolarssl-dev - if you want to use PolarSSL, or use OpenSSL/libcrypto otherwise.
apt-get install libsoxr-dev - if you want support for libsoxr-based resampling. 

This library is in many recent distributions, including Jessie and Raspioan Jessie; if not, instructions for how to build it from source for Raspian/Debian Wheezy are available at LIBSOXR.md.

all in one
```
apt-get install build-essential git  autoconf automake libtool libdaemon-dev libasound2-dev libpopt-dev libconfig-dev libavahi-client-dev libssl-dev  libsoxr-dev  -y 
```

# 2. Download Shairport Sync:

```
$ git clone https://github.com/mikebrady/shairport-sync.git
```

# 3. Create configuration file. Next, cd into the shairport-sync directory and execute the following command:

```
$ autoreconf -i -f
```

# 4. config the libary


```
$ ./configure --sysconfdir=/etc --with-alsa --with-avahi --with-ssl=openssl --with-metadata --with-soxr --with-systemv
```


# 5. Make

```

make && make install 

```

# 6. Startup Avahi-daemon

```
Avahi

Avahi 是Zeroconf规范的开源实现，常见使用在Linux上。包含了一整套多播DNS(multicastDNS)/DNS-SD网络服务的实现。它使用的发布授权是LGPL。Zeroconf规范的另一个实现是Apple公司的Bonjour程式。Avahi和Bonjour相互兼容(废话，都走同一个规范标准嘛，就象IE，Firefox，chrome都能跑HTTP1.1一样)。

Avahi允许程序在不需要进行手动网络配置的情况下，在一个本地网络中发布和获知各种服务和主机。例如，当某用户把他的计算机接入到某个局域网时，如果他的机器运行有Avahi服务，则Avahi程式自动广播，从而发现网络中可用的打印机、共享文件和可相互聊天的其他用户。这有点象他正在接收局域网中的各种网络广告一样。


Zeroconf
Zero configuration networking(zeroconf)零配置网络服务规范，是一种用于自动生成可用IP地址的网络技术，不需要额外的手动配置和专属的配置服务器。

“零配置网络服务”的目标，是让非专业用户也能便捷的连接各种网络设备，例如计算机，打印机等。整个搭建网络的过程都是通过程式自动化实现。如果没有 zeroconf，用户必须手动配置一些服务，例如DHCP、DNS，计算机网络的其他设置等。这些对非技术用户和新用户们来说是很难的事情。

Zeroconf规范的提出者是Apple公司.

```

we got problems.

```
root@cubieboard:/etc# avahi-daemon --debug 
Found user 'avahi' (UID 106) and group 'avahi' (GID 112).
Successfully dropped root privileges.
avahi-daemon 0.6.31 starting up.
Successfully called chroot().
Successfully dropped remaining capabilities.
No service file found in /etc/avahi/services.
socket() failed: Permission denied
socket() failed: Permission denied
Failed to create server: No suitable network protocol available
avahi-daemon 0.6.31 exiting.

```

Permission denied. we don't want chroot, so test it with

```
root@cubieboard:/etc# avahi-daemon --debug --no-drop-root
avahi-daemon 0.6.31 starting up.
No service file found in /etc/avahi/services.
SO_REUSEPORT failed: Protocol not available
SO_REUSEPORT failed: Protocol not available
Joining mDNS multicast group on interface eth0.IPv6 with address fe80::8d:4ff:fe80:ad0e.
New relevant interface eth0.IPv6 for mDNS.
Joining mDNS multicast group on interface eth0.IPv4 with address 192.168.1.100.
New relevant interface eth0.IPv4 for mDNS.
Network interface enumeration completed.
Registering new address record for fe80::8d:4ff:fe80:ad0e on eth0.*.
Registering new address record for 192.168.1.100 on eth0.IPv4.
Registering HINFO record with values 'ARMV7L'/'LINUX'.
Server startup complete. Host name is cubieboard.local. Local service cookie is 3284905152.
^CGot SIGINT, quitting.
Leaving mDNS multicast group on interface eth0.IPv6 with address fe80::8d:4ff:fe80:ad0e.
Leaving mDNS multicast group on interface eth0.IPv4 with address 192.168.1.100.
avahi-daemon 0.6.31 exiting.

```

Hack `/etc/init.d/avahi-daemon` , add `--no-drop-root` next to `$DAEMON -D` 

```
d_start() {
    $DAEMON -c && return 0

    if [ -e $DISABLE_TAG -a "$AVAHI_DAEMON_DETECT_LOCAL" != "0" ]; then
        # Disabled because of the existance of an unicast .local domain
        log_warning_msg "avahi-daemon disabled because there is a unicast .local domain"
        exit 0;
    fi;

    $DAEMON -D --no-drop-root
}

```

7. get the devices list

```
apt-get install alsa-utils
```

```
root@cubieboard:/etc# aplay -l
**** List of PLAYBACK Hardware Devices ****
card 0: sunxicodec [sunxi-CODEC], device 0: M1 PCM [sunxi PCM]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 1: DAC [USB Audio DAC], device 0: USB Audio [USB Audio]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
```


card 0  is the phonejack and card 1 is our usb sound card

The file `/etc/shairport-sync.conf` is here :

```
general = {
  name = "Kitchen";
  interpolation = "soxr";
};

alsa = {
  output_device = "hw:1";
};
```

8. setup the init

```
update-rc.d shairport-sync defaults 90 10
```
