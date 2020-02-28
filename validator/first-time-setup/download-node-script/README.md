# Download node script

{% hint style="info" %}
Optionally for faster synching on mainnet you can [download the blockchain database](downloading-blockchain-database.md)
{% endhint %}

## Download node.sh

**1.** Run the following command to download the node.sh script:

{% tabs %}
{% tab title="OSTN" %}
```
curl -LO https://harmony.one/node2.sh
mv node2.sh node.sh
chmod a+x node.sh
```
{% endtab %}

{% tab title="MAINNET" %}
```
curl -LO https://raw.githubusercontent.com/harmony-one/harmony/pangaea/scripts/node.sh
chmod u+x node.sh
```
{% endtab %}
{% endtabs %}

**Note:** Run the "OSTN" commands if you are connecting to the Open Staking TestNet. If you are connecting to the mainnet, use the commands under mainnet.

## Run Node

**1.** Create a new tmux session called "node".

```text
tmux new-session -s node
```

**2.**  Run the node.sh script with the following command. Once you do, it will ask for a passphrase for your BLS key file. Type your passphrase on the screen that follows and your node should be up and running.

```css
./node.sh -S -k YOUR-KEY-FILE.key -N testnet -z
```

{% hint style="info" %}
-S is to avoid having to run node.sh as root \(a common complaint on mainnet\)

-k is to specify your BLS key

-z is to start open staking
{% endhint %}

**3.** Detach your "node" tmux session by pressing **Ctrl+B** then releasing and pressing **D**.



To check if your node is syncing properly, please check the "_Node Monitoring_" section at the "[Useful Commands](https://docs.harmony.one/pangaea/help-section/useful-commands)" page. There you will find commands to check at which block height you are, which shard, epoch, etc.

If you want to detach your tmux session to leave the Harmony process running just press **Ctrl+B**. Let go of those keys. Then press **D**. This will safely disconnect you from your cloud instance and **shut down** tmux while leaving your node running in your cloud instance.

To re-attach to your tmux session where your node.sh is running please use the following command:

```text
tmux attach-session -t node
```

More details on how to use tmux please click [here](../../../additional-information/reference/tmux.md).

