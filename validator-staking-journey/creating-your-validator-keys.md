# Creating your Validator Keys



{% hint style="danger" %}
ToDo

* Review CLI commands and ensure references are valid for mainnet.
{% endhint %}

For working with all cli commands, the Harmony team has created a new tool, the Harmony CLI-tool.  
With this tool you will be able to do everything on your node, like:

* Creating a wallet
* Checking balances
* Create your Validator
* Create your delegator
* And many more...

## Download Harmony CLI tool

```text
curl -LO https://harmony.one/hmycli && mv hmycli hmy && chmod +x hmy
```

or alternatively use this, if the above version will not work for creating a wallet below \(for MacOSX users\) and also note that instead of using ./hmy you have to use ./hmy.sh:

```text
curl -O https://raw.githubusercontent.com/harmony-one/go-sdk/master/scripts/hmy.sh
chmod u+x hmy.sh
./hmy.sh -d
```

## New wallet creation <a id="new-local-account-creation"></a>

Creation of a new account is done as a function of a generated `bip39` mnemonic with 256 bits of entropy. You need to provide an account alias name of your choice \(keep it simple and clean\) and also need to  provide a passphrase:

```text
./hmy keys add example-account --use-own-passphrase

Enter passphrase:
Repeat the passphrase:

```

When creating keys this way, `hmy` will ask you to provide a passphrase.‌ The example-account is just a name you can pick, so pick something simple.  
Make sure you keep track of this passphrase for future use because the passphrase is used to decrypt the keystore when signing transactions. Also make sure you save the seed phrase.

To know where you wallet file has been created, just run the following command:

```text
./hmy keys location
```

The command above will return the location of your wallet file. Backup this wallet file somewhere else.‌

You can check the list of wallets \(local accounts\) with the following command:

```text
./hmy keys list
NAME                                  ADDRESS

acc3                                  one1wh4p0kuc7unxez2z8f82zfnhsg4ty6dupqyjt2
example-account                       one1658znfwf40epvy7e46cqrmzyy54
```

NOTE: once you have created the wallet, please make sure to submit the wallet address to this [form](https://pangaeabyharmony.typeform.com/to/uPIehn).

## Creating a new BLS key for your Node

You will now need to generate your bls keys, to get your BLS Keys enter the command:

```text
./hmy keys generate-bls-key
```

1. Give your passprase when asked
2. Confirm the passprase \(please save your password\)

With this done, you will get the "**public-key**", "**private-key**" and the "**encrypted-private-key-path**"

## Downloading Blockchain Database

Before the node can run and join consensus to earn block rewards, it will need a recent copy of blockchain database.  In order to download and extract the blockchain database, run the following commands \(replace `BLSPUBKEY` with your 96-digit hexadecimal BLS public key as displayed by the `generate-bls-key` command above\):

```text
curl -sSfLO https://hmny-pnga.s3-us-west-2.amazonaws.com/db/download-pangaea-db.sh
sh download-pangaea-db.sh BLSPUBKEY
```

This command takes up to an hour to complete.  After that, the following messages are displayed \(the shard\(s\) downloaded depends on your BLS public key\):

```text
Starting download of shard 2 database...
Starting download of shard 0 database...
Waiting for database download to finish...
Database download finished.

Ensure that there are no error messages above.  If any, re-run this script.
```

## Checking Wallet Balances

You can check wallet balances with the following command if you are running a node and are synced:

```text
./hmy balances one1f33w8c3dupm30p4fhemy89646xw7xpxcnmy6e4
```

or the following command if you're not running a node or not in sync:

```text
./hmy --node="https://api.s0.p.hmny.io" balances one1f33w8c3dupm30p4fhemy89646xw7xpxcnmy6e4
```

