## vmware

Things dealing with vmware products

### Shrinking virtual disk in ESXi

Recap from Boerlowie's [blog][0]:

In your EXSi shell, in the directory that contains your virtual machine:  
**Virtual size:**  
```ls –lh *.vmdk```  
**Real provisioned size:**  
```du –h *.vmdk```  
**Shrink them:**  
```vmkfstools --punchzero VirtualDisk.vmdk```

Replace `VirtualDisk` with your VirtualDisk file name.

[0]: http://boerlowie.wordpress.com/2012/09/06/how-to-shrink-a-thin-vmdk-on-esxi-5-0/ "shrinking virtual disk"
