# Create web repository
~~~
hashget-admin -v --build /var/www/virtual/hashdb.okerr.com/hashdb/ --project debsnap
~~~
make sure debsnap project is debsnap type:
~~~
# hashget-admin --status -p debsnap
debsnap DirHashDB(path:/var/cache/hashget/hashdb/debsnap stor:basename pkgtype:debsnap packages:0)
...
~~~

motd.txt - plain text file

config.json:
~~~
{
    "name": "okerr hashdb",
    "motd": "https://hashdb.okerr.com/hashdb/motd.txt",
    "submit": "https://hashdb.okerr.com/submit",
    "accept_url": [ "^https?://snapshots?.debian.org/archive/" ]
}
~~~