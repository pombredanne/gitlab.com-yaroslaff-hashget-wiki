# Anchor
Large (over 100K) file from *package*. Anchors are used to fetch *HashPackages* from *HashServer*.

# DirHashDB
Local *HashDB* stored in filesystem directory.

# HashDB 
Collection of *HashPackages*, local (*DirHashDB*) or remote (*HashServer*)

# HashPackage
Small local representation of *package*, which has URL of remote package and list of file hashes which could be extracted from that package

# HashServer
remote *HashDB* with HTTP interface

# Package
Remote file (most often - archive) which can contain one or more file. For example, linux kernel archive, wordpress archive and vim .deb package are all Packages. 

# Signature and Signature Type
Unique combination which identifies package. For example, for vim package:
```json
    "signatures": {
        "url": "http://snapshot.debian.org/archive/debian/20171002T041924Z/pool/main/v/vim/vim-common_8.0.0197-4+deb9u1_all.deb",
        "deb": "vim-common_8.0.0197-4+deb9u1_all"
    },
```


