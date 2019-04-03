# HashPackage format

HashPackage (index file) is small file which represents information about package file (e.g. about Debian package or Linux kernel sources or WordPress).

Example (apache2.2-common_2.2.22-13+deb7u12_i386.deb.json):
~~~~
{
    "url": "http://snapshot.debian.org/archive/debian-security/20170921T204324Z/pool/updates/main/a/apache2/apache2.2-common_2.2.22-13+deb7u12_i386.deb",
    "files": [
        "sha256:f55e3c821dd7ae9e411922d60dbb06b8b3daf21e3122b90a86d89cb8410b03ab",
        "sha256:a9419086fc2b70f69130c3ee9f8761b0a12b0c7da47d3b779b04ab3827081cf0",
...
    ],
    "anchors": [
        "sha256:3d1fc1529b4438c19d9cf51e823755e556425fbc8c2e771e7e08b916c4939ae7",
        "sha256:01c692a30734d715517592f269de4bf554036b82f294e6d053fb5a0f9e3dd75b"
    ],
    "signatures": {
        "url": "http://snapshot.debian.org/archive/debian-security/20170921T204324Z/pool/updates/main/a/apache2/apache2.2-common_2.2.22-13+deb7u12_i386.deb",
        "deb": "apache2.2-common_2.2.22-13+deb7u12_i386"
    },
    "hashes": [
        "sha256:f864b37d1b7ca2ffefaa8323fd3d5f7e68c05a4210d945a45f6cb0bee1177d9b",
        "md5:bc7b94c1ce91947c33185f2a7920a17f"
    ],
    "attrs": {
        "sum_size": 1022458,
        "crawled_date": "2019-03-10 10:11:23",
        "size": 293526,
        "indexed_size": 943327
    }
}
~~~~

- **url** - static URL to download original package
- **signatures** - list of signatures specific for this package. No other package could have same signature of same type.
- **hashes** - hash specifications for package file itself
- **files** - hashsum of files (with size larger then minimal, 10K) which could be extracted from package file.
- **anchors** - same as files but only for anchors (large files, 100Kb+). Anchors could be used to detect packages.
- **attrs**:
  - **size** - size of package file 
  - **indexed_size** summary size for all indexed files (listed in files)
  - **sum_size** summary size for all files (both indexed and not indexed)
  - **crawled_date** - date when HashPackage was generated 
