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

apache config:
~~~
<VirtualHost *:443>
    ServerName hashdb.okerr.com
    DocumentRoot /var/www/virtual/hashdb.okerr.com/
    ProxyPass /submit unix:/var/run/takeup/takeup.sock|uwsgi://zzz/
     
    SSLEngine on
    SSLCertificateFile /etc/letsencrypt/live/hashdb.okerr.com/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/hashdb.okerr.com/privkey.pem

    Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains"
      
</VirtualHost>

<VirtualHost *:80>
    DocumentRoot /var/www/virtual/hashdb.okerr.com/
    ServerName hashdb.okerr.com
    ProxyPass /submit unix:/var/run/takeup/takeup.sock|uwsgi://zzz/

    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteCond %{REQUEST_URI} !^/\.well\-known        
    RewriteRule (.*) https://%{SERVER_NAME}$1 [R=301,L]
</VirtualHost>
~~~

If accepting uploads via [takeup](https://gitlab.com/yaroslaff/takeup/), use this /etc/cron.hourly/hashget:
~~~
#!/bin/sh

/usr/local/bin/hashget-admin --logfile /var/log/hashget.log --submitted /var/run/takeup/uploads/new/ /var/www/virtual/hashdb.okerr.com/hashdb/
~~~