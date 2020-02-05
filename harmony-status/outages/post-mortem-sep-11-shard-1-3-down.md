# Post-Mortem Sep/11 shard 1,3 down

## Summary

On September 11th midnight around 1:10am PST, Andy and Chris initiated the planned rolling upgrade of the mainnet with features including block data structure update, cross-shard transactions, EIP155 signing update and Ethereum history fork rollup. After the upgrade, shard 1 and shard 3 is not reaching consensus because of some nodes failing to do state sync. After the state sync issue is fixed, we saw “BAD BLOCK” error indicating the existence of cross-shard txn in the new blocks, which is not recognizable by the code until the next epoch happens which will be around 36 hours later. Without the upgrade, these kind of cross-shard txns will be treated as single-shard txn without problem. But with the upgrade and before the actual cross-shard forking epoch happens, these transactions will be treated invalid. We change the code to add the logic to still accept these transactions as single shard transactions even with the upgraded code with cross-shard transaction logic until the actual fork epoch happens. Quickly after that, the shard 1 and 3 were recovered at around 3:30pm PST.

## Customer Impact

All Foundational Node runners in shard 1 and 3 got impacted due to the shards stopped generating new blocks and stopped in consensus. The accident last for around 14 hours.

## People Involved

* Leo \(leo@harmony.one\)
* Chris \(chris@harmony.one\)
* Andy \(andy@harmony.one\)
* RJ \(rongjian@harmony.one\)
* Eugene \(ek@harmony.one\)

## Timeline

