---
description: post-mortem for shard2 down accident on Nov/21/2019
---

# Post-Mortem Nov/21 shard 2 down

## Summary

On Nov 21st early morning around 3:25am PDT, foundational node runners reported shard 2 was down. Shard 2 network stopped to continue the consensus during the Epoch change from epoch 69 to epoch 70. The shard was recovered on Nov/21 around 10am after Andy restarted all the Harmony nodes on shard2.

## Customer Impact

All Foundational Node runners in shard 2 got impacted due to the shards stopped generating new blocks and stopped in consensus. The incident lasted for around 7 hours.

## People Involved

* Minh
* Andy
* Leo

## Timeline

* 2019-11-21     2:45 am PDT          last block of shard2, last block before epoch change \([https://explorer.harmony.one/\#/block/0x190c9c1e1e63ee083dbf048e6b3c7c26d14c49e51297c1105e0fbb5e60463cfe](https://explorer.harmony.one/#/block/0x190c9c1e1e63ee083dbf048e6b3c7c26d14c49e51297c1105e0fbb5e60463cfe)\)
* 2019-11-21     8:22 am PDT          Andy tried to restart all harmony nodes on shard2, it failed to resume the shard.
* 2019-11-21     10:06am PDT         Andy restarted shard 2, and shard 2 is recovered \([https://explorer.harmony.one/\#/block/0xe3c542bb4a412112b784506a7aa7aec532cb720dd07b4ed4707ecd4a7701e208](https://explorer.harmony.one/#/block/0xe3c542bb4a412112b784506a7aa7aec532cb720dd07b4ed4707ecd4a7701e208)\)

## Questions to ask

**1. How was the problem reported? How long did it take to respond to the issue? How could we cut that time in half?**

Our FN node runner reported this incident on Discord on 3:25am PDT Nov/21. Minh had recognized the accident around 5am.

**2. How long did it take to mitigate the issue? How could we cut that time in half?**

It took us about 7 hours to resolve the issue as the accident happened in the night and our engineers weren’t able to respond. We could bring the shard online quicker if we recognized that and get it recovered the first time in the morning. We do need to restart every single node at about the same time. This instruction has been updated in the devops runbook. The recovery time can be shortened if we have developed a system to auto-restart the shard if the shard down event is detected. But that may be taking too much effort.

**3. Are there any pending items that can help prevent this issue? If so, why wasn't it prioritized?**

We have experienced a similar shard offline issue on Nov. 6. Last time, shard 0 went offline for about 2 hours due to the same issue. This happened when half of our engineering team was doing a roadshow in India. We recognized the issue, did a root cause analysis, and had a fix ready for this issue, but we didn’t deploy the fixes to the mainnet as we thought the epoch changing failure wasn’t a common case and also the engineers were busily working on the ePoS development. Now, we have planned the release date on Nov 26, 2019.

If we did spend more resources on the real time shard consensus status monitoring using watchdog, we would shorten the time to recover this shard significantly.

**4. Can this issue be detected in a test environment? If not, why?**

This issue does not happen every time for epoch change on the mainnet, which means it would be difficult to reproduce the issue consistently in the testnet.

## Whys?

1. **Why was the view change failed?**

From the log, the most reasonable cause is when the potential new leader, let’s say v1. V1 will check whether the incoming received view change message’s viewID \(msg.viewID\) is greater than or equal to its own view changing ID \(own.viewID\). Thus there is a possibility that different validators start view change with different viewID. If v1 doesn’t check whether viewID from different validators are the same and just aggregates the signature as the current code do, then the aggregated signature will not pass verification. To solve the issue, we can add a dictionary with key is viewID \(uint64\) and value will be the aggregated signatures and bitmap. In this case, whenever one of the viewID \(the majority of viewchanging id in the network\) has enough M3 type signature \(i.e the number of the signature on the same viewID exceeds ⅔ of the network\), it can propose new view message and delete all the cached dictionary.

1. **Why did the monitoring not reporting the consensus failure in time?**

From my first hand experience with uptime, there is some delay \(from half an hour to a few hours\) with uptime alerts and pagerduty notifications. The uptime and pagerduty can be triggered once a node went offline, but it won’t differentiate an offline node and an offline shard.

The consensus alert from watchdog should be improved such that we can receive a real time notification once the shard lost its consensus.

1. **Why we won’t be able to recover the shard in the first attempt at restarting all the Harmony nodes?**

Andy restarted the offline shard around 8:40 AM, but it didn’t bring the network online. After some troubleshooting, he noticed that the script to generate a list of IP files for each shard has some problems. Andy did two batches of node migration on shard 2 and shard 3 the night before. The IP of newly launched nodes were not included in the shard2.txt file such that the total number of nodes restarted is less than the required number of nodes to reach network consensus.

1. **Why the shard\*.txt files are not updated?**

There is node migration project going on, which means the list of ip for each shard is keep changing everyday. This is a manual process for now, and it should be an automated process.

## Root Cause Analysis

The root cause of this accident is the same as the accident on Nov/6. Please refer to [Post-Mortem on Nov/6](post-mortem-nov-6-shard-0-down.md) for further details.

## Action Items

| Category | Description | Owner | Tracker |
| :--- | :--- | :--- | :--- |
| Corrective | Accept and verify multiple viewID in view change messages | Chao | [Done](https://github.com/harmony-one/harmony/pull/1856) |
| Detective | Fix watchdog alarm issue | Janet | [Done](https://github.com/harmony-one/harmony-ops/pull/248) |
| Preventive | Keep the shard IP address file up to date | Andy | [Issue](https://github.com/harmony-one/harmony-ops/issues/250) |
| Preventive | Automate the process of updating nodedb | Andy | [Issue](https://github.com/harmony-one/harmony-ops/issues/251) |

