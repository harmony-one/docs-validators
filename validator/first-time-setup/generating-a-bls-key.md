# Generating A BLS Key

## Creating a new BLS key

You will need to generate a BLS key in order to run a validating node. When generating a BLS key, the CLI will ask you to provide a passphrase to encrypt the BLS key file.‌ 

```text
./hmy keys generate-bls-key --passphrase
```

{% hint style="danger" %}
Remember your passphrase. You will need it to decrypt the BLS key file in order to create a validator & start a node with that key.

Create a backup of your BLS key file or save the BLS private key \(optional\).
{% endhint %}

{% hint style="info" %}
The BLS public key is the same as the name of the file, without the `.key`.
{% endhint %}

#### Checking which Shard the BLS will validate on

You can check which shard your key will validate for using the following command.

{% tabs %}
{% tab title="Open Staking Network" %}
```
./hmy --node="https://api.s0.os.hmny.io" utility shard-for-bls [BLS PUBLIC KEY]
```
{% endtab %}

{% tab title="Partner Test Network" %}
```text
./hmy --node="https://api.s0.ps.hmny.io" utility shard-for-bls [BLS PUBLIC KEY]
```
{% endtab %}
{% endtabs %}

Example output:

```text
{"shard-id":1}
```

