# Verification 

Upon verification, hashget-admin makes short HTTP HEAD request to URL of HashPackage. If HTTP status code is not 200 ("OK"), verification fails.

If reply has Content-Length header, it must be equal to HashPackage 'size' attribute. If not equal, verificaton fails. 

Otherwise (URL is valid, size not changed) verification is OK.

# Verification options
`hashget-admin --verify` will verify HashPackages in HashDB.

`-p PROJECT` will limit verification to only HashPackages in this project.

`--hp HPSPEC` will limit verification to only HashPackages which matches this [hpspec](specification).

if `--verify` command has optional value `delete` HashPackages which fails verification are deleted.

# Examples

```shell
# Verify all HP in project 'wp' 
$ hashget-admin --verify -p wp 

# Verify wordpress-5.1.1.zip and delete if it fails
$ hashget-admin --verify delete -p wp --hp name:wordpress-5.1.1.zip
``` 