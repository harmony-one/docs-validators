# Release Status

## CURRENT RELEASE

**Release Time** - Monday Dec 09 - 10:00 PST 2019

**Release Branch** - s3

**Release Type** - Rolling Upgrade

**Release Version** - Harmony \(C\) 2019. harmony, version v4883-v1-20191207.0-2-g5d53e6eb \(jenkins@ 2019-12-10T01:21:54+0000\)

### CHANGESET

* bugfix:
  * Verify multiple viewID in view change message
  * Do not propose new block if the node is not the leader
  * Epoch change leader did not propose block
* feature/enhancement:
  * Removed transaction throttling on mainnet

## PREVIOUS RELEASE

**Expected Release Time** - Tue Oct 08 21:00:00 UTC 2019

**Release Type** - Rolling Upgrade

**Release Version** - Harmony \(C\) 2019. harmony, version v4811-v1-20191008.0-0-gd34bc426 \(jenkins@ 2019-10-09T01:11:22+0000\)

### CHANGESET

* bugfix:
  * Fix blocking issue for initial consensus bootstrap 
  * Fix API issues for smart contract transaction handling
  * Fix index OOB bug in explorer API
* feature/enhancement:
  * Separated p2p group ID for mainnet, testnet and pangaea network
  * Added sender key check in consensus announce message handler.

## PREVIOUS RELEASE - V1-20191001.1 release

**Expected Release Time** - Tue Oct 01 21:00:00 UTC 2019

**Release Type** - Rolling Upgrade

**Release Version** - Harmony \(C\) 2019. harmony, version v4772-v1-20191001.1-0-gf112794d \(jenkins@ 2019-10-02T02:27:20+0000\)

### CHANGESET

* bugfix:
  * Fix cross-shard transaction committee verification error
  * Fix ChainID comparison bug on harmony API
* feature/enhancement:
  * Avoid computation-intensive initialization of VDF at the start of node program
  * Avoid duplicate transactions and receipts in the pending pool
* misc: 
  * Add -P option on node.sh to enable public rpc
  * Add -v option on node.sh to show node.sh's version

## PREVIOUS RELEASE - V1-20190924.0 release

**Release Time** - Tue Sep 24 21:00:00 UTC 2019

**Release Type** - Rolling Upgrade

**Release Version** - Harmony \(C\) 2019. harmony, version v4704-v1-20190924.0-0-g252aa562 \(jenkins@ 2019-09-25T00:05:28+0000\)

### CHANGESET

* bugfix:
  * Explorer stability fixes \(epoch transition, disable beacon sync etc.\)
  * Fix explorer's wrong balance issue for cross-shard transactions
* feature/enhancement:
  * Add getShardID, latestHeader and cross-shard receipt lookup APIs
  * Improve efficiency of state synchronization
  * Add pangaea specific ChainID
* misc: 
  * node http server listens to localhost by default

### ACTION REQUIRED

**Auto Upgrade** - None

