## Stages
Hashget work (little simplified) mainly separated into 3 stages:

- **Indexing**. Hashget examines file available on URL, and knows which files it may extract from that URL. Then it stores URL of that file and hashes of containing files into local hash database (HashDB).
- **Deduplication**. When prepare target directory for packing, hashget calculates checksum of every file, checks it against known hashes from HashDB and prepares 'exclude list' of files which you can download from network. Other files are compressed with tar and gzip along with .hashget-restore file, which lists all files to recovery from network.
- **Recovery**. On recovery (post-unpacking), hashget processes .hashget-restore file, downloading files from network, verifies and storing it on target directory. 

## Advantages

Hashget archives has following advantages

- **small** - sometimes it's 40-50 times smaller then .tar.gz without deduplication. 
- **standard** - it's packed into usual .tar.gz with just one additional file .hashget-restore in JSON format with simple structure.
- **automatical** - recovery process is automated. It will take little more time and bandwidth, but you will get exactly same unpacked result as you'd get with usual .tar.gz
- **self-sufficient** - each small hashget archive is fully complete 'in same world' (if Internet exists). This makes it very different from incremental/differential backups which requires large full backup and which becames nearly useless if full backup is damaged or lost. 

## Changes backup policy
Hashget archive makes backups smaller and much cheaper. With these changes now you can:
1. Backup more often. Not monthly but every night.
2. Complete backup. Do not backup just /home and /etc, backup whole VM (with all /opt packages, /usr/local/bin scripts, cron jobs etc. If will not forget anything if you will backup all).

## Risks
Hashget archives is self-sufficient in 'same world', but will not be if world will change a lot (e.g. Debian snapshot project will close).

Hashget project may disappear so you don't have hashget utility to unpack. (You can clone this repo to protect yourself against this.)

Source project (such as Debian) may terminate or crash, so you cannot download package. But:

- Debian is big project with huge IT resources. They spend much more money and efforts on their reliability then most of small companies, so, most likely they are much more reliable then average hashget user backup server.
- In that post-apocalyptic world without main Internet projects, do you really think you will need to restore something from backup in office? (Personally, I plan to drive armored semi-truck with tanker of fuel, shooting to bikers or zombies)
- Even in this situation, you know which files are missing, their hashes, so you can google it.








