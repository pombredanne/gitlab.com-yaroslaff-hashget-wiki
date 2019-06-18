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
# du -sh wordpress
46M	wordpress
~~~
