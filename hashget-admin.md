In examples, will sometimes use project 'wp' and hashpackage

To reproduce same testing configuration:
```
$ hashget --submit https://wordpress.org/wordpress-5.1.1.zip -p wp
```

# Getting information
```shell
# Show status for local project(s)
$ hashget-admin --status 
$ hashget-admin --status -p wp

# List packages in project(s)
$ hashget-admin --list 
$ hashget-admin --list -p wp

# Dump HashPackages
$ hashget-admin --dump --hp wordpress-5.1.1.zip
```

# Verify
```shell
# verify all packages (not recommended)
$ hashget-admin --verify

# verify all packages in project wp 
$ hashget-admin --verify -p wp

# verify matching package(s). all commands below will verify same package: 
$ hashget-admin --verify --hp url:https://wordpress.org/wordpress-5.1.1.zip
$ hashget-admin --verify --hp https://wordpress.org/wordpress-5.1.1.zip
$ hashget-admin --verify --hp name:wordpress-5.1.1.zip
$ hashget-admin --verify --hp wordpress-5.1.1.zip

```

# Delete HashPackages
```shell
# Purge one HashPackage
$ hashget-admin --purge --hp wordpress-5.1.1.zip
# Purge HashPackage and web symlinks (if maintaining web db)
$ hashget-admin --purge --hp wordpress-5.1.1.zip --webroot /var/www/hashdb/

#
# VERY CAREFUL for commands below
#

# Purge ALL packages in project wp
$ hashget-admin --purge -p wp --hp all

# Purge ALL packages in ALL projects
$ hashget-admin --purge --hp all
```
