# HashPackage expiration

HashPackage have optional 'expires' attribute. (Default: it's empty, package never expires).
If HashPackage expired (current time is after it's expiration date), it's not loaded into HashDB when hashget runs and not used for deduplication.

For example, this makes possible to use following backup scheme:
1. Each 1st January we make full backup January.tar.gz (with expies - 1st February) and store it
2. Each other day, we make short incremental backup
3. At 1st February, we make new full backup February.tar.gz
5. Even if some files exists in both full backups, all new incremental backups will be made against February backup (because January backup is expired).
6. January backup could be safely deleted at this moment (but could be still stored in backup).

Expiration is in format YYYY-MM-DD, e.g. '2018-12-31'.

## Indexing packages with expiration
To index hashpackage with expiration attribute use --submit with --expires:
```shell
hashget --submit https://wordpress.org/wordpress-5.1.1.zip -p my --expires 2019-06-01
```

To see expiration date for all or specific HashPackage(s):
```shell
$ hashget-admin --hp https://wordpress.org/wordpress-5.1.1.zip --get expires
2019-06-01 00:00:00

$ hashget-admin --get url expires
http://snapshot.debian.org/archive/debian/20190122T215529Z/pool/main/a/apt/libapt-pkg5.0_1.4.9_amd64.deb None
http://snapshot.debian.org/archive/debian/20161107T153538Z/pool/main/s/sgml-base/sgml-base_1.29_all.deb None
https://wordpress.org/wordpress-5.1.1.zip 2019-06-01 00:00:00
...
```

# Use expiring HashPackages
To pack packages based on expirations (in this example, archive will be deduplicated because HashPackage expires after expiration date for archive):
```shell
$ hashget --pack /tmp/wordpress -zf /tmp/wordpress.tar.gz --expires 2019-05-31
...
/tmp/wordpress (38.1M) packed into /tmp/wordpress.tar.gz (154.5K)
```

In examples below HashPackage will not be used.
```shell
# Archive will be expired AFTER hashpackage (full backup), so hashpackage should not be used
$ hashget --pack /tmp/wordpress -zf /tmp/wordpress.tar.gz --expires 2019-06-02
...
/tmp/wordpress (38.1M) packed into /tmp/wordpress.tar.gz (10.1M)

# Archive will never expires, so expiring hashpackages cannot be used 
$ hashget --pack /tmp/wordpress -zf /tmp/wordpress.tar.gz                     
...
/tmp/wordpress (38.1M) packed into /tmp/wordpress.tar.gz (10.1M)

```

# Deleting expires HashPackages
Expiration can be used as --hp filter in hashget-admin.

```shell
# Get HashPackages expired at specific date or today
$ hashget-admin --hp expired:2019-12-31 --list
wordpress-5.1.1.zip (68/1366)
$ hashget-admin --hp expired:2019-12-31 --get url
https://wordpress.org/wordpress-5.1.1.zip

# Set expiration in past
$ hashget-admin --hp wordpress-5.1.1.zip --set expires 2019-01-01
$ hashget-admin --hp expired --list
wordpress-5.1.1.zip (68/1366)

# Delete all expired packages
$ hashget-admin --hp expired --purge
Purge wordpress-5.1.1.zip (68/1366)
Purged 1 packages
```
