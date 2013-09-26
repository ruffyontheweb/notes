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

### Ports on glusterfs

| Transport | Service    | Port      | Version |
|:---------:|:----------:|----------:|:-------:|
| TCP       | glusterd   | 24007     |     ANY |
| RDMA      | glusterd   | 24008     |     ANY |
| TCP/RMDA  | glusterfs  | 24009+    |  < 3.4  |
| TCP/RMDA  | glusterfs  | 49152+    |    3.4  |
| TCP       | NFS        | 3846[5-7] |    ANY  |
| TCP       | NLM        | 38468     |    3.3  |
| TCP/UDP?  | portmap    | 111       |    ANY  |

From IRC's bot:
```
glusterd's management port is 24007/tcp and 24008/tcp if you use rdma.
Bricks (glusterfsd) use 24009 & up for <3.4 and 49152 & up for 3.4.
(Deleted volumes do not reset this counter.) Additionally it will listen on
38465-38467/tcp for nfs, also 38468 for NLM since 3.3.0. NFS also depends on rpcbind/portmap on port 111
```

[Tuning glusterfs][1]

**TODO:**
* Refactor this document

[0]: http://www.gluster.org/pipermail/gluster-users/2013-March/035731.html "CentOS 6.4 gluster"
[1]: http://www.jamescoyle.net/how-to/559-glusterfs-performance-tuning "gluster tuning"
