# Changing Validator Information

You can edit your validator’s information using the CLI with the following command.

```text
./hmy --node="https://api.s0.p.hmny.io" staking edit-validator \
    --validator-addr [ONE ADDRESS] \
    --name John --identity john --website john@harmony.one \
    --security-contact Alex --details "John the validator" \
    --rate 0.3 --min-self-delegation 2 --max-total-delegation 30 \
    --remove-bls-key 0xb9486167ab9087ab818dc4ce026edb5bf216863364c32e42df2af03c5ced1ad181e7d12f0e6dd5307a73b62247608611 \
    --add-bls-key 0xb9486167ab9087ab818dc4ce026edb5bf216863364c32e42df2af03c5ced1ad181e7d12f0e6dd5307a73b62247608611 \
    --chain-id testnet --passphrase

```

The CLI will prompt you to enter your BLS key file password.

`--validator-addr` is the address that will receive the rewards

`--amount` is the initial amount you want to stake

`--bls-pubkeys` takes a comma-separated list of BLS public keys \(not files\)

`--name` will be the name displayed on the Staking Explorer

`--identity` is additional information for the validator

`--details` is the description of the validator

`--security-contact` is additional information for the validator

`--website` will be displayed on the Staking Explorer

`--max-change-rate` is the maximum change that the validator can change their commission rate

`--max-rate` is the maximum commission rate that the validator can set

`--rate` is the starting commission rate of the validator

`--max-total-delegation` is the maximum amount that can be delegated to this validator

`--min-self-delegation` is the minimum amount of stake the validator must delegate to itself

{% hint style="info" %}
* **validator-addr** - is the subcommand for editing the validator's address
* **name, identity, website, security-contact, details** - are the description about validator
* **rate** - the commission rate that needs to be changed. Note that, max-rate and max-change-rate cannot be changed.
* **min-self-delegation** - is the minimum delegation the validator itself have to put
* **max-total-delegation** - is the maximum delegation the validator will accept
* **remove-bls-key** - the bls public key to remove from the validator’s list of bls public keys. When a key is removed the total stake will be evenly redistributed to the rest of the slots.
* **add-bls-key** - the bls public key to add to the validator’s list of bls public keys. When a key is added, the total stake will be evenly redistributed again to all the slots.
{% endhint %}



