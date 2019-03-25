Hashget work (little simplified) mainly separated into 3 stages:

## Indexing
Hashget examines file available on URL, and knows which files it may extract from that files. Then it stores URL of that file and hashes of containing files into local hash database (HashDB).

## Deduplication
When prepare target directory for packing, hashget calculates checksum of every file, checks it against known hashes from HashDB and prepares 'exclude list' of file which you can download from network. Other files are compressed with tar and gzip along with .hashget-restore file, which lists all files to recovery from network.

## Recovery
On recovery (post-unpacking), hashget processes .hashget-restore file, downloading files from network and storing it on target directory.



