You can combine external deduplication (provided by hashget) with internal deduplication and other features or [restic](https://restic.net/).

Prepare (download external data and index it)
~~~
wget https://wordpress.org/wordpress-5.2.2.tar.gz
hashget --submit https://wordpress.org/wordpress-5.2.2.tar.gz -p my --file wordpress-5.2.2.tar.gz
tar -xf wordpress-5.2.2.tar.gz
~~~

We will start with empty restic repository in /tmp/restic-repo:
~~~
$ export RESTIC_PASSWORD_FILE=/home/xenon/rpass
$ export RESTIC_REPOSITORY=/tmp/restic-repo/
$ du -sh /tmp/restic-repo/
1,1M	/tmp/restic-repo/
$ du  --apparent-size -sh wordpress
42M	wordpress
~~~

Now, prepare .hashget-restore.json and exclude files:
~~~
hashget -X exclude-list --prepack wordpress --hashserver
Saved: 1468 files, 1 pkgs, size: 40.5M. Download: 10.7M
~~~
Note, hashget promises to save 40Mb out of 42 total. Sounds good. Lets check it!

Now, add this wordpress directory to restic repository:
~~~
$ restic --exclude-file exclude-list backup wordpress
password is correct
scan [/tmp/wp/wordpress]
scanned 193 directories, 367 files in 0:02
[0:04] 100.00%  700.829 KiB / 700.829 KiB  560 / 560 items  0 errors  ETA 0:00 
duration: 0:04
snapshot bdbdeeea saved

$ du -sh /tmp/restic-repo/
2,1M	/tmp/restic-repo/
~~~

We added ~40Mb package and this increased repo size to just 1Mb. 

Now, let's unpack it:
~~~
$ restic restore bdbdeeea -t unpacked
password is correct
restoring <Snapshot bdbdeeea of [/tmp/wp/wordpress] at 2019-06-19 04:00:50.27835785 +0700 +07 by root@braconnier> to unpacked

$ du -sh unpacked/
2,8M	unpacked/
~~~

Totally, we had about 2.8Mb stored into restic (out of 40 total). This is very short files under 1Kb and one 500Kb .hashget-restore.json

Now, restore it:
~~~
$ hashget -u unpacked/wordpress/ --hashserver
Recovered 1468/1468 files 40.5M bytes (0 downloaded, 0 from pool, 10.7M cached) in 1.60s
$ du --apparent-size -sh unpacked/wordpress/
42M	unpacked/wordpress/
~~~

We got exactly same result, but it took just 1Mb in our repository instead of 40+ Mb. 