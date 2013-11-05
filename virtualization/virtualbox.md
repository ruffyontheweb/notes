### Make virtual disk more compressible

This zeros out the unused inodes to make the virtual disk more compressible.

```#mount -n -o remount,ro -t ext2 /dev/sda1 /```  
```#zerofree /dev/sda1```

```VBoxManage modifyhd "<VDI_FILE_HERE>" --compact```

### Convert physical installation to virtualbox
* From virtualbox.org: [Migrate Windows][0]

[0]: https://www.virtualbox.org/wiki/Migrate_Windows "Migrate Windows"
