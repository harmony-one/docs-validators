# Post-Mortem Aug/6 shard 3 down

## Summary

At around 2019-08-06 07:18 UTC \(00:18 PDT\), a wrong version of node software with incomplete/incorrect foundational node table was deployed onto Harmony-operated nodes of mainnet shard 3. The nodes, forming the majority quorum of shard 3, minted roughly 800 blocks with insufficient number of signatures. The foundational nodes in shard 3 refused to sign these blocks, but nevertheless admitted the blocks into their copy of canonical chain where they should not have.

At around 2019-08-07 05:26 UTC \(2019-08-06 22:26 PDT\), Harmony deployed and released a node software to the issue above, in order to create a hard fork by discarding the ~800 incorrect blocks from the shard 3 blockchain.

## Customer Impact

Foundational nodes in shard 3 earned no block rewards for 22 hours. Foundational nodes in shards 0,1, and 2 continued to operate normally to earn block rewards.

## People Involved

* Eugene Kim \(ek@harmony.one\)
* Leo Chen \(leo@harmony.one\)
* Rongjian Lan \(rongjian@harmony.one\)
* Christopher Liu \(chris@harmony.one\)

## Timeline

2019-08-06 07:18 UTC \(00:18 PDT\) – ek@ and chris@ started rolling restart without uploading the updated version of Harmony binary onto the S3 folder used for internal release, to test rolling restart procedure itself before actually upgrading the mainnet.

2019-08-06 07:32 UTC \(00:32 PDT\) – chris@ found shard 3 stopped earning block rewards; ek@ looked into the nodes, found “My Committee updated” messages being emitted in a loop in the log, and reported this on \#team-dev on Discord:

