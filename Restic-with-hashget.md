It's possible to combine external deduplication (provided by hashget) with internal deduplication and other nice features provided by [restic](https://restic.net/).

We will start with empty restic repository and put wordpress there (with power of hashget).
~~~
# du -sh /tmp/restic-repo/
1,1M	/tmp/restic-repo/

Lets create working directory and index wordpress:
~~~
# wget -q https://wordpress.org/wordpress-5.2.2.tar.gz
# hashget --submit https://wordpress.org/wordpress-5.2.2.tar.gz -p my --file wordpress-5.2.2.tar.gz --hashserver
# du -sh wordpress
46M	wordpress
~~~

Now, prepare exclude files (`hashget --prepack`) and backup to restic
~~~
# hashget -X exclude-list --prepack wordpress --hashserver
Saved: 1468 files, 1 pkgs, size: 40.5M. Download: 10.7M

# restic --exclude-file exclude-list backup wordpress
password is correct
scan [/tmp/wp/wordpress]
scanned 193 directories, 367 files in 0:02
[0:04] 100.00%  700.829 KiB / 700.829 KiB  560 / 560 items  0 errors  ETA 0:00 
duration: 0:04
snapshot 76b54230 saved

# du -sh /tmp/restic-repo/
2,1M	/tmp/restic-repo/
~~~

