# Staking your Validator

Staking can be performed using Harmony CLI to delegate tokens to a validator. You can delegate to your own validator or to someone else:

{% tabs %}
{% tab title="mainnet" %}
```text
./hmy --node="https://api.s0.t.hmny.io" staking delegate --delegator-addr one1pf75h0t4am90z8uv3y0dgunfqp4lj8wr3t5rsp --validator-addr one1pdv9lrdwl0rg5vglh4xtyrv3wjk3wsqket7zxy --amount 0 --chain-id mainnet --passphrase (yourwalletpass)
```
{% endtab %}

{% tab title="testnet" %}
```
./hmy --node="https://api.s0.os.hmny.io" staking delegate --delegator-addr one1pf75h0t4am90z8uv3y0dgunfqp4lj8wr3t5rsp --validator-addr one1pdv9lrdwl0rg5vglh4xtyrv3wjk3wsqket7zxy --amount 0 --chain-id testnet --passphrase (yourwalletpass)
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}


* **delegate** - the subcommand to perform delegation. Delegate puts the amount of token from delegator’s balance as delegation to the validator
* **delegator-addr** - the delegator’s one address
* **validator-addr** - the validator’s one address
* **amount** - the one tokens to delegate
{% endhint %}

This delegation transaction will be signed using the **delegator-addr**. Hence, the **delegator-addr** should exists in the keystore. Please check the section "[Wallet download, address and BLS key creation](https://docs.harmony.one/pangaea/pangaea-validators/wallet-download-and-address-creation)" on how to do it.  


