Hashget work (little simplified) mainly separated into 3 stages:

## Indexing
Hashget examines file available on URL, and knows which files it may extract from that URL. Then it stores URL of that file and hashes of containing files into local hash database (HashDB).

## Deduplication
When prepare target directory for packing, hashget calculates checksum of every file, checks it against known hashes from HashDB and prepares 'exclude list' of files which you can download from network. Other files are compressed with tar and gzip along with .hashget-restore file, which lists all files to recovery from network.

## Recovery
On recovery (post-unpacking), hashget processes .hashget-restore file, downloading files from network, verifies and storing it on target directory. 

Hashget archives has following advantages:

- **small** - sometimes it's 40-50 times smaller then .tar.gz without deduplication. 
- **standard** - it's packed into usual .tar.gz with just one additional file .hashget-restore in JSON format with simple structure.
- **automatical** - recovery process is automated. It will take little more time and bandwidth, but you will get exactly same unpacked result as you'd get with usual .tar.gz


