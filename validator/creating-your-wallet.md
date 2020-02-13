# Creating your Wallet

## New wallet creation <a id="new-local-account-creation"></a>

Creation of a new account is done as a function of a generated `bip39` mnemonic with 256 bits of entropy. You need to provide an account alias name of your choice \(keep it simple and clean\) and also need to  provide a passphrase:

```text
./hmy keys add example-account --passphrase

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

## Checking Wallet Balances

You can check wallet balances with the following command if you are running a node and are synced:

```text
./hmy balances one1f33w8c3dupm30p4fhemy89646xw7xpxcnmy6e4
```

or the following command if you're not running a node or not in sync:

```text
./hmy --node="https://api.s0.os.hmny.io" balances one1f33w8c3dupm30p4fhemy89646xw7xpxcnmy6e4
```

