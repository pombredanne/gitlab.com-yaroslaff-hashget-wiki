Directory HashDB stores HashPackages (indexes) on disk (usually in /var/cache/hashget/hashdb/<PROJECT>/ directory).

.options.json is file which specifies parameters. Example:
~~~~
{
    "storage": "basename",
    "pkgtype": "debsnap"
}
~~~~

Storage is either:
- **basename**: each HashPackage is stored in file based on basename in URL, e.g. file http://snapshot.debian.org/archive/debian-security/20170921T204324Z/pool/updates/main/a/apache2/apache2.2-common_2.2.22-13+deb7u12_i386.deb will be stored in /var/cache/hashget/hashdb/debsnap/p/apache2.2-common_2.2.22-13+deb7u12_i386.deb.json
- **hash2**: HashPackage path is based on it's SHA256 hashsum with 2 prefix bytes, e.g. HashPackage for package with file sha256sum aa09aac60c2ecac78548cee3e6f946426d523edf3269edcd2fb95c2e9219c884 will be stored in file ./aa/09/aac60c2ecac78548cee3e6f946426d523edf3269edcd2fb95c2e9219c884 in packages directory for this project
- **hash3**: same as hash2 but with 3-bytes prefix
