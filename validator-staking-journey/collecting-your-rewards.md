# Collecting your rewards

Collecting rewards can be performed using Harmony CLI as follows:

```text
./hmy --node="https://api.s0.p.hmny.io" staking collect-rewards --delegator-addr one1pf75h0t4am90z8uv3y0dgunfqp4lj8wr3t5rsp --chain-id testnet --passphrase
```

{% hint style="info" %}


* **collect-rewards** - is the subcommand for collecting rewards. All token rewards will be recorded in a separate balance for each validator/delegator pair. Delegators needs to issue collect-rewards command to move those token rewards into its own balance.
* **delegator-addr** - is the delegator address to collect rewards
{% endhint %}

This collect rewards transaction will be signed using the **delegator-addr**. Hence, the **delegator-addr** should exists in the keystore.

