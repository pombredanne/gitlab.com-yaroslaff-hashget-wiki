For testing purpose, we recommend to make test machine this way:
```
host# lxc-create -n mydebvm -t download -- --dist=debian --release=stretch --arch=amd64
host# lxc-start -n mydebvm
host# lxc-attach -n mydebvm
mydebvm# apt install -y wget apache2 mysql-server vim
```

After install, disk usage for rootfs:
```
host:/var/lib/lxc/mydebvm/rootfs# du -sh
725M	.
```

Because LXC root images and Debian packages are updated with time, later results could be little different, but not significantly.