Default HashDB server URL is https://hashdb.okerr.com/hashdb/ (can be overriden with --hashserver option)

# HashDB
```
a/
sig/
motd.txt
config.json
```

motd.txt is plain text displayed by client when HashServer initialized.
config.json is configuration of this hashserver

## anchors
Anchor file is large file (100k+ ) in package. Anchor is SHA256 hashsum of such file.
Anchors are used to discover which packages are installed. 

anchors are stored on a/ subdir under hashdb root (e.g. `https://hashdb.okerr.com/hashdb/a/` ) in 'hash3' format ( first 3 bytes in hex are used as subdirectories
e.g. https://hashdb.okerr.com/hashdb/a/0b/05/f8/0fcbb8737ad66abab87f2d2ac8a2c70466c184d005531f3fd70c9cfb2c links to HashPackage which has `sha256:0b05f80fcbb8737ad66abab87f2d2ac8a2c70466c184d005531f3fd70c9cfb2c` in anchors section).

## signatures
Signatures can be of different type (sigtype), currently only 'deb' sigtype is used. Signatures are used to quickly check if specific package (with known signature) is exists on hashserver.

Example:
https://hashdb.okerr.com/hashdb/sig/deb/v/vim/vim_8.0.0197-4+deb9u1_amd64 is symlinked to HashPackage with these sigtuatures:
```
    "signatures": {
        "url": "http://snapshot.debian.org/archive/debian/20171002T041924Z/pool/main/v/vim/vim_8.0.0197-4+deb9u1_amd64.deb",
        "deb": "vim_8.0.0197-4+deb9u1_amd64"
    },
```

Filesystem hierarchy can be different for different sigtypes. For 'deb' sigtype, it's using debian-style hierarchy. e.g. 
`sig/deb/v/vim/vim_8.0.0197-4+deb9u1_amd64` (for deb sig: vim_8.0.0197-4+deb9u1_amd64 - usual debian package) or `sig/deb/liba/libapache2-mod-php7.0/libapache2-mod-php7.0_7.0.33-0+deb9u3_amd64` (for libapache2-mod-php7.0_7.0.33-0+deb9u3_amd64 - library package). In other word, prefix is first letter of siguature or first 4 letters of signature (if starting with lib*).

# Package Pool
Package pool can be used to retrive packages over HTTP, it can be on separate URL or can share same hashdb root, pool/ directory. It uses same hash3 format as anchors. Each file is either package itself or symlink to package, so wget to proper hash should either return file or redirect or 404 error.

