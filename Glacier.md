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

$ dd if=/dev/urandom of=/tmp/delme/100M bs=1M count=100
100+0 records in
100+0 records out
104857600 bytes (105 MB, 100 MiB) copied, 0.558418 s, 188 MB/s

$ tar -czf /tmp/delme.tar.gz -C /tmp/delme/ . 

$ ls -lh /tmp/delme.tar.gz 
-rw-r--r-- 1 xenon xenon 101M May 11 17:12 /tmp/delme.tar.gz
~~~
Now we have test archive /tmp/delme.tar.gz
</details>

# Uploading and indexing files

## Uploading full backup
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
File size is 104874548 bytes. Will upload in 13 parts.
Spawning threads...
Uploading part 1 of 13... (0.00%)
Uploading part 2 of 13... (7.69%)
Uploading part 3 of 13... (15.38%)
Uploading part 4 of 13... (23.08%)
Uploading part 5 of 13... (30.77%)
Uploading part 6 of 13... (38.46%)
Uploading part 7 of 13... (46.15%)
Uploading part 8 of 13... (53.85%)
Uploading part 9 of 13... (61.54%)
Uploading part 10 of 13... (69.23%)
Uploading part 11 of 13... (76.92%)
Uploading part 12 of 13... (84.62%)
Uploading part 13 of 13... (92.31%)
Completing multipart upload...
Upload successful.
Calculated total tree hash: 19d205711cd282e9b8f00b6b92a36f47e9a746591323643bb8783ffdbc99d67a
Glacier total tree hash: 19d205711cd282e9b8f00b6b92a36f47e9a746591323643bb8783ffdbc99d67a
Location: /985538140660/vaults/MyVault/archives/e-v_lBA2iTqUNBpPqlq4oPaz-1LYLtSeqXuGyq5dG4rrw7fAf0GEXPPcpxmKC443Fqj5nbODV9Sw8poT1CB8CSKvar5ff2EI1FH-L1w_4vFMXDyLEuQzDwMYJt6N-xtaJGFD4U-JTg
Archive ID: e-v_lBA2iTqUNBpPqlq4oPaz-1LYLtSeqXuGyq5dG4rrw7fAf0GEXPPcpxmKC443Fqj5nbODV9Sw8poT1CB8CSKvar5ff2EI1FH-L1w_4vFMXDyLEuQzDwMYJt6N-xtaJGFD4U-JTg
Done.
~~~
</details>

Here we just need Archive ID: `e-v_lBA2iTqUNBpPqlq4oPaz-1LYLtSeqXuGyq5dG4rrw7fAf0GEXPPcpxmKC443Fqj5nbODV9Sw8poT1CB8CSKvar5ff2EI1FH-L1w_4vFMXDyLEuQzDwMYJt6N-xtaJGFD4U-JTg`

We will index this file now (and set expiration to 18 May 2019):
~~~
hashget --submit e-v_lBA2iTqUNBpPqlq4oPaz-1LYLtSeqXuGyq5dG4rrw7fAf0GEXPPcpxmKC443Fqj5nbODV9Sw8poT1CB8CSKvar5ff2EI1FH-L1w_4vFMXDyLEuQzDwMYJt6N-xtaJGFD4U-JTg --file /tmp/delme.tar.gz --expires 2019-05-15 --project glacier --hashserver --expires 2019-05-18
~~~

> Note, we used unusual url (glacier upload id). hashget can not download such unusual URLs, but we will use [pool](filepool) mechanism for this.

<details><summary>You can also add other tags to HashDB information and inspect it easily.</summary>

~~~
$ hashget-admin --hp url:e-v_lBA2iTqUNBpPqlq4oPaz-1LYLtSeqXuGyq5dG4rrw7fAf0GEXPPcpxmKC443Fqj5nbODV9Sw8poT1CB8CSKvar5ff2EI1FH-L1w_4vFMXDyLEuQzDwMYJt6N-xtaJGFD4U-JTg --set desc "My delme.tar.gz full backup"
$ hashget-admin --get expires desc url -p glacier
2019-05-18 00:00:00 My delme.tar.gz full backup e-v_lBA2iTqUNBpPqlq4oPaz-1LYLtSeqXuGyq5dG4rrw7fAf0GEXPPcpxmKC443Fqj5nbODV9Sw8poT1CB8CSKvar5ff2EI1FH-L1w_4vFMXDyLEuQzDwMYJt6N-xtaJGFD4U-JTg
~~~
</details>

