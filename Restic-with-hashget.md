It's possible to combine external deduplication (provided by hashget) with internal deduplication and other nice features provided by [restic](https://restic.net/).

We will start with empty restic repository and put wordpress there (with power of hashget).
~~~
# du -sh /tmp/restic-repo/
1,1M	/tmp/restic-repo/
~~~

Lets create working directory and index wordpress:
~~~
# wget -q https://wordpress.org/wordpress-5.2.2.tar.gz
# hashget --submit https://wordpress.org/wordpress-5.2.2.tar.gz -p my --file wordpress-5.2.2.tar.gz --hashserver
# tar -xf wordpress-5.2.2.tar.gz
# du -sh wordpress
46M	wordpress
~~~

Now, main part: prepare exclude files and backup to restic
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

We stored 40Mb directory and this consumed just 1Mb in restic repo (40 times less because of hashget external deduplication).

Recovery process:
~~~
# restic restore 76b54230 -t unpacked
password is correct
restoring <Snapshot 76b54230 of [/tmp/wp/wordpress] at 2019-06-19 04:30:55.760618336 +0700 +07 by root@braconnier> to unpacked
# du -sh unpacked/wordpress/
2,8M	unpacked/wordpress/
# hashget -u unpacked/wordpress/ --hashserver
Recovered 1468/1468 files 40.5M bytes (0 downloaded, 0 from pool, 10.7M cached) in 1.56s
# rm unpacked/wordpress/.hashget-restore.json 
# du -sh unpacked/wordpress/
46M	unpacked/wordpress/
~~~
