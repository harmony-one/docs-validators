# Creating your Validator BLS Keys

For working with all CLI commands, the Harmony team has created a new tool, the Harmony CLI-tool. With this tool you will be able to do everything on your node, like:

* Creating a wallet
* Checking balances
* Create your Validator
* Create your delegator
* And many more...

## Download Harmony CLI tool

**1.** First, 

```text
curl -LO https://harmony.one/hmycli && mv hmycli hmy && chmod +x hmy
```

or alternatively use this, if the above version will not work for creating a wallet below \(for MacOSX users\) and also note that instead of using ./hmy you have to use ./hmy.sh:

```text
curl -O https://raw.githubusercontent.com/harmony-one/go-sdk/master/scripts/hmy.sh
chmod u+x hmy.sh
./hmy.sh -d
```

## Creating a new BLS key for your Node

You will now need to generate your bls keys, to get your BLS Keys enter the command:

```text
./hmy keys generate-bls-key
```

1. Give your passprase when asked
2. Confirm the passprase \(please save your password\)

With this done, you will get the "**public-key**", "**private-key**" and the "**encrypted-private-key-path**"

#### Checking which shard your BLS key is for

You can check which shard your key is for by using the following command

```text
./hmy utility shard-for-bls be23bc3c93fe14a25f3533feee1cff1c60706845a4907c5df58bc19f5d1760bfff06fe7c9d1f596b18fdf529e0508e0a --node https://api.s0.os.hmny.io 
{"shard-id":1}
```

## Checking Wallet Balances

You can check wallet balances with the following command if you are running a node and are synced:

```text
./hmy balances one1f33w8c3dupm30p4fhemy89646xw7xpxcnmy6e4
```

or the following command if you're not running a node or not in sync:

```text
./hmy --node="https://api.s0.os.hmny.io" balances one1f33w8c3dupm30p4fhemy89646xw7xpxcnmy6e4
```

