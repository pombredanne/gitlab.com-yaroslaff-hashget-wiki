# HashPackage specification (hpspec) format
hpspec used in option `--hp HPSPEC` to apply action (e.g. `--list` or `--verify`) only to specific HashPackage(s).

**url**: either raw URL or with url: prefix. Examples (equal): 
`--hp https://wordpress.org/wordpress-5.1.1.zip` or 
`--hp url:https://wordpress.org/wordpress-5.1.1.zip`

**name**: basename of remote file. Examples (equal): 
`--hp wordpress-5.1.1.zip` or 
`--hp name:wordpress-5.1.1.zip`

**sig**: signature of HashPackage, Example: `--hp sig:deb:apache2-bin_2.4.25-3+deb9u6_amd64`

**all**: or all:all matches all HashPackages. 

All these options will match same package:
```shell
--hp url:https://wordpress.org/wordpress-5.1.1.zip
--hp https://wordpress.org/wordpress-5.1.1.zip
--hp name:wordpress-5.1.1.zip
--hp wordpress-5.1.1.zip
```