![](https://lh3.googleusercontent.com/qjr3__6meEUEEz0ZnFe-FZ2WOnqqN9jnM17zvbECouWjyEKfduPdm-okOOSnnexCn4lWR_FD5OLVE3NJV5YV-G1453gi5Tiw9ON4ZSLZkN5ZDDTGdxKke_tRIfULqk5IcYlnJJgy)

2019-08-06 07:45 UTC \(00:45 PDT\) – ek@ found that a wrong version was used in the rolling upgrade test.

2019-08-06 07:45 UTC \(00:45 PDT\) – ek@ and leo@ started a video conference to discuss the strategy, then decided to use the db file from the sentry node to overwrite all harmony nodes and restart the shard.

2019-08-06 08:57 UTC \(01:57 PDT\) – ek@ killed all Harmony-operated nodes in shard 3, copied database from sentry node 3, and updated all Harmony-operated nodes in shard 3.

2019-08-06 09:30 UTC \(02:30 PDT\) approx. – ek@ found that consensus was still not reached. ek@ and leo@ further analyzed the logs, and found that the invalid blocks had been admitted into sentry 3’s copy of blockchain

2019-08-06 09:30 UTC \(02:30 PDT\) approx. - shard 3 was restarted, but consensus is still not reached. Further analysis indicates due to the block syncing logic bug, the blockchain we saved contains invalid blocks as well.

2019-08-07 04:16 UTC \(21:16 PDT\) - Last commit to revert the block was merged \([a4c1972c773e9c38380e598056b464e6c4008324](https://github.com/harmony-one/harmony/commit/a4c1972c773e9c38380e598056b464e6c4008324)\)

2019-08-07 04:20 UTC \(21:20 PDT\) - reboot the shard 3, by uploading new binary, re-init harmony node process.

2019-08-07 05:33 UTC \(22:33 PDT\) - shard 3 is brought back up. All nodes are doing consensus. Made the announcement to \#foundational-nodes channel.

2019-08-07 05:45 UTC \(22:45 PDT\) - FN runners are reporting their nodes are up and running.

![](https://lh6.googleusercontent.com/l2HWjwHL35Uj-aW5b6vxI2hG6InodOaQqtLEu9zyC_2y27kCWjav1lL1KQ9BiNq4h_dnVNj7tczae3Ncl878v9iL6XehFS11IohCAC2kkwQb5taohgKJre3NgHEci_Z2V29MasTH)

## Questions to ask

1. how long does it take to respond to the issue? how the problem was reported? how to cut it into half?
2. The issue was found by Eugene/Chris during the rolling update test in the first moment. The shard3 wasn’t reaching consensus after the rolling update. The problem was reported by Eugene to internal development channel. I don’t think we can cut the time to find the issue.
3. how long does it take to mitigate the issue? how to cut it into half?
4. It took around 22 hours from receiving the alert to fully restore the shard3. The initial recovery didn’t work and it was too late at time. The next day was spent on further investigation and testing of the block rollback fix. To cut the time in half requires us to have a better understanding of the block rollback mechanism.
5. Is there any pending items that can help prevent this issue? If so, why it wasn't prioritized?
6. We don’t have any pending items that can help prevent this issue. However, the rolling update process wasn’t well tested and reviewed before. That’s a miss of our design. We didn’t have much time to test and perfect the rolling update scripts.
7. Can this issue being detected in a test environment, if not, why?
8. We could detect this issue in a test environment. We missed it in our test environment due to the time pressure of this update.

## Root Cause Analysis

* The old version of the node software were rolling updated to all Harmony nodes in shard 3 and formed a majority quorum. A hard fork is created in this case. The quorum was generating block as usual. Due to the bug in our state syncing code, the node didn’t check if the block received from the quorum are valid block or not. Then the good node wrote the block into the blockchain and created corrupted blockchain database. None of the node can be recovered from the corrupted db and the hardfork.

## Five Whys

* Why the shard 3 was down?
  * The shard 3 was still running consensus by the quorum of Harmony nodes which are running the wrong version of software. Foundational nodes were unable to join consensus running the right version of software due to the number of keys are different in the two versions.
* Why the wrong version of software was deployed to Harmony nodes?
  * The rolling update script was using a shared team bucket unique-bucket-bin/ to retrieve the node software and deploy to Harmony nodes. The wrong version of the node software was uploaded to the shared bucket folder.
* Why the rolling update script didn’t check the software version?
  * This is a miss in our rolling update software. It simply trust engineers to upload the right version of software into the right bucket.
* Why the wrong version of software was uploaded to the production bucket folder?
  * The unique-bucket-bin/ bucket is a shared repo to store all the build artifacts and everyone has the write permission.  This is why the wrong version of the Harmony node program was uploaded to the official bucket folder and we have no log to track it. We have a script to upload the binaries automatically w/o checking 
* Why use the shard bucket and no permission control?
  * The deployment pipeline for mainnet was built upon the testing infrastructure where the permission is granted to everyone in the team and not managed. There is no finer grade of permission control designed for the unique-bucket-bin bucket.

## Action Items

| Category | Description | Owner | Tracker |
| :--- | :--- | :--- | :--- |
| Preventive | Fix issue that no verification that new blocks received from state syncing | Chao | [1313](https://github.com/harmony-one/harmony/issues/1313) |
| Preventive | Check the version of harmony node software before rolling update | Eugene | [1314](https://github.com/harmony-one/harmony/issues/1314) |
| Preventive | A standardized rolling update process document | Andy | [Done](https://docs.google.com/document/d/1JR03wlPCWCrABOm0fqNbgg9nrb7Zh2nr9HotO0ibPQY/edit) |
| Preventive | Use a different s3 bucket for rolling update | Eugene | [1316](https://github.com/harmony-one/harmony/issues/1316) |
| Preventive | Permission control on the rolling update bucket | Eugene | [1317](https://github.com/harmony-one/harmony/issues/1317) |
| Preventive | Enable access log on official bucket | Eugene | [1318](https://github.com/harmony-one/harmony/issues/1318) |
| Detective | Check the version of harmony node on each of the node | Andy | [1315](https://github.com/harmony-one/harmony/issues/1315) |
| Corrective | Rollback the hard fork blocks in the blockchain to resume the consensus | RJ | [Done](https://github.com/harmony-one/harmony/pull/1295) |
| Corrective | Deploy new node software to restore shard 3 | Eugene | Done |

