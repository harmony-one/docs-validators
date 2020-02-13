# Downloading Blockchain Database

## Downloading Blockchain Database - NOT FOR OSTN

Before the node can run and join consensus to earn block rewards, it will need a recent copy of blockchain database.  In order to download and extract the blockchain database, run the following commands \(replace `BLSPUBKEY` with your 96-digit hexadecimal BLS public key as displayed by the `generate-bls-key` command above\):

```text
curl -sSfLO https://hmny-pnga.s3-us-west-2.amazonaws.com/db/download-pangaea-db.sh
sh download-pangaea-db.sh BLSPUBKEY
```

This command takes up to an hour to complete.  After that, the following messages are displayed \(the shard\(s\) downloaded depends on your BLS public key\):

```text
Starting download of shard 2 database...
Starting download of shard 0 database...
Waiting for database download to finish...
Database download finished.

Ensure that there are no error messages above.  If any, re-run this script.
```

## 

