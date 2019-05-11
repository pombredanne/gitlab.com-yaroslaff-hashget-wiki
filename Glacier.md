# Prerequisites
hashget does not depends on any specific Glacier tools, you can use what is most comfortable to you. We use [glacier_upload](https://github.com/tbumi/glacier-upload).

## Install/configure glacier upload:
~~~
sudo pip3 install glacier_upload
~~~
<details>
<summary>Configure ~/.aws/config</summary>

~~~
[default]
region=eu-central-1
aws_access_key_id = YOUR_ACCESS_KEY_ID
aws_secret_access_key = YOUR_SECRET_ACCESS_KEY
~~~
</details>

<details>
<summary>Make random test file /tmp/delme.tar.gz</summary>

~~~
$ mkdir /tmp/delme

$ dd if=/dev/urandom of=/tmp/delme/1M bs=1M count=1
1+0 records in
1+0 records out
1048576 bytes (1.0 MB, 1.0 MiB) copied, 0.00605954 s, 173 MB/s

$ tar -czf /tmp/delme.tar.gz -C /tmp/delme/ . 
~~~
Now we have test archive /tmp/delme.tar.gz
</details>

# Uploading and indexing files
Uploading file /tmp/delme.tar.gz:
<details>
<summary>glacier_upload -v MyVault -d "My test archive. Delete me." -f /tmp/delme.tar.gz 
</summary>

~~~
Reading file...
Opened single file.
Initiating multipart upload...
File size is 1049152 bytes. Will upload in 1 parts.
Spawning threads...
Uploading part 1 of 1... (0.00%)
Completing multipart upload...
Upload successful.
Calculated total tree hash: 02faa557c9ea01e583fee4980195bfc5369a2cb49c05dfe5038149ca02d91008
Glacier total tree hash: 02faa557c9ea01e583fee4980195bfc5369a2cb49c05dfe5038149ca02d91008
Location: /985538140660/vaults/MyVault/archives/xa06zlRje02jSPTp5X4y9QreX62GUnUH7igVcaUStzmBXGhForTYJUOzym1Ff8VJSZQg4x-MI0uLjK-n11vFf6UV6nJB-V3loim1VFm9dtOM5UurlJIcdisGB3QftPcBT-ECA0mWFw
Archive ID: xa06zlRje02jSPTp5X4y9QreX62GUnUH7igVcaUStzmBXGhForTYJUOzym1Ff8VJSZQg4x-MI0uLjK-n11vFf6UV6nJB-V3loim1VFm9dtOM5UurlJIcdisGB3QftPcBT-ECA0mWFw
Done.

~~~
</details>

Here we just need Archive ID: `xa06zlRje02jSPTp5X4y9QreX62GUnUH7igVcaUStzmBXGhForTYJUOzym1Ff8VJSZQg4x-MI0uLjK-n11vFf6UV6nJB-V3loim1VFm9dtOM5UurlJIcdisGB3QftPcBT-ECA0mWFw`

We will index this file now (and set expiration to 18 May 2019):
~~~
hashget --submit glacier://xa06zlRje02jSPTp5X4y9QreX62GUnUH7igVcaUStzmBXGhForTYJUOzym1Ff8VJSZQg4x-MI0uLjK-n11vFf6UV6nJB-V3loim1VFm9dtOM5UurlJIcdisGB3QftPcBT-ECA0mWFw --file /tmp/delme.tar.gz --expires 2019-05-15 --project glacier --hashserver --expires 2019-05-18
~~~

> Note, we used unusual url `glacier://`. hashget can not download such unusual URLs, but we will use [pool](filepool) mechanism for this.