## Make differential backup
Now we will make little change to our data and make hashget archive:
~~~
$ dd if=/dev/urandom of=/tmp/delme/10M bs=1M count=10
10+0 records in
10+0 records out
10485760 bytes (10 MB, 10 MiB) copied, 0.191431 s, 54.8 MB/s
~~~

And make new archive:
~~~
$ hashget --pack /tmp/delme -zf /tmp/delme2.tar.gz --expires 2019-05-18 --hashserver
~~~
<details><summary>output</summary>

~~~
STEP 1/3 Indexing...
Indexing done in 0.53s. 0 local + 0 pulled + 0 new = 0 total packages
total=0 local=0 pulled=0 new=0
STEP 2/3 prepare exclude list for packing...
Saved: 1 files, 1 pkgs, size: 100.0M. Download: 100.0M
STEP 3/3 tarring...
/tmp/delme (110.0M) packed into /tmp/delme2.tar.gz (10.0M)

~~~
</details>
As you see, package is deduplicated at 100% (all 100M of duplicate data was skipped).

If you will make simple .tar archive or hashget archive where expiration date will be set after expiration date of full backup, it will take 110Mb.

## Restore backup

Now, we want to restore from differential backup delme2.tar.gz. Obtain it usual way as you download from glacier and unpack.

~~~
mkdir /tmp/unpacked
tar -C /tmp/unpacked/ -xf /tmp/delme2.tar.gz
~~~ 
<details>
<summary>If you are curious, you can inspect .hashget-restore</summary>

```json
{
    "expires": "2019-05-18",
    "files": [
        {
            "atime": 1557569560,
            "ctime": 1557569556,
            "file": "100M",
            "gid": 1000,
            "mode": 420,
            "mtime": 1557569556,
            "sha256": "31ce6ee9d44cf793381b3a15336f6638e1c3e719cdcf4efaefd17398d98e02c7",
            "size": 104857600,
            "uid": 1000
        }
    ],
    "packages": [
        {
            "hash": "sha256:d3ad034c0100233591fe75a108c4876424869a1997b35505404bdf854df3b857",
            "url": "e-v_lBA2iTqUNBpPqlq4oPaz-1LYLtSeqXuGyq5dG4rrw7fAf0GEXPPcpxmKC443Fqj5nbODV9Sw8poT1CB8CSKvar5ff2EI1FH-L1w_4vFMXDyLEuQzDwMYJt6N-xtaJGFD4U-JTg"
        }
    ],
    "packagesize": 104874548,
    "packagesize_exact": true
}
```
</details>

<details>
<summary>You can get ArchiveID for all files:</summary>

~~~
$ cat .hashget-restore.json |grep '"url"'| cut -f 4 -d\"
e-v_lBA2iTqUNBpPqlq4oPaz-1LYLtSeqXuGyq5dG4rrw7fAf0GEXPPcpxmKC443Fqj5nbODV9Sw8poT1CB8CSKvar5ff2EI1FH-L1w_4vFMXDyLEuQzDwMYJt6N-xtaJGFD4U-JTg
~~~
</details>

Download file(s) from glacier to any directory and unpack with pool:
~~~
$ hashget -u /tmp/unpacked/ --pool /tmp/glacier/ --hashserver --user
Recovered 1/1 files 100.0M bytes (0 downloaded, 100.0M from pool, 0 cached) in 4.98s
~~~

## Delete files from Glacier
You can use hashget HashDB for cleaning glacier vaults.

This command will list glacier UploadIDs which is expired for today
~~~
hashget-admin --hp expired -p glacier --get url
~~~

And this command will list glacier UploadIDs which is expired at this date (in future or in past):
~~~
$ hashget-admin --hp expired:2019-05-31 -p glacier --get url
e-v_lBA2iTqUNBpPqlq4oPaz-1LYLtSeqXuGyq5dG4rrw7fAf0GEXPPcpxmKC443Fqj5nbODV9Sw8poT1CB8CSKvar5ff2EI1FH-L1w_4vFMXDyLEuQzDwMYJt6N-xtaJGFD4U-JTg
~~~

Then you can use `delete_glacier_archive` utility from glacier-upload package to delete it from vault.

## Cheatsheet for glacier_upload
~~~
$ init_inventory_retrieval -v MyVault
Sending inventory-retrieval initiation request...
Job initiation request recieved. Job ID: UDyUTfn_P84JSqI0UbnlIwuk0U4S0h1Wwgv6XXvQimE76h3Ucm8t_WMb-5RFdjpz5LOP1YYFXdqRg2yXLbxI-KKJfBoV
~~~