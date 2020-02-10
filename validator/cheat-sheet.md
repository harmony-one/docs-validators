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
# Log in to your validator node

cd /Users/johnwhitton/projects/staking/OSTN
ssh -i "JOHN_OSTN_VALIDATOR_1.pem" ec2-user@ec2-18-188-12-224.us-east-2.compute.amazonaws.com

# Download the CLI
curl -LO https://harmony.one/hmycli && mv hmycli hmy && chmod +x hmy

# Create your validator wallet
./hmy keys add johnv0-account

# See your account
./hmy keys list

# Export private key (if needed to import into another wallet)
./hmy keys export-private-key one1y5n7p8a845v96xyx2gh75wn5eyhtw5002lah27

# Show the location of the keys (you should copy this file to a safe backup)
./hmy keys location

# Generate your BLS Key
./hmy keys generate-bls-key

# Download Node Software (as of writing OSTN is using node2.sh
curl -LO https://harmony.one/node2.sh
mv node2.sh node.sh
chmod a+x node.sh

# Install tmux
sudo yum install tmux

# Start a tmux session
tmux new-session -s node

# Start your node
./node.sh -S -N staking -k be1d3bc4c5bc185bd12226912372007ae7baa573dcf1e7182e13728db121001cf33d6fd80969c39312eb50cf5b090d87.key -z

# Detach your tmux session <CTL>b d


# Register your validator
./hmy --node="https://api.s0.os.hmny.io" staking create-validator --validator-addr one1y5n7p8a845v96xyx2gh75wn5eyhtw5002lah27 --name JohnWhittonV0 --identity johnIdentityV0 --website john@harmony.one --security-contact John --details "John the validator Shard 0" --rate 0.05 --max-rate 0.8 --max-change-rate 0.02 --min-self-delegation 10 --max-total-delegation 100 --bls-pubkeys be1d3bc4c5bc185bd12226912372007ae7baa573dcf1e7182e13728db121001cf33d6fd80969c39312eb50cf5b090d87 --amount 10 --chain-id testnet

# {"transaction-receipt":"0x8396f56d0c9252777cd3a9c289bbcf7e9faf7bd3d0f2a3fc6f5375ae4a415443"}

# Check your validator has been registered
./hmy blockchain validator all | grep one1y5n7p8a845v96xyx2gh75wn5eyhtw5002lah27
./hmy blockchain validator information one1y5n7p8a845v96xyx2gh75wn5eyhtw5002lah27

# Changing your validator profile

# Staking your validator

# Bidding for a slot

# Check if your bid was succesful

# Unstaking your validator

# Seeing all your stakers

# Monitoring your rewards

# Collectiing your rewards

```
{% endtab %}

{% tab title="Mainnet" %}
```

```
{% endtab %}
{% endtabs %}

#### 

