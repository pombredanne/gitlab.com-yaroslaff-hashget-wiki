Hashget tries to be very effective and not making load to backend projects (such as snapshot.debian.org) except when it is really needed.

1. Original packages files (such as stored on snapshot.debian.org) is not needed for compression. Hashget uses index files (HashPackages) for this. 
2. Index files are stored locally and on hashget HashServer. When new package is indexed, package is downloaded (just once!) then it's uploaded to hashserver and new index file is published for everyone on hashserver. *In most cases there are no (zero) requests to snapshot.debian.org.*
3. Original packages are needed for recovery, but this happens not often.
4. HashGet uses local cache for HTTP downloaded files. So even if someone experiments with hashget and deletes local HashDB AND file is not found on HashServer, it will be used from local http cache.

