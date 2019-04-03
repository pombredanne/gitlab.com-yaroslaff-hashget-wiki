File .hashget-restore.json is added to root of each hashget compressed archive and contains information to restore archive.

Example:
~~~
{
    "files": [
        {
            "atime": 1554251915,
            "ctime": 1554251911,
            "file": "linux-3.18.137.tar.xz",
            "gid": 0,
            "mode": 420,
            "mtime": 1553325711,
            "sha256": "ed4e08c8cbeeda7191db00474f9c0515bf37bf872743dbfed8aaa574cfe2b8e2",
            "size": 81515808,
            "uid": 0
        },
        ...
    ],
    "packages": [
        {
            "hash": "sha256:ed4e08c8cbeeda7191db00474f9c0515bf37bf872743dbfed8aaa574cfe2b8e2",
            "url": "https://cdn.kernel.org/pub/linux/kernel/v3.x/linux-3.18.137.tar.xz"
        }
    ],
    "packagesize": 81515808,
    "packagesize_exact": true
}
~~~

**packages** - is list of hash and URL to download each package files. 

**files** - is list of files to restore. Hashget processes each downloaded package file, and for each file found in package looks if it's listed in files section and restores it with same attributes.

**packagesize** is summary size of each package in bytes to estimate download traffic.

**packagesize_exact** is boolean (almost always will be 'true') indicating that packagesize is accurate (sum of sizes for all packages). If size of package is missing from at least one hashpacakge (when packing), packagesize_exact will be false, and packagesize will have lower-range estimation.
