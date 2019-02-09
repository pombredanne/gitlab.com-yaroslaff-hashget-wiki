# HashGet

## Basic usage

### Just fetching file by it's hash

This is rarely needed, but this is simplest core feature 
~~~
$ bin/hashget --get sha256:e30642d899811439c641210124c23444af5f01f5bc8b6f5248101944486122dd
./adduser.local.conf

$ sha256sum ./adduser.local.conf 
e30642d899811439c641210124c23444af5f01f5bc8b6f5248101944486122dd  ./adduser.local.conf
~~~

### Effective packing of Debian virtual machines filesystes

Note: before packing, you may need to *crawl* filesystem to have effective local hash database (HashDB)
~~~
# hashget -p /var/lib/lxc/mydebian/rootfs/
~~~
This makes two files, hashget-exclude and .hashget_restore.

**hashget-exclude**
exclude file lists files which can be recovered later (re-downloaded from the web resources, such as https://snapshot.debian.org/ or  https://kernel.org/)

File created in homedir of current user. Could be overriden with -X option. Same option should be given to `tar` command for packing.

**.hashget_restore** 
(in root of virtual machine filesystem). Has all info (URLs) needed to download proper files and restore files which were skipped by tarring -X hashget-exclude

~~~
tar -czf /tmp/rootfs.tar.gz -C /var/lib/lxc/mydebian/rootfs/ -X /root/hashget-exclude .
~~~




## Advanced usage
[Verify debian packages](debverify)

## Development
[TODO](TODO)