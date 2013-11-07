## Compiling glusterfs from source

*This HowTo is for Debian-based*

```Source:``` Download from gluster [website][0]

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

### Create a gluster volume
_Setting glusterfs server consists 3 steps: setting a pool of trusted peers,
creating the volume, start the volume._

1) Setting up a pool of trusted peers. On each server, we need to add other
servers to the trusted peers pool except itself. For example, on gluster0

```# gluster peer probe gluster1```  
```# gluster peer probe gluster2```

2) Creating the volume, for my need, I want distributed-replicate cluster.
On each server, I have ```/export/data_0, /export/data_1, /export/data_2```
Data will be distributed across ```data_{0-2}``` and replicated between
server gluster0 and gluster1; ```gluster1:/export/data_0``` is a replicate
of ```gluster0:/export/data_0``` for instance.

```# gluster volume create gStore replicate 2 transport tcp gluster0:/export/data_0 gluster1:/export/data_0 gluster0:/export/data_1 gluster1:/export/data_1 gluster0:/export/data_2 gluster1:/export/data_2```

3) Start volume

```# gluster volume start gStore```

### On client side
* Mount manually:  
```# mount -t glusterfs server:/GlusterVol /mnt/mountPoint```  

* Using fstab:  
```server:/GlusterVol /mnt/mountPoint glusterfs defaults 0 0```

### Upgrading to CentOS 6.4

If you upgrade the server from 6.3 to 6.4, the client will fail to mount.
Check out this [email][1] from Alan Orth. If you need `audit2allow` tool,
install it with
```# yum install policycoreutils-python```

### Ports on glusterfs

| Transport | Service    | Port      | Version |
|:---------:|:----------:|----------:|:-------:|
| TCP       | glusterd   | 24007     |     ANY |
| RDMA      | glusterd   | 24008     |     ANY |
| TCP/RMDA  | glusterfs  | 24009+    |  < 3.4  |
| TCP/RMDA  | glusterfs  | 49152+    |    3.4  |
| TCP       | NFS        | `3846[5-7]` |    ANY  |
| TCP       | NLM        | 38468     |    3.3  |
| TCP/UDP?  | portmap    | 111       |    ANY  |

From IRC's bot:
```glusterd's management port is 24007/tcp and 24008/tcp if you use rdma.
Bricks (glusterfsd) use 24009 & up for <3.4 and 49152 & up for 3.4.  
(Deleted volumes do not reset this counter.) Additionally it will listen on
38465-38467/tcp for nfs, also 38468 for NLM since 3.3.0. NFS also depends on rpcbind/portmap on port 111```

More references:  
* [Tuning glusterfs][2]  
* Joe Julian's [blog][3]

**TODO:**
* Refactor this document

[0]: http://www.gluster.org/download/ "gluster"
[1]: http://www.gluster.org/pipermail/gluster-users/2013-March/035731.html "CentOS 6.4 gluster"
[2]: http://www.jamescoyle.net/how-to/559-glusterfs-performance-tuning "gluster tuning"
[3]: http://joejulian.name/blog/category/glusterfs/ "Joe Julian's blog"
