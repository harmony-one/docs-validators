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
ssh -i "JOHN_OSTN_VALIDATOR_1.pem" ec2-user@ec2-18-216-111-12.us-east-2.compute.amazonaws.com

# Update the software and install tmux
sudo yum update
sudo yum install tmux

# Download the CLI
curl -LO https://harmony.one/hmycli && mv hmycli hmy && chmod +x hmy

# Generate your BLS Key
./hmy keys generate-bls-key


#### Download and run NODE using https://harmony.one/node2.sh #####

# Download Node Software (as of writing OSTN is using node2.sh)
curl -LO https://harmony.one/node2.sh
mv node2.sh node.sh
chmod a+x node.sh

# Run your node - NOTE : Replace the BLS key below with your BLS key
./node.sh -S -N staking -k be1d3bc4c5bc185bd12226912372007ae7baa573dcf1e7182e13728db121001cf33d6fd80969c39312eb50cf5b090d87.key -z

#### END Download and run NODE using https://harmony.one/node2.sh #####


#### Download and run node using P-OPS approach ####

bash <(curl -sSL https://raw.githubusercontent.com/SebastianJ/harmony-tools/master/install/install.sh) --node
ln -s *.key bls.key
./node.sh -k bls.key -N staking -z -D -S

#### END Download and run node using P-OPS approach ####

# Check the version of your harmony binary
# It should align with http://watchdog.hmny.io/report-staking version on the left
# e.g. Version-v5546-master-20191208.0-169-g3c4d5074
LD_LIBRARY_PATH=. ./harmony -version


# Create your validator wallet
./hmy keys add johnv0-account

# See your account
./hmy keys list

# Export private key (if needed to import into another wallet)
./hmy keys export-private-key one1y5n7p8a845v96xyx2gh75wn5eyhtw5002lah27

# Show the location of the keys (you should copy this file to a safe backup)
./hmy keys location





# Install tmux
sudo yum install tmux

# Start a tmux session
tmux new-session -s node

# Start your node
./node.sh -S -N staking -k be1d3bc4c5bc185bd12226912372007ae7baa573dcf1e7182e13728db121001cf33d6fd80969c39312eb50cf5b090d87.key -z

# Detach your tmux session <CTL>b d


# Register your validator
./hmy --node="https://api.s0.os.hmny.io" staking create-validator --validator-addr one1c2rmw8z24fcclgkx82cv70p29adkl4hxu34udl --name JohnWhittonV0 --identity johnIdentityV0 --website john@harmony.one --security-contact John --details "John the validator Shard 0" --rate 0.05 --max-rate 0.8 --max-change-rate 0.02 --min-self-delegation 10 --max-total-delegation 100 --bls-pubkeys 7690161e4ceec736f9a05bc16530d5668200e7c2e1b667ede6e17d8f654f80806418f529b8701af1a72ab9f00c02a191 --amount 10 --chain-id testnet

# {"transaction-receipt":"0x8396f56d0c9252777cd3a9c289bbcf7e9faf7bd3d0f2a3fc6f5375ae4a415443"}

# Check your validator has been registered
./hmy blockchain validator all | grep one1c2rmw8z24fcclgkx82cv70p29adkl4hxu34udl
./hmy blockchain validator information one1c2rmw8z24fcclgkx82cv70p29adkl4hxu34udl

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

