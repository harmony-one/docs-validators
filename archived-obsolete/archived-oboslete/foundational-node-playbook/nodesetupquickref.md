---
description: FAQ and knowledge-base of running Harmony Node
---

# Node setup quick reference

## **First time running/setting up your node:**

```text
cd Downloads/                                  //change directory to downloads folder
chmod 400 Your_Pem_File                        //makes your key not publicly viewable
ssh -i......from amazon connect                //ssh into aws instance
sudo yum update                                //updates instance
mkdir -p ~/.hmy/keystore                       //makes a new directory to store harmony keys
```

## **Setting up a new wallet:**

```text
curl -LO https://harmony.one/wallet.sh         //downloads script
chmod a+x wallet.sh
./wallet.sh -d
./wallet.sh new                                //makes new Harmony wallet ID
./wallet.sh blsgen                             //makes BLS keypair
./wallet.sh list
```

## **Launching and running your node:**

Note if you are upgrade your node use `tmux attach-session -t node`instead of `tmux new-session -s node`

```text
sudo yum install -y tmux                       //installs tmux
tmux new-session -s node                       //creates new tmux session named "node"
curl -LO https://harmony.one/node.sh           //downloads script
chmod a+x node.sh
sudo ./node.sh                                 //sync your node to harmony blockchain
ctrl+b then d                                  //detaches from tmux
```

## **Checking your software version**

```text
LD_LIBRARY_PATH=$(pwd) ./harmony -version
```

## **Checking your block height**

```text
# beacon chain shard
tac latest/zerolog*.log | grep -m 1 blockShard.:0 | grep -oE blockNumber.:[0-9]+

# your shard, ex, 1
tac latest/zerolog*.log | grep -m 1 blockShard.:1 | grep -oE blockNumber.:[0-9]+
```

## **Checking the System Status**

* Balances - [http://harmony.one/balances](http://harmony.one/balances)
* Block Height \(Hourly earnings\) - [http://harmony.one/1h](http://harmony.one/1h)
* Explorer - [http://explorer.harmony.one](http://explorer.harmony.one)

## **Monitoring your node:**

```text
grep BINGO latest/zerolog*.log                 //checks BINGOs
./wallet.sh balances                           //checks your node's current balance
curl -OL http://harmony.one/mystatus.sh        //downloads script to check node status
chmod u+x ./mystatus.sh
./mystatus.sh all                              //checks all your node statuses
tac latest/zero*.log | grep -oam 1 -E "\"(blockNumber|myBlock)\":[0-9\"]*"
                                               //checks your current block number
tac latest/*.log | grep -oam 1 -E "\"(blockShard|[Ss]hardID)\":[0-3]"
                                               //checks which shard you're in
```

## **Useful Commands:**

```text
sudo ./node.sh                                 //restarts sync/if genesis not found in blockchain
tmux attach                                    //reattach back to tmux
./wallet.sh importBLS --key BLS_Private_Key    //resets your passphrase
watch -n 10 ./wallet.sh balances
./wallet.sh exportPriKey --account             //moves private key
```

## Check disk space

```text
df -h
```

## Check disk usage

```text
du -h
```

