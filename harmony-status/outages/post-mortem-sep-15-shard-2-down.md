# Post-Mortem Sep/15 shard 2 down

## Summary

On Sep 15, 2019 around 8:30 PM PDT, John reported that shard 2 in the mainnet lost consensus. Shortly after that, we started to identify the root cause of the issue. The first symptom we noticed is that the version deployed on that shard is not consistent: some of the nodes have earlier versions. The second symptom is that we were not able to deploy the latest stable version on that shard. After ssh to the node, we noticed that a few nodes had 20 KB of free space left.

Some further investigations revealed that the free storage space of many other nodes in shard 2 were approaching 0. The same issue was happening to other shards too. Then we increased the storage space of all legacy nodes on each shard from 50 GB to 100 GB. The block data sync was finished around 12:30 AM, and finally we recovered shard 2 around 1:00 AM.

## Customer Impact

All Foundational Node runners in shard 2 got impacted due to the shards stopped generating new blocks and stopped in consensus. The incident lasts for around 5 hours.

## People Involved

* Leo \(leo@harmony.one\)
* Andy \(andy@harmony.one\)
* RJ \(rongjian@harmony.one\)
* John \(john@harmony.one\)

## Timeline

* 2019-09-15   Sunday Morning Andy recovered a few offline nodes due to the storage issue
* 2019-09-15         ~16:00 PDT Andy finished a python script to upgrade EBS volume upgrade for legacy nodes
* 2019-09-15         ~17:30 PDT Andy applied \(the first part of\) the EBS upgrade actions to the mainnet
* 2019-09-15         ~20:10 PDT John reported the issue on the discord channel 
* 2019-09-15     ~20:12 PDT Leo and Andy acknowledged the issue
* 2019-09-15         ~21:00 PDT Identified the block heights are different for some nodes
* 2019-09-15         ~21:30 PDT Identified the root cause of the issue
* 2019-09-15         ~21:30 PDT Leo and Andy applied the 2nd part of EBS volume extension to every single legacy node
* 2019-09-15          ~22:00 PDT Leo started to work on the blockchain DB sync part
* 2019-09-16     ~01:10 PDT shard 2 has been recovered

## Questions to ask

1. How was the problem reported? How long did it take to respond to the issue? How could we cut that time in half?

John shared the message on \#team-devops channel, and tagged Leo and Andy. Andy and Leo took the actions to resolve the issue immediately.

The improvement we’d like to propose is to set up a system to monitor the consensus status in each shard, in that case, if any shard lost consensus status, we will get notified immediately.

1. How long did it take to mitigate the issue? How could we cut that time in half?

We recovered shard 2 around 1:10 AM PDT on Monday. The shard went offline around 5 hours. One solution, which Leo is working on right now, is to push the latest blockchain DB data to S3, and then download the data from the offline nodes directly.

1. Are there any pending items that can help prevent this issue? If so, why wasn't it prioritized?

We have Grafana dashboards to monitor free disk space of our nodes, but it went offline last Friday. We had limited bandwidth, and manpower to bring it online on time. Also, there is no alert set yet to that chart. [Ticket](https://github.com/harmony-one/harmony-ops/issues/121) created.

we brought the Grafana dashboards [online](https://github.com/harmony-one/harmony-ops/issues/106) on Sep 18, 2019.

1. Can this issue be detected in a test environment? If not, why?

No. This issue can not be detected in our test environment. We launch the testnet with a fresh environment every time. It takes a while to consume all the hard disk space \(30GB for testnet\) before the new test environment gets created.

## 5 Whys?

1. Why the shards are down?

Lots of our nodes went offline because no enough free storage space was available for the nodes to store generated blocks. Then ⅔ consensus is lost as more nodes were not eligible for voting.

1. Why the node run out of space?

The rate of blockchain data is growthing fast, and initially, we recommended 30 GB of storage for our node runners.

1. Why didn’t we detect the issue?

We do have a chart to monitor the available storage of each node, but we didn’t set an alert to actively monitoring this metric. This issue is not significant for other three shards.

1. Why didn’t we upgrade the space earlier?

We didn’t catch this issue until Sunday morning when I was doing on call to recover a few offline nodes due to its low storage issue.

## Root Cause Analysis

1. Need to change our strategy from passionate monitoring to active monitoring
2. All monitoring tools requires certain amount of effort to maintain and improve on a daily/weekly basis

## Action Items

| Category | Description | Owner | Tracker |
| :--- | :--- | :--- | :--- |
| Preventive | [Restore Prometheus and Grafana service](https://github.com/harmony-one/harmony-ops/issues/106) | Andy | DONE |
| Preventive | [Set up storage space alert](https://github.com/harmony-one/harmony-ops/issues/121) | Andy | DONE |
| Preventive | [Add functionality to enable Grafana monitoring TF nodes](https://github.com/harmony-one/harmony-ops/issues/85) | Andy | DONE |
| Corrective | [Python script to extend Terraform nodes volume to 100 GB](https://github.com/harmony-one/harmony-ops/issues/110) | Andy | DONE |
| Corrective | [Python script to extend legacy nodes volume to 100 GB](https://github.com/harmony-one/harmony-ops/issues/107) | Andy | DONE |

