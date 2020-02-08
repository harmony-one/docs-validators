# Starting your Validator Node

## Download Node

Use the following command to download the node.sh script:

```text
curl -LO https://raw.githubusercontent.com/harmony-one/harmony/pangaea/scripts/node.sh
chmod u+x node.sh
```



## Run Node

First you need to start a new tmux session again:

```text
tmux new-session -s node
```

Then run the node.sh script with the following command. Once you do, it will ask for a passphrase for the BLS key file. Type your passphrase on the screen that follows and your node should be up and running:

```text
./node.sh -S -k f98570a54c445867a84b0b83d52d2bda1e9e283b20e50aad269be196f2a4bea5fe01fe4ad956721b0b99b1ee78f65f02.key -N testnet -z
```

\*change the above command with your .key BLS name previously generated

{% hint style="info" %}
-S is to avoid having to run node.sh as root \(a common complaint on mainnet\)

-k is to specify your BLS key

-z is to start open staking
{% endhint %}

To check if your node is syncing properly, please check the "_Node Monitoring_" section at the "[Useful Commands](https://docs.harmony.one/pangaea/help-section/useful-commands)" page. There you will find commands to check at which block height you are, which shard, epoch, etc.

If you want to detach your tmux session to leave the Harmony process running just press **Ctrl+B**. Let go of those keys. Then press **D**. This will safely disconnect you from your cloud instance and **shut down** tmux while leaving your node running in your cloud instance.

To re-attach to your tmux session where your node.sh is running please use the following command:

```text
tmux attach-session -t node
```

More details on how to use tmux please click [here](../additional-information/reference/tmux.md).

