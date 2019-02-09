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
# hashget -p /var/lib/lxc/delme/rootfs/
~~~
This makes two files, hashget-exclude and .hashget_restore.

#### hashget-exclude*
(in homedir of current user, often root)
exclude file lists files which can be recovered later (re-downloaded from the web resources, such as http://snapshot.debian.org)

#### .hashget_restore 
(in root of virtual machine filesystem). Has all info (URLs) needed to download proper files and restore files which were skipped by tarring -X hashget-exclude


## Advanced usage
[Verify debian packages](debverify)

## Development
[TODO](TODO)