### Backup
<details><summary>examples</summary>

~~~shell
#
# Backup and restore
#

# Backup
$ hashget -zf FILENAME --pack DIRECTORY --hashserver


# Pack with restic
$ hashget -X exclude-list --prepack wordpress --hashserver
Saved: 1468 files, 1 pkgs, size: 40.5M. Download: 10.7M

$ restic --exclude-file exclude-list backup wordpress
password is correct
scan [/tmp/wp/wordpress]
scanned 193 directories, 367 files in 0:02
[0:04] 100.00%  700.829 KiB / 700.829 KiB  560 / 560 items  0 errors  ETA 0:00 
duration: 0:04
snapshot 76b54230 saved
~~~
</details>


### Restore
<details><summary>examples</summary>

~~~shell
$ hashget -u DIRECTORY --hashserver

# Unpack with restic
$ restic restore 76b54230 -t unpacked
password is correct
restoring <Snapshot 76b54230 of [/tmp/wp/wordpress] at 2019-06-19 04:30:55.760618336 +0700 +07 by root@braconnier> to unpacked
$ hashget -u unpacked/wordpress/ --hashserver
Recovered 1468/1468 files 40.5M bytes (0 downloaded, 0 from pool, 10.7M cached) in 1.56s
~~~
</details>

### Submitting
~~~
$ hashget --submit https://wordpress.org/wordpress-5.2.2.tar.gz -p my --file wordpress-5.2.2.tar.gz --hashserver
~~~

### Info about HashPackages
~~~
$ hashget-admin --status
$ hashget-admin --list -p PROJECT
$ hashget-admin --get url expires -p PROJECT
~~~

### FilePool
~~~
$ hashget-admin --build /var/www/html/hashdb/ --pool /tmp/pool
$ hashget-admin --build . --pool /tmp/pool
build pool . in webroot .
./pool/sha256/04/90/05/a9f08b11fb74faf98985ac6844cf4e0b9c47f48e3059c41db78b4f9d8c > /tmp/pool/wordpress-5.2.1.zip
$ python3 -m http.server
$ hashget -u PATH --pool http://localhost:8000/ /tmp/pool2
~~~
