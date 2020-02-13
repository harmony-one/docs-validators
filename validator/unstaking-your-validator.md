# Unstaking your validator

## **Un-delegation can be performed using Harmony CLI as follows:**

{% tabs %}
{% tab title="OSTN" %}
```
./hmy --node="https://api.s0.os.hmny.io" staking undelegate --delegator-addr one1pf75h0t4am90z8uv3y0dgunfqp4lj8wr3t5rsp --validator-addr one1pdv9lrdwl0rg5vglh4xtyrv3wjk3wsqket7zxy --amount 0 --chain-id testnet --passphrase (yourwalletpass)
```
{% endtab %}

{% tab title="mainnet" %}
```
./hmy --node="https://api.s0.t.hmny.io" staking undelegate --delegator-addr one1pf75h0t4am90z8uv3y0dgunfqp4lj8wr3t5rsp --validator-addr one1pdv9lrdwl0rg5vglh4xtyrv3wjk3wsqket7zxy --amount 0 --chain-id mainnet --passphrase (yourwalletpass)
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}


* **undelegate -** is the subcommand for performing undelegation. Undelegate will move the amount of delegated tokens into the “Undelegated” state which after 4 \( for Pangaea\) epochs will be automatically credited to the delegator’s account.
* **delegator-addr -** one address of the delegator
* **validator-addr -** one address of the validator
* **amount -** one tokens to undelegate

**chain-id -** can be one of {mainnet, **testnet**, pangaea} pls use **testnet**
{% endhint %}

This undelegation transaction will be signed using the delegator-addr. Hence, the delegator-addr should exists in the keystore.

### When will I get my tokens back after undelegating?

When you decide to undelegate your tokens from a validator in Pangaea your tokens will be subjected to a cooldown period lasting **7 epochs / 70 minutes**.

This means that you **won't have access to your tokens or can transfer them for 70 minutes** after choosing to undelegate your tokens from a validator.

You can read more about epochs in Pangaea [here](https://docs.harmony.one/pangaea/help-section/epochs).

