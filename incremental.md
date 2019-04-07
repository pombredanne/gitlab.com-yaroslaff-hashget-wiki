### Incremental / Differential backups with hashget

Prepare data for test
```shell
$ mkdir /tmp/test
$ dd if=/dev/urandom of=/tmp/test/1M bs=1M count=1
1+0 records in
1+0 records out
1048576 bytes (1.0 MB, 1.0 MiB) copied, 0.0198294 s, 52.9 MB/s
```

Make first full backup (since all data is custom, disable hasherver to make it faster)
```shell

$ hashget -zf /tmp/full-1.tar.gz --pack /tmp/test --hashserver
STEP 1/3 Indexing...
Indexing done in 0.00s. 0 local + 0 pulled + 0 new = 0 total packages
STEP 2/3 prepare exclude list for packing...
saved: 0 files, 0 pkgs, size: 0. Download: 0
STEP 3/3 tarring...
/tmp/test (1.0M) packed into /tmp/full-1.tar.gz (1.0M)
```
1M packed into 1M.

Put into into http available resource and index
```shell
$ sudo cp /tmp/full-1.tar.gz /var/www/html/hg/
$ hashget --submit http://localhost/hg/full.tar.gz --project my_incremental --hashserver
```

Make any changes to data and pack again
```shell
$ date > /tmp/test/date

$ hashget -zf /tmp/diff.tar.gz --pack /tmp/test --hashserver
STEP 1/3 Indexing...
Indexing done in 0.00s. 0 local + 0 pulled + 0 new = 0 total packages
STEP 2/3 prepare exclude list for packing...
saved: 1 files, 1 pkgs, size: 1.0M. Download: 1.0M
STEP 3/3 tarring...
/tmp/test (1.0M) packed into /tmp/diff.tar.gz (482.0)
```
Incremental (delta) backup is very short. But will require full backup available on same URL for unpacking

To make new full backup delete old from index:
```shell
$ hashget-admin --purge --hp full-1.tar.gz
```

Now make new full backup:
```shell
$ hashget -zf /tmp/full-2.tar.gz --pack /tmp/test --hashserver
STEP 1/3 Indexing...
Indexing done in 0.00s. 0 local + 0 pulled + 0 new = 0 total packages
STEP 2/3 prepare exclude list for packing...
saved: 0 files, 0 pkgs, size: 0. Download: 0
STEP 3/3 tarring...
/tmp/test (1.0M) packed into /tmp/full-2.tar.gz (1.0M)
```

Backups will be differential if you will index only full backups, or incremental if you will index also delta backups.

Obviously, full backup name/url could be different, e.g. full-01012019.tar.gz 

When made new full backup, to avoid creating new delta backups based on old full backup, [delete old package](https://gitlab.com/yaroslaff/hashget/wikis/hashget-admin#delete-hashpackages) from HashDB.
