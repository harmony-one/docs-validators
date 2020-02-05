---
description: post-mortem for shard0 down accident on Nov/6/2019
---

# Post-Mortem Nov/6 shard 0 down

## Summary

On Nov 6, 2019 around 10:40 AM PDT, some FN node runner \(e.g. nyet \| Blockdaemon\) informed us that his node was offline in shard 0. Andy acknowledged the message around that time, and started to troubleshoot the issue.

From some preliminary troubleshooting, shard 0 was offline while other shards were up and online. This was confirmed on the harmony /1h and explorer webpages. The last block successfully mined was around 10:28 am on that day. After that, we can not find any newer block or leader for the shard.

## Customer Impact

All Foundational Node runners in shard 0 got impacted due to the shards stopped generating new blocks and stopped in consensus. The incident lasted for around 1 hour and 40 mins.

## People Involved

* Andy
* RJ

## Timeline

* 2019-11-6         ~10:40 am PDT Andy acknowledged the shard offline msg
* 2019-11-6         ~ 11:00 am PDT Andy restarted shard 0, but shard 0 didn’t get recovered
* 2019-11-6         ~ 11:30 am PDT Two corrective actions: 1. The list of ip files for shard 0 was not up to date due to node migration on devops instance 2. The parameter for the cmd to restart the whole shard was not properly set, it restarted 5 instances instead of 50 per one call.
* 2019-11-6         ~12:10 am PDT shard 0 online

## Questions to ask

**1. How was the problem reported? How long did it take to respond to the issue? How could we cut that time in half?**

Our FN node runner reported this incident on Discord. Andy recognized the issue around the same time. But this might not be ideal if we could not identify the network status in real-time using monitoring tool. The uptime always have a delay to report offline nodes, it might be better to improve our own watchdog such that it can trigger real-time alerts once a shard lost its consensus.

**2. How long did it take to mitigate the issue? How could we cut that time in half?**

It took us about 1 hour and 40 mins to resolve the issue. We could bring a shard online quicker if we recognized that we do need to restart every single node about the same time. This instruction has been updated in the DevOps runbook.

**3. Are there any pending items that can help prevent this issue? If so, why wasn't it prioritized?**

The watchdog might be the critical tool we can use to monitor the consensus status of each shard in real-time. I don’t think we have enough resources to further improve this tool due to the focus on staking design and implementation.

**4. Can this issue be detected in a test environment? If not, why?**

This issue does not happen every time for epoch change on the mainnet, which means it would be difficult to reproduce the issue consistently in the testnet.

## Whys?

1. **Why was the view change failed?**

From the log, the most reasonable cause is when the potential new leader, let’s say v1. V1 will check whether the incoming received view change message’s viewID \(msg.viewID\) is greater than or equal to its own view changing ID \(own.viewID\). Thus there is a possibility that different validators start view change with different viewID. If v1 doesn’t check whether viewID from different validators are the same and just aggregates the signature as the current code do, then the aggregated signature will not pass verification. To solve the issue, we can add a dictionary with key is viewID \(uint64\) and value will be the aggregated signatures and bitmap. In this case, whenever one of the viewID \(the majority of viewchanging id in the network\) has enough M3 type signature \(i.e the number of the signature on the same viewID exceeds ⅔ of the network\), it can propose new view message and delete all the cached dictionary.

1. **Why did the monitoring not reporting the consensus failure in time?**

From my first-hand experience with uptime, there is some delay \(from half an hour to a few hours\) with uptime alerts and pagerduty notifications. The uptime and pagerduty can be triggered once a node went offline, but it won’t differentiate an offline node and an offline shard.

The consensus alert from watchdog should be improved such that we can receive a real-time notification once the shard lost its consensus.

## Root Cause Analysis

The incident happened right at the block of epoch transition. From the analysis of logs of multiple instances including the previous leader, the new leader and normal validators during the epoch transition, the incident is a complication of multiple issues coming together but the main root cause of the failure is the malfunctioning of the view change protocol at epoch transition.

Specifically, the following issues were found:

### Major:

* [New leader at index 0 doesn't propose new blocks](https://github.com/harmony-one/harmony/issues/1809)
* [Validators failed to verify NewView message when view change happened](https://github.com/harmony-one/harmony/issues/1808)

### Minor:

* [The old leader didn't stop proposing blocks after it's not leader anymore](https://github.com/harmony-one/harmony/issues/1807)
* [Bingo didn't show when a non-leader becomes leader after epoch change](https://github.com/harmony-one/harmony/issues/1810)

Before the epoch transition, there was one view change happening which made the node at index 1 the leader. During the epoch transition, all of the nodes reset their leader back to the node in index 0. However, the new leader didn’t propose new block after the transition, which is an existing bug. The bug was not manifested because previously the view change successfully kicked in at every epoch transition. Unfortunately, this time the view change failed too.

From the log, the most reasonable cause is when the potential new leader, let’s say v1. V1 will check whether the incoming received view change message’s viewID \(msg.viewID\) is greater than or equal to its own view changing ID \(own.viewID\). Thus there is a possibility that different validators start view change with different viewID. If v1 doesn’t check whether viewID from different validators are the same and just aggregates the signature as the current code do, then the aggregated signature will not pass verification. To solve the issue, we can add a dictionary with key is viewID \(uint64\) and value will be the aggregated signatures and bitmap. In this case, whenever one of the viewID \(the majority of viewchanging id in the network\) has enough M3 type signature \(i.e the number of signature on the same viewID exceeds ⅔ of the network\), it can propose new view message and delete all the cached dictionary.

At the same time, we found two minor issues with the node. One is that the old leader didn’t stop proposing blocks after the leader is reset to the node at index 0. Another is the new leader didn’t print out Bingo at the block when it was the validator and just became the leader. These are minor issues not directly contributing to the incident.

## Action Items

| Category | Description | Owner | Tracker |
| :--- | :--- | :--- | :--- |
| Preventive | Uptime robot not very responsive | Andy | Todo |
| Preventive | [Measure elapsed time using current time instead of latest block header time](https://github.com/harmony-one/harmony-ops/pull/248) | Janet | Done |
| Corrective | [Bingo didn't show when a non-leader becomes leader after epoch change](https://github.com/harmony-one/harmony/issues/1810) | RJ | Done |
| Corrective | [The old leader didn't stop proposing blocks after it's not leader anymore](https://github.com/harmony-one/harmony/issues/1807) | RJ | Done |
| Corrective | [New leader at index 0 doesn't propose new blocks](https://github.com/harmony-one/harmony/issues/1809) | RJ | Done |
| Corrective | [Validators failed to verify NewView message when view change happened](https://github.com/harmony-one/harmony/issues/1808) | Chao | Issue found |

