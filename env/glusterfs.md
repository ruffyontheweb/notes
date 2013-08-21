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

----------oOo------------

### Upgrading to CentOS 6.4

If you upgrade the server from 6.3 to 6.4, the client will fail to mount.
Check out this [email][0] from Alan Orth. If you need `audit2allow` tool,
install it with
```
# yum install policycoreutils-python
```

[0]: http://www.gluster.org/pipermail/gluster-users/2013-March/035731.html "CentOS 6.4 gluster"
