# Changing Validator Information

You can edit your validator’s information using the CLI with the following command.

```text
./hmy --node="https://api.s0.p.hmny.io" staking edit-validator \
    --validator-addr [ONE ADDRESS] \
    --name John --identity john --website john@harmony.one \
    --security-contact Alex --details "John the validator" \
    --rate 0.3 --min-self-delegation 2 --max-total-delegation 30 \
    --remove-bls-key [BLS PUBLIC KEY] \
    --add-bls-key [BLS PUBLIC KEY] --passphrase
```

The CLI will prompt you to enter your BLS key file password. Only the `--validator-addr` field is required; all other fields are optional.

`--validator-addr` is the validator address that you want to edit

`--name` to change the name displayed on the Staking Explorer

`--identity` to change the identity field

`--website` to change the website field

`--details` to change the details field

`--security-contact` to change the security contact field

`--rate` to change the current commission rate

`--min-self-delegation` to change the minimum stake by the validator

`--max-total-delegation` to change the maximum stake that the validator can have

`--remove-bls-key` to remove a BLS public key associated with your validator

`--add-bls-key` to add another BLS public key to your validator 

{% hint style="info" %}
`--validator-addr`is the only field that is required.

Sending the command without the arguments will leave those fields of your validator as is.
{% endhint %}



