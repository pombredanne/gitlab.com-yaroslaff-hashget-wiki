FilePools are used to save bandwidth, speed-up restore and work even if source servers are unavailable.

Pool is specified with `--pool` option, for example:
~~~
$ hashget --info . --pool /tmp/pool/ 
$ hashget --info . list --pool /tmp/pool/
$ hashget -u . --pool /tmp/pool
$ hashget -u . --pool http://myhashdb.example.com/
~~~

# Populating pool

## Manual
Just drop packages to pool directory: `cp *.tar.gz *.deb /tmp/pool` or in any subdirectory inside pool, e.g. `/tmp/pool/project1` or `/tmp/pool/user1`

## Automatical
Package is added to pool directory when it's downloaded if `--pool` options given.
~~~
$ mkdir /tmp/pool
$ hashget -u . --pool /tmp/pool
...
Recovered 8534/8534 files 450.0M bytes (0 downloaded, 0 from pool, 98.7M cached) in 155.92s

# Pool populated now
$ ls /tmp/pool
adduser_3.115_all.deb                                  liblz4-1_0.0~r131-2+b1_amd64.deb
apache2-bin_2.4.25-3+deb9u6_amd64.deb                  liblzma5_5.2.2-1.2+b1_amd64.deb
...

# Now packages are taken from pool
$ hashget -u . --pool /tmp/pool
...
Recovered 8534/8534 files 450.0M bytes (0 downloaded, 98.7M from pool, 0 cached) in 146.92s
~~~
`--submit`, `--index` and `--postunpack` (`-u`) are adding downloaded packages to pool (if --pool given). `--postunpack` (`-u`) is reading from pool.

# Make HTTP pool
~~~
$ hashget-admin --build /var/www/html/hashdb/ --pool /tmp/pool
build pool /tmp/pool in webroot /var/www/html/hashdb/
/var/www/html/hashdb/pool/sha256/9f/3f/4a/6071ccf0a66876b05be617b8c4efbc85344c0e6b28da393196110e6826 > /tmp/pool/libpsl5_0.17.0-3_amd64.deb
/var/www/html/hashdb/pool/sha256/96/51/52/0ea7d04c0034502c29f365a23aad11409c365ac2fba4ddee7d3789438d > /tmp/pool/libaio1_0.3.110-3_amd64.deb
/var/www/html/hashdb/pool/sha256/4e/69/97/224779a11ec08bc395357818e50621eec349892ec8ff6efd8830b9e850 > /tmp/pool/perl-modules-5.24_5.24.1-3+deb9u5_all.deb
~~~

Now can use this pool:
~~~
$ hashget -u . --pool http://localhost/hashdb
..
Recovered 8534/8534 files 450.0M bytes (0 downloaded, 98.7M from pool, 0 cached) in 143.54s
~~~
