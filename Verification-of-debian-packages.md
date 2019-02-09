Download Release and Release.gpg from root of repository, e.g.
~~~
wget http://ftp.us.debian.org/debian/dists/Debian9.7/Release
wget http://ftp.us.debian.org/debian/dists/Debian9.7/Release.gpg
~~~

Import keys
~~~
$ gpg --import keys/*asc

gpg: key EF0F382A1A7B6500: 4 signatures not checked due to missing keys
gpg: key EF0F382A1A7B6500: "Debian Stable Release Key (9/stretch) <debian-release@lists.debian.org>" not changed
gpg: key 8B48AD6246925553: 2 signatures not checked due to missing keys
gpg: key 8B48AD6246925553: "Debian Archive Automatic Signing Key (7.0/wheezy) <ftpmaster@debian.org>" not changed
gpg: key 9D6D8F6BC857C906: 3 signatures not checked due to missing keys
gpg: key 9D6D8F6BC857C906: "Debian Security Archive Automatic Signing Key (8/jessie) <ftpmaster@debian.org>" not changed
gpg: key 7638D0442B90D010: 3 signatures not checked due to missing keys
gpg: key 7638D0442B90D010: "Debian Archive Automatic Signing Key (8/jessie) <ftpmaster@debian.org>" not changed
gpg: key EDA0D2388AE22BA9: 3 signatures not checked due to missing keys
gpg: key EDA0D2388AE22BA9: "Debian Security Archive Automatic Signing Key (9/stretch) <ftpmaster@debian.org>" not changed
gpg: key E0B11894F66AEC98: 3 signatures not checked due to missing keys
gpg: key E0B11894F66AEC98: "Debian Archive Automatic Signing Key (9/stretch) <ftpmaster@debian.org>" not changed
gpg: Total number processed: 6
gpg:              unchanged: 6

~~~

Trust keys
~~~
$ gpg --edit-key EDA0D2388AE22BA9
(trust, ultimate)
~~~

Verify:
~~~
$ gpg --verify Release.gpg Release
gpg: Signature made Thu Jan 24 02:14:01 2019 +07
gpg:                using RSA key A1BD8E9D78F7FE5C3E65D8AF8B48AD6246925553
gpg: Good signature from "Debian Archive Automatic Signing Key (7.0/wheezy) <ftpmaster@debian.org>" [ultimate]
gpg: Signature made Thu Jan 24 02:14:01 2019 +07
gpg:                using RSA key 126C0D24BD8A2942CC7DF8AC7638D0442B90D010
gpg: Good signature from "Debian Archive Automatic Signing Key (8/jessie) <ftpmaster@debian.org>" [ultimate]
gpg: Signature made Thu Jan 24 02:23:05 2019 +07
gpg:                using RSA key 067E3C456BAE240ACEE88F6FEF0F382A1A7B6500
gpg: Good signature from "Debian Stable Release Key (9/stretch) <debian-release@lists.debian.org>" [ultimate]

$ echo $?
0

~~~



List keys
~~~
$ gpg --list-keys
/home/username/.gnupg/pubring.kbx
------------------------------
...
pub   rsa4096 2017-05-20 [SC] [expires: 2025-05-18]
      067E3C456BAE240ACEE88F6FEF0F382A1A7B6500
uid           [ unknown] Debian Stable Release Key (9/stretch) <debian-release@lists.debian.org>
~~~

Add to keyring:
~~~
gpg --keyring ./my.gpg --no-default-keyring --import 067E3C456BAE240ACEE88F6FEF0F382A1A7B6500.asc
~~~