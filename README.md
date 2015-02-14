docker-busybox
==============

This is a minmal busybox image that has been baked specifically for running Open vSwitch
It contains:

- Busybox
- OpenSSL
- Python 2.7
- Kmod (`lsmod`, `insmod`, `modprobe`)

## Using this image

In a Dockerfile, you can write

    FROM socketplane/busybox

Or you can open a shell

    docker run -it socketplane/busybox sh

## Building this image

The build process is borrowed from [jpetazzo/busybox](https://github.com/jpetazzo/docker-busybox)

    ./mkrootfs tarmaker-buildroot
    # Wait an hour
    docker build -t socketplane/busybox .
 
