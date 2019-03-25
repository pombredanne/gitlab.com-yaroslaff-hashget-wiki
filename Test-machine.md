For testing purpose, we recommend to make test machine this way:

```
host# lxc-create -n mydebvm -t download -- --dist=debian --release=stretch --arch=amd64
host# lxc-start -n mydebvm
host# lxc-attach -n mydebvm
mydebvm# apt install wget apache2 mysql-server vim
```

After install, disk space for rootfs:
```
host:/var/lib/lxc/mydebvm/rootfs# du -sh
725M	.
```