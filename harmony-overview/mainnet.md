# Mainnet

## Overview

Mainnet v0 is Harmony's production environment which started running since June 28th 2019. It is a temporarily permissioned blockchain under active development and upgrade. The current mainnet is versioned "v0" and its main purpose is to exercise the currently implemented features on a large-scale and semi-open network in preparation for the fully permissionless Mainnet in next phase.

### Shards

Currently, Harmony mainnet v0 contains 4 shards and a total of 1000 validator nodes. Out of the 1000 nodes, 680 of them are operated by Harmony team and 320 of them are operated by external validators including our foundational node runners.

The 4 shards are indexed with 0, 1, 2, 3 respectively and shard 0 is the beacon chain. Right now, the nodes in shard 1, 2, and 3 are synchronizing and maintaining an updated copy of shard 0 \(beacon chain\) in preparation for the upcoming features such as cross-link verification and staking.

### Block Latency

The current block latency in mainnet v0 is configured to be at least 8 seconds. This means the new block will not be proposed until 8 seconds have passed after the last block proposal even if the actual FBFT consensus finishes within 8 seconds.

While the 8 seconds is the current lower bound of mainnet block latency, the actual block latency depends on how fast the FBFT consensus can be reached, which mostly depends on network latency and computation speed of validators. If a lot of nodes have network bandwidth or CPU resource issue, the actual block latency can grow beyond 8 seconds. This situation happened when the beacon chain synchronization was first released and it heavily consumed lots of bandwidth and computation power of nodes in shard 1, 2, 3, causing those shards to have block latency up to 16 seconds.

### Block Rewards

There is a fixed 24 ONE block rewards for every block proposed in each shard. The 24 ONE token are equally rewarded to all validators who signed the COMMIT message on that block. The block rewards algorithm works in the following way. If the leader collects enough signatures from the validators \(2/3 quorum + 1\), then there is a 2 seconds timer started. Within the next 2 seconds, either 90% of validators signed the block already, or 2 seconds passed, depending on which one reach at first. Then the block rewards will be equally rewards to all validators who signed.

Thus, in this case, not all the validators are able to get the block rewards if the validator is not fast enough to sign the COMMIT message within the time or in the top 90%. The rate of earning block rewards for a validator depends on following factors:

* Number of signers on the block: more signers means less individual reward.
* Block latency: the faster the block latency, the more rewards over time.

In order to allow more external validators to earn block rewards, the nodes operated by Harmony team are currently configured to have a **2 seconds delay** when signing the COMMIT message.

The way to improve the block rewards, includes using a faster host \(AWS _t3.small_ is the minimal\) to run the node software, providing a better network uplink to the host \(1G/s is the minimal\). In some cases, the p2p network may not be formed evenly that may include some offline nodes in the shard, it is better to restart the node software to connect to different peers.

### Web Resources

* Blockchain Explorer
  * [**https://explorer.harmony.one/**](https://explorer.beta.harmony.one/#/)\*\*\*\*
* 1 hour earning of validator nodes
  * [**https://harmony.one/1h**](https://harmony.one/1h)\*\*\*\*

### RPC Endpoints

* **Shard 0**
  * SDK RPC end point
    * [https://api.s0.t.hmny.io](https://api.s0.t.hmny.io)
  * SDK RPC Websocket end point
    * wss://ws.s0.t.hmny.io
  * [http://s0.t.hmny.io](http://s0.t.hmny.io)
  * Explorer Node REST end point
    * [http://e0.t.hmny.io:5000](http://e0.t.hmny.io:5000)
* **Shard 1**
  * SDK RPC end point
    * [https://api.s1.t.hmny.io](https://api.s1.t.hmny.io)
  * SDK RPC websocket end point
    * wss://ws.s1.t.hmny.io
  * [http://s1.t.hmny.io](http://s1.t.hmny.io)
  * Explorer Node REST end point
    * [http://e1.t.hmny.io:5000](http://e1.t.hmny.io:5000)
* **Shard 2**
  * SDK RPC end point
    * [https://api.s2.t.hmny.io](https://api.s2.t.hmny.io)
  * SDK RPC Websocket end point
    * wss://ws.s2.t.hmny.io
  * [http://s2.t.hmny.io](http://s2.t.hmny.io)
  * Explorer Node REST end point
    * [http://e2.t.hmny.io:5000](http://e2.t.hmny.io:5000)
* **Shard 3**
  * SDK RPC end point
    * [https://api.s3.t.hmny.io](https://api.s3.t.hmny.io)
  * SDK RPC Websocket end point
    * wss://ws.s3.t.hmny.io
  * [http://s3.t.hmny.io](http://s3.t.hmny.io)
  * Explorer Node REST end point
    * [http://e3.t.hmny.io:5000](http://e3.t.hmny.io:5000)

