---
description: Sample commands for the staking workflow
---

# Cheat Sheet

Below are sample commands used for the Staking Workflow.

This example is using the Open Staking Test Network and actual private keys. Do not reuse the keys provided here.

#### Log into your node

{% tabs %}
{% tab title="OSTN" %}
```text
#### PRE-REQUISITES ####
# Create your AWS Instance
# Have a terminal tool like ITERM2
# Download your .pem file to a directory of your choosing
#### END PRE-REQUISITES ####

#### Iniital Set up ####

# Log in to your validator node
cd /Users/johnwhitton/projects/staking/OSTN
ssh -i "JOHN_OSTN_VALIDATOR_1.pem" ec2-user@ec2-18-217-173-222.us-east-2.compute.amazonaws.com

# Update the software and install tmux
sudo yum update
sudo yum install tmux

# Download the CLI
curl -LO https://harmony.one/hmycli && mv hmycli hmy && chmod +x hmy

# Generate your BLS Key
./hmy keys generate-bls-key

#### Download and run NODE using https://harmony.one/node2.sh #####

# Start a tmux session
tmux new-session -s node

# Download Node Software (as of writing OSTN is using node2.sh)
curl -LO https://harmony.one/node2.sh
mv node2.sh node.sh
chmod a+x node.sh

# Run your node - NOTE : Replace the BLS key below with your BLS key
./node.sh -S -N staking -z -k be1d3bc4c5bc185bd12226912372007ae7baa573dcf1e7182e13728db121001cf33d6fd80969c39312eb50cf5b090d87.key

# Detach your tmux session <CTRL>+b, then d

#### END Download and run NODE using https://harmony.one/node.sh #####

# Check the version of your harmony binary
# It should align with http://watchdog.hmny.io/report-staking version on the left
# e.g. Version-v5546-master-20191208.0-169-g3c4d5074
LD_LIBRARY_PATH=. ./harmony -version

#### Creating your Wallet and Staking ####
# Create your validator wallet
./hmy keys add johnv1-account

# See your account
./hmy keys list

# Export private key (if needed to import into another wallet)
./hmy keys export-private-key one1wwtelhx8nfuu50z0lttqtz4mfrlsn6jm97ske4

# Show the location of the keys (you should copy this file to a safe backup)
./hmy keys location

# Fund your validator
curl -X GET https://faucet.os.hmny.io/fund?address=one1wwtelhx8nfuu50z0lttqtz4mfrlsn6jm97ske4
#{"success":true,"balances":[{"shard":0,"balance":"1000"},{"shard":1,"balance":"0"},{"shard":2,"balance":"0"},{"shard":3,"balance":"0"}]}

# Register your validator
./hmy --node="https://api.s0.os.hmny.io" staking create-validator --validator-addr one1wwtelhx8nfuu50z0lttqtz4mfrlsn6jm97ske4 --name JohnWhittonV1 --identity johnIdentityV1 --website john@harmony.one --security-contact John --details "John the validator Shard 1" --rate 0.05 --max-rate 0.8 --max-change-rate 0.02 --min-self-delegation 10 --max-total-delegation 100 --bls-pubkeys 3bdc1c12828cdea5e9fa7bdd6ea808e2587ccf7c7a694f5437162b9d9496c74995470ad3d049f672bfdf79ee05e7bc14 --amount 30 --chain-id testnet

# {
#  "id": "21",
#  "jsonrpc": "2.0",
#  "result": {
#    "blockHash": "0x5d44cfb6d85bbf81e2dcb0b6a8986522d443c5d6c9af1c43d1ad49efb9eac78a",
#    "blockNumber": "0x2e3",
#    "contractAddress": null,
#    "cumulativeGasUsed": "0x512f00",
#    "gasUsed": "0x512f00",
#    "logs": [],
#    "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
#    "sender": "one1wwtelhx8nfuu50z0lttqtz4mfrlsn6jm97ske4",
#    "status": "0x1",
#    "transactionHash": "0xd20009ae47177202983d90838340e3a5c3d9b113a95e32e4cbda2b2e9cc89ac4",
#    "transactionIndex": "0x0",
#    "type": 0
#  }
#}

# Check your validator has been registered
./hmy --node="https://api.s0.os.hmny.io" blockchain validator all | grep one1wwtelhx8nfuu50z0lttqtz4mfrlsn6jm97ske4
./hmy --node="https://api.s0.os.hmny.io" blockchain validator information one1wwtelhx8nfuu50z0lttqtz4mfrlsn6jm97ske4

# Seeing all your delegators
./hmy --node https://api.s0.os.hmny.io/ blockchain delegation by-validator one1wwtelhx8nfuu50z0lttqtz4mfrlsn6jm97ske4 

# Monitoring your rewards
./hmy --node https://api.s0.os.hmny.io/ blockchain delegation by-delegator one1wwtelhx8nfuu50z0lttqtz4mfrlsn6jm97ske4

# Collecting your rewards

./hmy --node="https://api.s0.os.hmny.io" balances one1wwtelhx8nfuu50z0lttqtz4mfrlsn6jm97ske4
./hmy --node="https://api.s0.os.hmny.io" staking collect-rewards --delegator-addr one1wwtelhx8nfuu50z0lttqtz4mfrlsn6jm97ske4 --chain-id testnet
./hmy --node="https://api.s0.os.hmny.io" balances one1wwtelhx8nfuu50z0lttqtz4mfrlsn6jm97ske4

# Unstaking your validator


# Changing your validator profile

# Staking your validator

# Bidding for a slot

# Check if your bid was succesful

# Unstaking your validator

# Collectiing your rewards

```
{% endtab %}

{% tab title="Mainnet" %}
```

```
{% endtab %}
{% endtabs %}

#### 

