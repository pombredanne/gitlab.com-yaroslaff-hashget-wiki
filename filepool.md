FilePools are used to save bandwidth, speed-up restore and work even if source servers are unavailable.

Pool is specified with `--pool` option, for example:
~~~
$ hashget -u . --pool /tmp/pool
$ hashget -u . --pool http://myhashdb.example.com/
~~~
