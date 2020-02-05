# Post-Mortem July 23-24 shards 0,1,2,3 down

## Summary:

Between late night July 23 2019 to noon July 24 2019 PST, shard 0, 2, 1, 3 stopped generating new blocks one after another:

Shard 0 down for 4 hours and 16 minutes \(July 23 09:51:01pm - July 24 02:07:26am\)

* Last block before shard being down: [https://explorer.harmony.one/\#/block/0x5bd943083f615284a1385423716910ff37dd55831568a42d6bae5615a94dd6e8](https://explorer.harmony.one/#/block/0x5bd943083f615284a1385423716910ff37dd55831568a42d6bae5615a94dd6e8)
* First block after recovery [https://explorer.harmony.one/\#/block/0xf7ec49b25f1c7345628478c65237c1061fea1a90f62613f51bef03d32a9027cb](https://explorer.harmony.one/#/block/0xf7ec49b25f1c7345628478c65237c1061fea1a90f62613f51bef03d32a9027cb)

  Shard 2 down for 3 hours and 6 minutes \(July 24 07:13:10am - 10:19:11am\)

* Last block before shard being down: [https://explorer.harmony.one/\#/block/0x721352b467db265f6389ecabd2c8ad4e084daf0c76da64e2d03d2609369b848f](https://explorer.harmony.one/#/block/0x721352b467db265f6389ecabd2c8ad4e084daf0c76da64e2d03d2609369b848f)
* First block after recovery [https://explorer.harmony.one/\#/block/0x6116da153f91aee12c219676f59bf630ac5b831457fcc327654d4ace34274629](https://explorer.harmony.one/#/block/0x6116da153f91aee12c219676f59bf630ac5b831457fcc327654d4ace34274629)

  Shard 1 down for 1 hour and 3 minutes \(July 24 10:23:45am - 11:26:30am\)

* Last block before shard being down: [https://explorer.harmony.one/\#/block/0xbecff60aca10376ed43e6726d0245e93016aae99da99f8dbee04811c6f276822](https://explorer.harmony.one/#/block/0xbecff60aca10376ed43e6726d0245e93016aae99da99f8dbee04811c6f276822)
* First block after recovery [https://explorer.harmony.one/\#/block/0x98ebcec83ff2eba74f83b849a881f61da4712204a401ab60de86351e1a80d1e9](https://explorer.harmony.one/#/block/0x98ebcec83ff2eba74f83b849a881f61da4712204a401ab60de86351e1a80d1e9)

  Shard 3 down for 1 hour and 56 minutes \(July 24 12:03:23pm - 01:59:38pm\)

* Last block before shard being down: [https://explorer.harmony.one/\#/block/0xbfc60538ebcaed0864dbe3213f80c4f758da1867b1e61e620a2c66fccf6248cc](https://explorer.harmony.one/#/block/0xbfc60538ebcaed0864dbe3213f80c4f758da1867b1e61e620a2c66fccf6248cc)
* First block after recovery [https://explorer.harmony.one/\#/block/0x227cd8146b5b462345ff7222d64a1811478d96617fc25bbe9b3afa5913fa038f](https://explorer.harmony.one/#/block/0x227cd8146b5b462345ff7222d64a1811478d96617fc25bbe9b3afa5913fa038f)

  Shard 3 down again for 1 hour and 7 minutes \(July 24 03:29:00pm - 04:36:34pm\)

* Last block before shard being down: [https://explorer.harmony.one/\#/block/0xfd3f818668e29478d7811242a9f8f8a927e93950532cc9710c28d661a50d0865](https://explorer.harmony.one/#/block/0xfd3f818668e29478d7811242a9f8f8a927e93950532cc9710c28d661a50d0865)
* First block after recovery [https://explorer.harmony.one/\#/block/0xdc6d31cc9e16ee9e121d784cc3aff3ec134c3f65fab3476505aef4e6da5d343a](https://explorer.harmony.one/#/block/0xdc6d31cc9e16ee9e121d784cc3aff3ec134c3f65fab3476505aef4e6da5d343a) 

The main trigger was our log-checking script unzipping the compressed log file which consumed most of the machine resources and caused leader node crash or failure to commit block into disk. Also, the flaw of the view change protocol can’t recover from the previous state that stuck.

The effect is that affected shards stop generating new blocks for hours until we successfully restart the shard with temporary fixes to unblock the consensus process.

Fortunately, all shards are operating normally now and no forks were created. No customer funds were affected nor double-spend happened.

## Customer Impact:

* FN nodes across all the four shards stopped earning rewards during the shards being down.
* Some FN nodes were alerted with the incident which caused trouble or confusion to them. Consequently, our reputation with our FN nodes to some extent was negatively affected.

## People Involved:

* Leo \(leo@harmony.one\)
* RJ \(rongjian@harmony.one\)
* Chao \(chao@harmony.one\)

## Timeline:

July 23:

10:05pm: nyet from FN nodes reported stuck nodes in shard 0

\(initial investigation by Leo, RJ, Chao, and we decided to restart leader in shard 0\)

We see log from validators that they keep getting invalid Announce message:

{"ip":"52.192.230.162","lvl":"dbug","mode":"Sycning","msg":"\[OnAnnounce\] Receive announce message","myBlock":270109,"myViewID":270108,"phase":"Announce","port":"9000","t":"2019-07-24T05:30:49.627730594Z"}

{"BlockNum":"270108","MsgBlockNum":270109,"ip":"52.192.230.162","lvl":"dbug","mode":"Sycning","msg":"\[OnAnnounce\] BlockNum not match","myBlock":270109,"myViewID":270108,"phase":"Announce","port":"9000","t":"2019-07-24T05:30:49.631456691Z"}

This implies that the leader is having problem in its local state

We thought restarting the leader will fix the leader’s state problem.

11:02pm: Leo restarted Leader

11:03pm:

Leo \| HarmonyYesterday at 11:03 PM

just restarted the harmony process

stuck in proposing

{"blockNum":270109,"ip":"3.112.219.248","lvl":"info","msg":"PROPOSING NEW BLOCK ------------------------------------------------","port":"9000","selectedTxs":0,"t":"2019-07-24T06:03:41.725145232Z"}

{"funcFile":"node\_newblock.go","funcLine":76,"funcName":"github.com/harmony-one/harmony/node.\(\*Node\).WaitForConsensusReadyv2.func1","ip":"3.112.219.248","lvl":"eror","msg":"Cannot got commit signatures from last block: SetLastCommitSig failed with wrong number of committed message","numCommittedMsg":0,"port":"9000","t":"2019-07-24T06:03:41.725283974Z"}

stuck in this message

This brought up another problem that the last commit signatures are not persisted in DB and once the leader is restarted, the last commit signatures are gone.

11:17pm:

\(After discussion via wechat call\)

Chao worked on adding a fix to repropose previous block so as to re-collect the “LastCommitSigs” again from validators \([https://github.com/harmony-one/harmony/pull/1238](https://github.com/harmony-one/harmony/pull/1238)\)

RJ worked on a PR to store lastCommitSigs in local disk \([https://github.com/harmony-one/harmony/pull/1235](https://github.com/harmony-one/harmony/pull/1235)\)

July 24:

1:51am: both Chao and RJ’s fix was merged.

1:58am: Leo shut down all Harmony nodes in shard 0 and updated them with the new binary with the fixes

2:06am: Leo restarted all Harmony nodes in shard 0 and the shard resumed producing new blocks.

2:29am: Leo reinited explorer node for shard 0 and explorer.harmony.one start showing blocks from shard 0 again.

7:00am-1:00pm: Shard 2, 1, 3 were down too and similar recovery process were used to successfully resume the shard

~2:01pm: During the investigation of shard 3 being down, Leo found that the unzipping and log checking script may potentially use up all the machine resources when the log grows big. Chao had reported disk usage up and down w/o human intervention that caused us doubt the issue with EBS volume. During the inspection of the leader of shard3, Leo run the “ps -ef” command and found some suspicious process running in the system while the disk usage of “df” command output varied. By checking the process carefully, we realized that the scripts that were used to scrap the log and reports status is the culprit of the out-of-disk space, and memory/cpu usage spikes, like shown in Grafana. The system resources were allocated to the log analysis script and the gzip process to unzip the files, which may used most of the empty disk space in temporary files, thus caused LevelDB write failure.

![](https://lh6.googleusercontent.com/zXLckxjd0EB5RcbVPLBPd6sBL5lUxH9x6VgdtMSsVjLRd8qANvw0XxXgLpoTDeqIBGo6XRX0Niz_KvXvgrHVg4dO5vIXg-9QXH88KrW5PyKFedF9FM8kPA_rA2HkJugC_TEluG6p)

ec2-user 16275 1 0 20:29 ? 00:00:00 bash -c { tac ../tmp\_log/\*/\*log; zcat ../tmp\_log/\*/\*log.gz \| tac; } \| grep Signers \| python -c $'import time, datetime, collections, sys, json; a = collection

ec2-user 16284 16275 0 20:29 ? 00:00:00 bash -c { tac ../tmp\_log/\*/\*log; zcat ../tmp\_log/\*/\*log.gz \| tac; } \| grep Signers \| python -c $'import time, datetime, collections, sys, json; a = collection

ec2-user 16288 16284 45 20:29 ? 00:01:20 gzip -cd ../tmp\_log/log-20190628.153354/validator-34.247.55.134-9000-2019-06-28T17-29-51.298.log.gz ../tmp\_log/log-20190628.153354/validator-34.247.55.134-9000-

## Questions to ask:

1. how long does it take to respond to the issue? how the problem was reported? how to cut it into half?
2. First incident: last HOORAY on shard0 is 4:50UTC \(9:50PMPDT\) and @nyet reported node stuck at 10:05PM. The 15 minutes cap could be shortened to half by adding a block rewards monitor to detect the block latency in each shard. If one shard didn’t generate block reward in ONE minute, the on-call engineer shall be paged immediately.
3. how long does it take to mitigate the issue? how to cut it into half?
4. It took around 4 hours from receiving the alert to fully restore the shard0. We did basic analysis and immediately discussed the mitigation solution. The patch has to be implemented, reviewed, and tested. The binary has to be deployed to all the nodes on shard0. As it is the first time we see this issue, 4 hour is pretty fast.
5. Is there any pending items that can help prevent this issue? If so, why it wasn't prioritized?
6. We have planned to, and started to work on metrics of the blockchain. However, the work wasn’t prioritized till now because we were focusing on the feature development for mainnet V1. We have uptime monitor to monitor the uptime of the harmony process, however, we didn’t detect the situation the blockchain hung and not generating block rewards.
7. can this issue being detected in a test environment, if not, why?
8. We could’ve run a canary testnet with faster blocktime so it will hit the issues related long-running program before the real mainnet does.

## Root Cause Analysis:

* The log monitoring script unzips all the logs in the node; when the log size grows big, the unzipping consumes all of the resources \(Disk, Mem, CPU\) which either crashed the leader program or failed the disk commit of new block.
* The crashed leader triggered view change on the shard, but the view change protocol did not succeed
* The failure of committing new block into disk made block number in consensus object inconsistent with the current block number in disk, causing invalid Announce message sent from leader. The announce message have mismatching block number in block and in message which is not recognized by validators.

## Action Items:

| Category | Description | Owner | ETA |
| :--- | :--- | :--- | :--- |
| Preventive | Fix issue that nodes stuck in sync mode. | Chao | Done |
| Preventive | Clean up log files on disk regularly after download/upload to S3 | Leo | Done |
| Preventive | Don’t exit the program if not fatal errors | MD |  |
| Detective | Monitor which node is leader | Leo/Andy | Done |
| Detective | Detect the “gap of block rewards”, and paging | Chris | Done |
| Detective | Monitor block rewards for ⅔ of nodes | Chris | Done |
| Corrective | Re-propose previous blocks | Chao | Done |
| Corrective | Disable log checking script | Stephen | Done |
| Corrective | Make log checking script efficient | Chris | Done |
| Corrective | Persist last commit signatures in disk | RJ | Done |
| Corrective | Consensus message-level retry | RJ | Done |

