```
:%s/^deb http:\/\/\([^ ]\+ \)/deb http:\/\/local_ip:3142\/\1/g
```

**Note**: replace `local_ip` with your local yokal `apt-cacher-ng` server ip address
or some nice name.