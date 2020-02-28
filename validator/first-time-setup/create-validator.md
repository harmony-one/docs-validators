# Create validator

{% hint style="info" %}
**Note registering as a validator requires you have ONE tokens** in the wallet you are using for registration. OSTN is currently using pre-funded accounts. To gain access to funds in OSTN you can reach out on discord [foundational nodes channel](https://discord.gg/nyPcdPC). Moving forward a faucet  with funding instructions will be available. 
{% endhint %}

**A validator can be created using Harmony CLI \(hmy\) as follows:**  
Please note that the below examples include addresses and BLS public keys that you need to swap out for your own.

```text
./hmy --node="https://api.s0.os.hmny.io" staking create-validator --validator-addr one1pdv9lrdwl0rg5vglh4xtyrv3wjk3wsqket7zxy --name John --identity john --website john@harmony.one --security-contact Alex --details "John the validator" --rate 0.1 --max-rate 0.9 --max-change-rate 0.05 --min-self-delegation 2 --max-total-delegation 30 --bls-pubkeys 0xb9486167ab9087ab818dc4ce026edb5bf216863364c32e42df2af03c5ced1ad181e7d12f0e6dd5307a73b62247608611 --amount 3 --chain-id testnet --passphrase yourwalletpass
```

Upon pressing enter it will prompt you to enter your bls password also. If everything goes well, you'll receive a tx hash in return.

{% hint style="info" %}
the BLS pub key is obtained during the bls key creation, if you didn't take note of it during the creation you can recover it via the command :

./hmy keys recover-bls-key &lt;absolute\_path\_to\_your\_bls.key&gt;

ex : ./hmy keys recover-bls-key /root/b3727f8a497174dba2d4859a131a79f4065fa8d18b5e51e5624bc7dcc1e39ae25b0eb5a0b9a4b25a49c9ee3d3ebf7804.key
{% endhint %}

{% hint style="info" %}


* **hmy** - Harmony CLI binary
* **staking** - subcommand for staking
* **create-validator** - subcommand for creating validator
* **validator-addr** - validator’s one address
* **name, identity, website, security-contact, details** - are the description about the validator
  * 
* **rate, max-rate, max-change-rate** - are the commission rates where the values should be between \[0, 1\] indicating the decimal percentage. 
  * A rate of 0.1 means 10% of the rewards will be cut and given to the validator and the rest will be given to all the delegators \(including the validator’s self delegation\), 
  * a max-rate of 0.9 means the maximum commission rate that the validator can set is 90%, and 
  * a max-change-rate of 0.05 means the maximum rate of change the validator can apply to it’s commission rate every epoch is 5%
* **min-self-delegation** - is the minimum delegation the validator itself have to put
* **max-total-delegation** - is the maximum delegation the validator will accept \(setting it to 0 means there is no maximum delegation limit\)
* **bls-pubkeys** - is the comma separated list of bls public keys for all validating slots the validator like to take
* **amount** - the initial stake of the validator \(self-delegation\). It is evenly distributed to each of the validating slots. E.g., if the amount is 100 and there are 10 bls public keys, each slot stake will be 10
* **chain-id** - can be one of {mainnet, **testnet**, devnet} ; please use "**testnet"** in this case.
{% endhint %}

This create-validator transaction will be signed using the **validator-addr key**. Hence, the **validator-addr key** should exist in the keystore. There are multiple ways to manage your wallet account. Using [mathwallet](https://docs.harmony.one/home/api/math-wallet/mathwallet) or Harmony CLI local keystore. Using Harmony CLI you could do this as follows:

```text
./hmy keys import-private-key <your-private-key>

Or

./hmy keys import-ks <absolute-path-to-keystore-json-file> passphrase
```

After adding the key to the local keystore, execute following command that lists the account names and one addresses of all the local accounts.

```text
./hmy keys list

NAME                  ADDRESS
ac1                   one1pdv9lrdwl0rg5vglh4xtyrv3wjk3wsqket7zxy
ac2                   one1pdv9lrdwl0rg5vglh4xtyrv3wjk3wsqket7zxy
```

### When does the validator become active?

In Pangaea the validator will become active within 1 epoch / 10 minutes.

You can read more about epochs in Pangaea [here](https://docs.harmony.one/pangaea/help-section/epochs).

Once create-validator is active and assuming you secured a seat in the EPOS committee, you'll start receiving rewards along with the famous BINGOs

```text
tail -f latest/zerolog-validator-x.x.x.x-9000.log | grep -i BINGO
{"level":"info","port":"9000","ip":"213.136.79.89","blockNum":3916,"epochNum":26,"ViewId":3916,"blockHash":"0xca71fc9aa92f694f664aa34d7e3e82cf9b678e3a062d3bbbabebfbc5f0598d84","numTxns":0,"numStakingTxns":0,"caller":"/mnt/jenkins/workspace/harmony-release/harmony/node/node_handler.go:359","time":"2019-12-11T14:49:08.983338784+01:00","message":"BINGO !!! Reached Consensus"}

```

## Checking Validator Information:

**Getting information about a validator can be performed using Harmony CLI as follows:**  


```text
./hmy --node=https://api.s0.os.hmny.io/ blockchain validator information one1pdv9lrdwl0rg5vglh4xtyrv3wjk3wsqket7zxy
```

The output should show the validator information for validator one address one1pdv9lrdwl0rg5vglh4xtyrv3wjk3wsqket7zxy:

