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

## Uploading full archive
Uploading file /tmp/delme.tar.gz as base (full) archive:
~~~
glacier_upload -v MyVault -d "My test archive. Delete me." -f /tmp/delme.tar.gz
~~~
<details>
<summary>output</summary>

~~~
Reading file...
Opened single file.
Initiating multipart upload...
File size is 1048997 bytes. Will upload in 1 parts.
Spawning threads...
Uploading part 1 of 1... (0.00%)
Completing multipart upload...
Upload successful.
Calculated total tree hash: 1be67b4b6f599087503385a812cfb4a56d403f171b3aa9f7df6b6c36cb10c9b4
Glacier total tree hash: 1be67b4b6f599087503385a812cfb4a56d403f171b3aa9f7df6b6c36cb10c9b4
Location: /985538140660/vaults/MyVault/archives/QoUIGQ04973_nd-fkjBk1CzAz6fvFcgCnrCSB0tt539MG2zfWRz_LZ3J3Ht96cfs1ZZv21_WUm7h94DYRUUY5UCEgIozshcuTTTnZSFYUrdDcjSaK64nQcW28quv1g9F61cV-EmGYw
Archive ID: QoUIGQ04973_nd-fkjBk1CzAz6fvFcgCnrCSB0tt539MG2zfWRz_LZ3J3Ht96cfs1ZZv21_WUm7h94DYRUUY5UCEgIozshcuTTTnZSFYUrdDcjSaK64nQcW28quv1g9F61cV-EmGYw
Done.
~~~
</details>

Here we just need Archive ID: `QoUIGQ04973_nd-fkjBk1CzAz6fvFcgCnrCSB0tt539MG2zfWRz_LZ3J3Ht96cfs1ZZv21_WUm7h94DYRUUY5UCEgIozshcuTTTnZSFYUrdDcjSaK64nQcW28quv1g9F61cV-EmGYw`

We will index this file now (and set expiration to 18 May 2019):
~~~
hashget --submit glacier://QoUIGQ04973_nd-fkjBk1CzAz6fvFcgCnrCSB0tt539MG2zfWRz_LZ3J3Ht96cfs1ZZv21_WUm7h94DYRUUY5UCEgIozshcuTTTnZSFYUrdDcjSaK64nQcW28quv1g9F61cV-EmGYw --file /tmp/delme.tar.gz --expires 2019-05-15 --project glacier --hashserver --expires 2019-05-18
~~~

> Note, we used unusual url `glacier://`. hashget can not download such unusual URLs, but we will use [pool](filepool) mechanism for this.