* 2019-09-11 08:10 UTC Rolling upgrade started by Andy and Chris
* 2019-09-11 08:52 UTC Leo reported the shard 1 and 3 down
* 2019-09-11 10:32 UTC Initial debugging from Leo shows “unknown ancestor” issue which is a symptom of state sync failure.
* {"level":"warn","port":"9000","ip":"13.229.111.253","error":"unknown ancestor","inChain":"784565","MsgBlockNum":"784915","caller":"/home/ec2-user/harmony/consensus/consensus\_v2.go:211","time":"2019-09-11T10:09:52.898673587Z","message":"\[OnAnnounce\] Block content is not verified successfully"}
* 2019-09-11 21:00 UTC Leo fixed the DNS issues related to the state sync failure.
* 2019-09-11 21:36 UTC Leo reported failure of block validation.
* {"level":"error","port":"9000","ip":"13.231.130.45","caller":"/home/ec2-user/harmony/core/blockchain.go:1662","time":"2019-09-11T21:08:19.257525529Z","message":"\n\#\#\#\#\#\#\#\#\#\# BAD BLOCK \#\#\#\#\#\#\#\#\#\nChain config: {ChainID: 1 EIP155: 28 CrossTx: 28 CrossLink: 10000000}\n\nNumber: 774847\nHash: 0xe42f814b0652c51e5f9af9844d3aa0120af4728ca72aedcd134c60e4e3e42361\n\n\nError: cannot handle cross-shard transaction until after epoch 28 \(now 27\)\n\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\n"}
* 2019-09-11 21:36 UTC RJ fixed the block validation issue by adding backward compatibility logic for cross-shard transactions before the actual forking epoch. [https://github.com/harmony-one/harmony/pull/1572](https://github.com/harmony-one/harmony/pull/1572)
* 2019-09-11 22:30 UTC Shard1 and Shard3 were restarted by Eugene and Leo successfully after the fix.

## Questions to ask

1. How was the problem reported? How long did it take to respond to the issue? How could we cut that time in half?
2. Problem reported immediately after the rolling upgrade caused the shards to stall.
3. How long did it take to mitigate the issue? How could we cut that time in half?
4. The shards were restored after around 13-14 hours of the incident happening. We could’ve reverted the node’s client software to immediately rollback the upgrade and restore the shards. On the other hand, we could have scheduled the rolling upgrade earlier in the daytime instead of at midnight and thus we can have more time and man power to resolve the issue in time.
5. Are there any pending items that can help prevent this issue? If so, why wasn't it prioritized?
6. Nothing is pending for the rolling upgrade that may prevent this issue. This issue is due to a corner case scenario that we missed in the code logic. Potential mitigation is to have a rolling upgrade design doc listing all the potential corner cases and carefully review it before the upgrade.
7. Can this issue be detected in a test environment? If not, why?
8. Yes, it can, but it’s a rare scenario which can only happen if cross-shard transactions are issues during the rolling upgrade, which we didn’t consider beforehand.

## 5 Whys?

1. Why the shards are down? Less than ⅔ of the nodes are replying to consensus message, due to failure in state sync’ing and software backward incompatability issue for cross-shard transactions.
2. Why the rolling upgrade is initiated during midnight?

Because the actual forking epoch will happen later the week and if we don’t do it during the midnight to catch the latest next epoch change, the actual enabling of the cross-shard transaction logic will happen during weekend which is not good for maintenance and potential issue fixing.

1. Why the DNS issue found in the betanet upgrade happened again the mainnet upgrade?

We don’t have a release manager keeping track of all the issues during the release process and this DNS issue is an example of a missed problem that we should take care of during mainnet upgrade. Besides, we didn’t do a code change auditing between the old and new version of the code, which could have surface the problem more clearly.

1. Why the nodes had “BAD BLOCK” error?

Previously our code did not support cross-shard transaction logic and didn’t take use of the transaction field “ToShardID”. With the new code upgrade, the ToShardID is used for cross-shard txn logic which is only enabled at a certain epoch few days after the scheduled upgrade. Somehow, before the actual enabling of the cross-shard logic, some cross-shard txns were sent to the network during or after the rolling upgrade. However, the new code treated the cross-shard txn as invalid while actually it should be considered as single-shard transaction just as the previous mainnet logic.

1. Why the nodes failed to do state sync’ing?

We accidentally did regression on the state sync configuration by changing the state sync threshold from 10% back to 66%, which is a higher requirement for successful sync. The DNS servers are configured in a way that includes shard nodes that’s neighbors who will be upgraded together in batch in the rolling upgrade. The rolling upgrade restarted the majority of the DNS nodes in batch which caused other nodes failing to reach the threshold of state sync’ing, thus failing to sync.

## Root Cause Analysis

1. State sync’ing consensus threshold was changed to 10% only in branch s3 and deployed to mainnet in the last upgrade. However, the master branch still have the old number of 66%. This current rolling upgrade directly uses master branch with a [merge](https://github.com/harmony-one/harmony/pull/1563) from s3 branch that didn’t surface the threshold change. This caused the mainnet rolling upgrade to be unsuccessful with the last few batch of nodes failing to do state sync’ing due to lack of active nodes behind the DNS server. This further caused the shards down due to not enough nodes reaching consensus.
2. In the meantime of the failure of rolling upgrade, cross-shard transactions were received by the failed shards’ leaders and proposed in the next new block. The new block failed to be accepted by nodes with the new code due to the lack of backward compatibility logic in the new code.

## Action Items

<table>
  <thead>
    <tr>
      <th style="text-align:left">Category</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Owner</th>
      <th style="text-align:left">Tracker</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Preventive</td>
      <td style="text-align:left">Internal Code <a href="https://docs.google.com/spreadsheets/d/1oIox0C7hHnYgIv0FXRUZl2L5bKdnfoS43nP57oFte0Q/edit#gid=0">Auditing</a> on
        core protocol</td>
      <td style="text-align:left">RJ</td>
      <td style="text-align:left">On-going</td>
    </tr>
    <tr>
      <td style="text-align:left">Preventive</td>
      <td style="text-align:left">Log betanet upgrade issues to prepare for mainnet upgrade</td>
      <td style="text-align:left">Charles</td>
      <td style="text-align:left">Done</td>
    </tr>
    <tr>
      <td style="text-align:left">Preventive</td>
      <td style="text-align:left">Enforce code change audit for the mainnet upgrade with related code contributors</td>
      <td
      style="text-align:left">Leo</td>
        <td style="text-align:left">Process</td>
    </tr>
    <tr>
      <td style="text-align:left">Corrective</td>
      <td style="text-align:left">Add cross-shard tx backward compatibility <a href="https://github.com/harmony-one/harmony/pull/1572">https://github.com/harmony-one/harmony/pull/1572</a>
      </td>
      <td style="text-align:left">RJ</td>
      <td style="text-align:left">Done</td>
    </tr>
    <tr>
      <td style="text-align:left">Corrective</td>
      <td style="text-align:left">
        <p>update state sync threshold to 10%</p>
        <p><a href="https://github.com/harmony-one/harmony/pull/1571">https://github.com/harmony-one/harmony/pull/1571</a>
        </p>
      </td>
      <td style="text-align:left">RJ</td>
      <td style="text-align:left">Done</td>
    </tr>
  </tbody>
</table>