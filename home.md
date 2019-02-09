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

or verbosely:

~~~
$ bin/hashget -v --get sha256:e30642d899811439c641210124c23444af5f01f5bc8b6f5248101944486122dd
downloading http://snapshot.debian.org/archive/debian/20160627T043218Z/pool/main/a/adduser/adduser_3.115_all.deb to /home/xenon/.CacheGet/files/http/snapshot.debian.org/archive/debian/20160627T043218Z/pool/main/a/adduser/adduser_3.115_all.deb
Starting new HTTP connection (1): snapshot.debian.org
http://snapshot.debian.org:80 "GET /archive/debian/20160627T043218Z/pool/main/a/adduser/adduser_3.115_all.deb HTTP/1.1" 200 241436
download 235.8K status code: 200
look for sha256:e30642d899811439c641210124c23444af5f01f5bc8b6f5248101944486122dd
clean dir /tmp/hashget-uniunpack-z3kvv1w9
./adduser.local.conf
~~~

## Advanced usage
[Verify debian packages](debverify)

## Development
[TODO](TODO)