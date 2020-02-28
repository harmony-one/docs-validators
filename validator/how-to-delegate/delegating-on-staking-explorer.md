# Delegating on staking explorer

### Overview

Harmony staking explorer is s[taking.harmony.one ](https://staking.harmony.one/)and harmony has a chrome extension which is used for signing private keys and signing transactions. This guide walks through installing the chrome extension and importing your private keys so that you can log in, view validators and delegate

#### Installing harmonyExtension 

You can install the Harmony Extension from the chrome store [here](https://chrome.google.com/webstore/detail/harmony/dmknnpkhnockodmnclcellfiilmklimd)

### Import your private key into the Harmony Extension

#### Retrieve your private key for your account

* Log into your validator - this is your AWS instance which you deployed your validator
* Get your account name by using `./hmy keys list`
* Get your private key from your node by running command `./hmy keys export-private-key <<account-name>>` where account name is the account you created and listed above
* Copy your private key

#### Import your private key into the Harmony Extension

* Open the Harmony extension

![Open the Harmony Extension](../../.gitbook/assets/selectharmonyextension.png)

* Add another account
* Select Private Key
* Paste your private key

#### Log in to staking.harmony.one

#### Set your network

#### View validators

#### Delegate to a validator

