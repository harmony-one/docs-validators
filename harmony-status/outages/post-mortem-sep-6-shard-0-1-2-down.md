# Post-Mortem Sep/6 shard 0,1,2 down

## Summary

On Sep. 6 shard 0, 1, 2 on Harmony mainnet were down from 9:10am \(PDT\). No blocks can be generated nor consensus was reached. We had to recover the three shards by restarting the Harmony validator nodes on those shards. The root cause of the accident was due to the fact that we had forgotten to update the node.sh / harmony node binary on our terraform-launched nodes in all shards during the last rolling update on Sep. 3rd. The epoch transition happened on Sep. 6 morning, thus all the terraform-launched nodes were not able to join in the consensus due to their outdated committee member table. The shards had not enough valid validators to verify the consensus. Thus, the shards were down. The three shards were recovered at 1:51pm \(PDT\) after we manually updated the node.sh/binary on all terraform-launched nodes and restarted the node software on all Harmony nodes.

## Customer Impact

All foundational node runners \(~240\) on shard 0, 1, 2 weren’t able to join consensus and earn block rewards for 6 hours.

## People Involved

* Leo Chen \(leo@harmony.one\)
* Chris Liu \(chris@harmony.one\)

## Timeline

9:10am 9/6 2019 PDT: @Harmonaut on telegram foundational node channel reports all three shards 0,1,2 are down. Investigation started by Leo Chen

11:44am 9/6 2019 PDT: restarted shard 0, and shard 0 seems recovered, got one BINGO

11:44am 9/6 2019 PDT: restarted all three shards, shards seemed recovered with one BINGO.

12:12PM 9/6 2019 PDT: found the root cause of the shard down.

1:51PM 9/6 2019 PDT: all three shards are recovered and started to generate block rewards.

## Root Cause Analysis

Harmony’s consensus algorithm needs 2/3 + 1 validators in order to reach consensus. Our current design of epoch transition happens around every 36 hours. In the beginning of each epoch, the validator committee may be changed, with a new set of BLS pubkeys. All the validators in the same shard have to share the same set of BLS pubkeys in order to join in the consensus. Right now, the new set of pubkeys are hardcoded into the harmony node program and can only be updated using the rolling update process. When we had the rolling update on Sep. 3rd, there is an epoch transition scheduled to be around Sep. 6th morning. However, the operation engineer forgot to update around 60 nodes in each of the 4 shards, as these 60 nodes were launched using new terraform scripts and not fully compatible with the original rolling update process. So, after the epoch change around Sep. 6th morning, ~60 nodes weren’t able to join the consensus, also some foundation nodes were also outdated, there were in total more than ⅓ of nodes in each shard couldn’t join the consensus. Thus, shard 0, 1, and 2 were down. Shard 3 was still up due to the epoch change on shard 3 didn’t happen on Sep. 6th. Shard 3 lagged behind on epoch change because it was previously down for over 20 hours in another incident.

## Questions to ask

1. How was the problem reported? How long did it take to respond to the issue? How could we cut that time in half?

The problem was reported by foundational node runner on telegram channel. The team immediately responded on the report to look into the issue. However, the report time can be cut if the pager we setup on sentry nodes sends paging to on-call engineers. Then we don’t have to wait for the FN node runners reporting on the issue.

1. How long did it take to mitigate the issue? How could we cut that time in half?

It took around 6 hours to mitigate the issue. The time can be cut if we can find the root cause faster. The node.sh script wasn’t updated on all our terraform nodes. We currently have no monitors of node version on all Harmony nodes. If we have the harmony version monitors, we can actually prevent this accident from happening.

1. Are there any pending items that can help prevent this issue? If so, why wasn't it prioritized?

We don’t have pending items that can prevent this issue. However, we did have an action item to monitor the consensus/block status on our sentry nodes and setup pager. Those paging setup wasn’t fully tested and we need to revisit it. If we had the monitor working, we can respond to the issue earlier and faster.

1. Can this issue be detected in a test environment? If not, why?

This issue is not a software bug, purely operational errors. Thus, it won’t be detected in a test environment.

## 5 Whys?

1. Why the shards are down? Why wasn’t shard 3 down?

Shard 0,1,2 each have around 60 terraform nodes. They were not updated to the latest node.sh and harmony node software. When the epoch changed that triggered committee member change, none of those 60 nodes could join in the consensus anymore as they have an old set of validator committee BLS keys. Since we released the node.sh and harmony node software update on Sep 3rd, a lot of foundation node runners are also offline due to their nodes running older software as well. In this case, in all the three shards, more than ⅓ of nodes \(83\) were offline, this is like an attack to the shards. After the epoch change, not enough validators were online, the shard couldn’t reach consensus anymore. And the existing nodes were stuck in leader change loop. Shard 3 was still up because shard3 was lagging behind in the epoch change. Epoch change was based on the number of blocks. Shard 3 was previously down for 20+ hours, that’s why it takes more time for shard 3 to catch up the number of blocks to trigger epoch change. So, in this case, shard 3 was still up using the older epoch.

1. Why the validators running old harmony node software?

We did a rolling update on Sep 3rd. The operation engineer forgot to update the node.sh and harmony software on the terraform node as they are using a different process than our legacy node.

1. Why didn’t the foundation node runners update their node software?

Foundation node runners need to update the node.sh and restart the node.sh script in order to pick up the latest software version. Since we had a shard down event on Aug 30th and the rolling update on Sep 3rd, foundation node runners weren’t left enough time to update their node before the epoch changed. We have less trust from foundation node runners due the recent frequent incidents. Also, we didn’t actively engage all the foundation node runners to update their node.sh and harmony node binary on time. Or we haven’t educated the node runners on how to keep their node up to date.

1. Why didn’t we detect the older version of the node software?

We have no checking built in to the rolling update script to make sure the software is really updated. If the software isn’t updated, the rolling update script may still continue. Also, we have no monitor of the software version running on the network, even though we have added the information in the ping message logs.

## Action Items

| Category | Description | Owner | Tracker |
| :--- | :--- | :--- | :--- |
| Preventive | Monitor the harmony node versions on all Harmony nodes. Report different version than the expected deployed one. | Leo | [\#1315](https://github.com/harmony-one/harmony/issues/1315) |
| Preventive | Check the software version after rolling update to make sure nodes are running the right version. | Eugene | [\#1596](https://github.com/harmony-one/harmony/issues/1596) |
| Preventive | Actively engage foundation node runners to update their node software after rolling update. Reach 90%+ of node online status for FN runners. | Li | [Task](https://github.com/harmony-one/harmony/projects/16#card-26429962) |
| Preventive | Publish node runner engagement agreement to keep certain SLA on node update and monitoring. | Sahil | [Task](https://github.com/harmony-one/harmony/projects/16#card-26448749) |
| Preventive | Release process be more transparent with enough heads-up to FN runners | Leo | [Release Status](https://nodes.harmony.one/harmony-status/release) |
| Detective | Revisit all the sentry nodes on the paging setup for consensus monitoring | Eugene | [\#1595](https://github.com/harmony-one/harmony/issues/1595) |
| Corrective | Update node software on all terraform nodes | Chris | Done |

