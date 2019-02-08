Download Release and Release.gpg from root of repository, e.g.
~~~
wget http://ftp.us.debian.org/debian/dists/Debian9.7/Release
wget http://ftp.us.debian.org/debian/dists/Debian9.7/Release.gpg
~~~

Verify (failed):
~~~
$ gpgv Release.gpg Release
gpgv: unknown type of key resource 'trustedkeys.kbx'
gpgv: keyblock resource '/home/xenon/.gnupg/trustedkeys.kbx': General error
gpgv: Signature made Thu Jan 24 02:14:01 2019 +07
gpgv:                using RSA key A1BD8E9D78F7FE5C3E65D8AF8B48AD6246925553
gpgv: Can't check signature: No public key
gpgv: Signature made Thu Jan 24 02:14:01 2019 +07
gpgv:                using RSA key 126C0D24BD8A2942CC7DF8AC7638D0442B90D010
gpgv: Can't check signature: No public key
gpgv: Signature made Thu Jan 24 02:23:05 2019 +07
gpgv:                using RSA key 067E3C456BAE240ACEE88F6FEF0F382A1A7B6500
gpgv: Can't check signature: No public key
~~~

Fetch key:
~~~
gpg --keyserver pgp.mit.edu --recv-keys 067E3C456BAE240ACEE88F6FEF0F382A1A7B6500
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