**Manual Upgrade** - Build on Harmony s3 branch, last commit is: [\#1639](https://github.com/harmony-one/harmony/pull/1639)

## PREVIOUS RELEASE - V1-20190917.0 release

**Release Time** - Tue Sep 17 23:30:06 UTC 2019

**Release Type** - Rolling Upgrade

**Release Version** - Harmony \(C\) 2019. harmony, version v4596-v1-20190917.0-2-g3232e519 \(jenkins@ 2019-09-18T04:19:21+0000\)

### CHANGESET

* bugfix:
  * potential memory leak on beacon chain node \([\#1588](https://github.com/harmony-one/harmony/pull/1588)\)
  * beacon syncing blocking issue \([\#1586](https://github.com/harmony-one/harmony/pull/1586/files)\)
  * additional info on cross-shard transactions
* feature/enhancement:
  * [harmony cli ](https://github.com/harmony-one/go-sdk)tool with ledger support for transaction on mainnet
  * [math wallet chrome extension](https://chrome.google.com/webstore/detail/math-wallet/afbcbjpbpfadlkmhmclhkeeodmamcflc?hl=en) support on mainnet
  * ~~unified end point for SDK and explorer RPC service \(~~[~~\#1587~~](https://github.com/harmony-one/harmony/pull/1587)~~\)~~
  * ~~new API for cross-shard transactions~~
  * **cross-shard tx enabled on mainnet**
* misc: 
  * node.sh update to support non-validating nodes \([\#1589](https://github.com/harmony-one/harmony/pull/1589)\)

### ACTION REQUIRED

**Auto Upgrade** - None

**Manual Upgrade** - Build on Harmony s3 branch, last commit is: [\#1610](https://github.com/harmony-one/harmony/commit/0903a96db865a6ee824b609124b943bfab388f66)

## PREVIOUS RELEASE - V1-20190911.0 release

**Release Time** - Wed Sep 11 08:04:27 UTC 2019

**Release Type** - Rolling Upgrade

**Release Version** - v4560-v1-20190911.1-0-gaafdaa47

### ACTION REQUIRED

**Auto Upgrade -** None

**Manual Upgrade -** Build on harmony master branch, last commit is:  
[ https://github.com/harmony-one/harmony/commit/fefc3432412666c3123d19971720ca8f8469f573](https://github.com/harmony-one/harmony/commit/aafdaa47b93dcb875a5f98c286ffd21aff43d999)

**Disk Space** - Ensure you have at least 60GB of space on your disk

* [AWS Upgrade Guide - English](https://docs.google.com/document/d/1oAwjvNHMUlf4lfdR3U_NdQWKrgGKYjlKBazWxzm496I/edit?usp=sharing)
* [AWS Upgrade Guide - Chinese](https://docs.google.com/document/d/1V1veFmgW0V-PKWacuaB_4_0kD_cDFjeAWxZHaDopgWo/edit?usp=sharing)

{% file src="../.gitbook/assets/how-to-increse-disk-space-in-aws-eng.pdf" caption="Download AWS Upgrade Guide Engilsh" %}

{% file src="../.gitbook/assets/how-to-increse-disk-space-in-aws-chn.pdf" caption="Download AWS Upgrade Guide Chinese" %}

## PREVIOUS RELEASE - S3 release

**Release Date -** Tuesday September 3rd

**Release Type** - Rolling Upgrade

**Release Version** - v4351-s3-20190903.0-0-gc3f4cb91

**Release Detail** - Harmony \(C\) 2019. harmony, version v4351-s3-20190903.0-0-gc3f4cb91 \(ec2-user@ 2019-09-04T02:45:33+0000\)

## ACTION REQUIRED

**Manual Upgrade** - manual upgrade is needed

download the [node.sh](https://harmony.one/node.sh) from [https://harmony.one/node.sh](https://harmony.one/node.sh)

**Check the Harmony version:**

`LD_LIBRARY_PATH=$(pwd) ./harmony -version`

restart the node.sh using the original options you started the node. If no new binary is downloaded, remove the "md5sum.txt" file using `rm md5sum.txt`.

## PREVIOUS RELEASE - Day ONE Mainnet

**Release Date -** Friday June 28th 08:43 Pacific Time

**Release Type -** [Manual Upgrade](https://github.com/harmony-one/nodes-wiki/tree/124a4fa71024022e8d5ecf62e7efcbb694665a79/docs/playbook/cheatsheet.md)

**Release Version -** v3805-r3\_20190626-37-g43191b8e

**Release Detail -** Harmony \(C\) 2019. harmony, version v3805-r3\_20190626-37-g43191b8e \(jenkins@ 2019-06-28T15:10:57+0000\)

## ACTION REQUIRED

**Auto Upgrade -** None

**Manual Upgrade -** [Manually restart node.sh](https://github.com/harmony-one/nodes-wiki/tree/124a4fa71024022e8d5ecf62e7efcbb694665a79/docs/playbook/cheatsheet.md)

## PLANNED RELEASES

**Mainnet Phase 2**

**Mainnet Phase 3**

## PREVIOUS RELEASES

**Release Date -** Tuesday June 25th 19:00 Pacific Time

**Release Type -** Rolling Upgrade

**Release Version -** v3768-r3\_20190626-0-g12bb1d44

**Release Detail -** Harmony \(C\) 2018. harmony, version v3768-r3\_20190626-0-g12bb1d44 \(jenkins@ 2019-06-26T02:45:49+0000\)

