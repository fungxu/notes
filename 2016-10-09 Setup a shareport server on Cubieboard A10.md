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
