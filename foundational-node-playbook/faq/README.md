# Frequently Asked Questions

_**I want to run more than one node on a single machine. Can I do that?**_

Yes, you can. However due to reliability issues, it is not advised not to do that. If the machine fails, all your nodes will fail as well. It is imperative to keep as many nodes up as possible at all times.

_**I'm getting this error message. "Can't find the matching account key of account\_index:123"**_

This is normal if you just got your index number! This means that there is a placeholder account on that account index. Please give the team 1-2 days to enter your account key and launch the index.

_**I can't find the balance on explorer.harmony.one**_

This is a known issue that the explorer has some compatibility issues with the view change algorithm. Even though it shows only 3 shards sometimes, it doesn't mean the shard is down. Please use your wallet ID to check your block rewards while we continue to fix this issue.

_**DEBUG\[06-10\|20:14:13.729\] \[SYNC\] no peers to connect to**_

A known issue. Please run `sudo ./node.sh $account` to retry.

_**Is my ONE Address linked to my BLS keys?**_

Generation of the BLS keys are independent of your harmony ONE address, you can create any amount of BLS keys independent.

_**How do I know which shard my node is at?**_

You can run `grep "shard" latest/validator-ip-address.log` and you'll see messages similar to

```text
{"got notified":"harmony/0.0.1/node/shard/2/ActionPause","ip":"54.149.210.19","lvl":"info","msg":"[DISCOVERY]","port":"9000","t":"2019-06-14T23:48:41.523967984Z"}
```

Here the node is on shard 2.

## _How do I check for Bingos?_

Run the command in tmux

`tac latest/zero*.log | grep -am 1 "BINGO"`

## _Am I part of consensus? \(Have I synced\)_

The more blocks in a blockchain the more content you need to download in order for your node to be up to date with the blockchain. When you are syncing you are downloading all the preexisting blocks. To know if you have finished syncing use the command above to check for Bingos. A bingo means that consensus has been reached on a new block.

## _Is my node still running when I close my terminal instance?_

If you hit **ctrl b + d** you DETACH from tmux and your node will continue running/syncing.

## _How to check my bootnode connectivity?_

First of all, you may install **netcat** command as a network diagnostic tool. This [document](https://ixnfo.com/en/installing-and-using-netcat.html) may provide some guidance. Then, use the following command to check the bootnode connectivity.

```text
nc -tv -w3 100.26.90.187 9874
nc -tv -w3 54.213.43.194 9874

nc -tv -w3 13.113.101.219 12019
nc -tv -w3 99.81.170.167 12019

# all of them should return something like the following

Connection to 13.113.101.219 port 12019 [tcp/*] succeeded!
/multistream/1.0.0
```

If there is any issue that the bootnode can't connect, please contact our Discord channel for help.

