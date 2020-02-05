# Post-Mortem Aug/30 shard 0,1,2,3 down

## Summary

On August 30th night around 10:33pm, Leo had accidentally disabled the public access of the release bucket on s3. All of Harmony t3 nodes on mainnet stopped working due to the node.sh auto-updated harmony binaries using invalid xml files downloaded from the s3 bucket. The majority of the foundational nodes stopped working as well because of the same auto-updated invalid binaries, which had caused all 4 shards can’t reach consensus due to a lack of validators. The shards 0,2,3 were recovered at around 11:55pm after a restart of the entire network. The shard 1 was recovered at around 1:05am after a few restarts.

## Customer Impact

All Foundational Node runners got impacted due to all the shards stopped generating new blocks and stopped in consensus.

## People Involved

* Leo \(leo@harmony.one\)
* Chris \(chris@harmony.one\)
* Andy \(andy@harmony.one\)

## Timeline

* 2019-08-31 05:31:24 UTC Leo updated public access policy on pub.harmony.one bucket. Public bucket access was disabled on pub.harmony.one bucket.
* 2019-08-31 05:33 UTC Chris received the first paging from [pagerduty alert](https://harmonyone.pagerduty.com/alerts/PZVTYET). The harmony process stopped on one t3 node.
* 2019-08-31 05:43 UTC Leo updated the access policy on pub.harmony.one bucket to restore public access.
* 2019-08-31 05:53 UTC Foundational node runner \(@insider\) reported his node had stopped generating BINGO.
* 2019-08-31 05:54 UTC Harmony process on all the t3 nodes went offline.
* 2019-08-31 05:53 - 06:20 UTC More FN runners \(@Gaia, @Eddie, @nyet, @3ni6m4, @spidey6600, @Cutluck\) reported their nodes are offline.
* 2019-08-31 05:45 - 06:55 UTC Leo and Chris restarted all Harmony nodes once \(shard3 at first\), to restore the shard. Shard 3, 0, 2 were up and generating consensus on blocks again.
* 2019-08-31 05:45 - 07:00 UTC Shard 1 was restarted twice and the validator logs show nodes were doing a different round of view changes and didn’t consolidate on the view change. It was stuck.
* 2019-08-31 07:05 UTC Shard1 was restarted again and it reached consensus.

## Questions to ask

1. How was the problem reported? How long did it take to respond to the issue? How could we cut that time in half?
2. Initially, we received a few alerts from PagerDuty about some nodes that went offline around 10:33 on Friday, Aug 30, 2019, PDT. Then the pagerduty reported all the t3 nodes went offline due to the harmony process was stopped. Chris was on-call and he immediately responded to the event. Leo investigated the instances and found the instances were still up but the harmony processes were stopped. Don’t think we can cut the response time in this regard.
3. How long did it take to mitigate the issue? How could we cut that time in half?
4. The issue was resolved on shard 0,2,3 within 1 hour 25 minutes. Shard 1 was recovered within 2 hours 35 minutes. The time can be cut into half if we immediately restart all the nodes after we identified the shard down event.
5. Are there any pending items that can help prevent this issue? If so, why wasn't it prioritized?
6. There are no detailed tasks pending to prevent this issue. We had previously discussed the risk of the auto-update feature in the node.sh, but we haven’t had action items defined.
7. Can this issue be detected in a test environment? If not, why?
8. The issue may be detected in a test environment if we are using a testing environment similar to the production environment using node.sh. However, our current test environment is using a different mechanism to launch the harmony process, which is not able to detect the issue.

## 5 Whys?

1. Why the shards are down? More than ⅓ of validator processes stopped, thus the shard can’t reach consensus nor change leaders.
2. Why the validator processes are stopped? The validators are running node.sh. Node.sh downloads harmony binaries every 5 minutes and restart to run the new harmony file if it is different from the existing one. When the accident happened, the downloaded harmony file is an invalid xml file instead of a valid executable binary.
3. Why the downloaded harmony file is an invalid xml file? We have accidentally disabled the public bucket access. Thus when the node.sh calls the “curl” command to download the binary, it creates a xml file saying invalid key. This is the function of the Amazon s3 service. After node.sh creates the xml file, it didn’t verify the harmony binary. Instead, it blindly executes the xml file which is not executable. Thus the validator process stopped.
4. Why the node.sh blindly executes the xml file? In node.sh, we didn’t verify the downloaded binaries. Thus it is not able to distinguish the xml file from a valid executable binary. It assumes all files downloaded from the s3 bucket are valid.
5. Why the access to the public bucket was disabled? We have received a warning email from Amazon web services asking us to examine the access permission of several public buckets. We have disabled public access to a few buckets which had resulted in Jenkins's job failure. During the repair process, we accidentally disabled the public access policy on the pub.harmony.one bucket which should have public read access for validators to download binaries. 

## Root Cause Analysis

The trigger of the accident was an email from AWS warning us about the public access of several buckets. We were trying to limit the access of the public buckets. In this process, the Jenkins job failed as it relies on the public access of one bucket to upload/download harmony binaries. We were trying to repair the failure of the Jenkins job. In the meantime, we accidentally disabled the public access to the pub.harmony.one bucket, which should have public read access. Our node.sh script periodically download binaries from the pub.harmony.one bucket and auto-update the validator process if new binaries are downloaded. Since the read access is revoked, the curl command we used in node.sh actually downloaded an XML file from s3 bucket claiming missing key of the binary. The node.sh script wrongly assumed the generated XML file is a valid executable binary. Thus, node.sh killed the existing running validator process and tried to execute the XML file, which failed to execute. All our t3.small based nodes are using the node.sh and the auto-update mechanism, thus all of them failed. Most of our foundational node runners are using the node.sh and the auto-update also failed their validator processes. In this case, the shard has not enough validators to reach consensus. The shards were stopped.

## Action Items

| Category | Description | Owner | Tracker |
| :--- | :--- | :--- | :--- |
| Preventive | Verify the downloaded binaries | Eugene | Done |
| Preventive | Fail the curl command if the access to s3 bucket is disabled | Eugene | Done |
| Detective | Stagger the update of the binary in harmony t3 nodes | Leo | [102](https://github.com/harmony-one/experiment-deploy/issues/102) |
| Corrective | Don’t do rolling update or major operation change at night | Leo | Process |
| Corrective | Restore the public read access to the pub.harmony.one repo | Leo | Done |
| Corrective | Restart the shard to restore the consensus | Chris | Done |

