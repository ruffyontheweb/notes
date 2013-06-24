## Compiling glusterfs from source

*This HowTo is for Debian-based*

```Source:``` Download from gluster [website](http://www.gluster.org/download/).

### Install required packages

```# apt-get install flex bison libssl-dev fuse fuse-utils libevent-dev libreadline6-dev libaio-dev libaio1 attr```

In case you don't have compiler and `make`, install it,

```# apt-get install gcc make```

### Compile the glusterfs source

```# ./configure```  
```# make -j4```  
```# make install```

### On server side
```# update-rc.d glusterd defaults```

### On client side
    *   Mount manually:  
```# mount -t glusterfs server:/GlusterVol /mnt/mountPoint```  

    *   Using fstab:  
```server:/GlusterVol /mnt/mountPoint glusterfs defaults 0 0```
