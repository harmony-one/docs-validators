# tmux

## Why _tmux_? <a id="why-tmux"></a>

Running your node inside _tmux_ allows you to disconnect from your cloud instance \(i.e. AWS, Vultr\) without shutting down your node on your cloud instance.

## Tips: <a id="tips"></a>

### Creating a session: <a id="creating-a-session"></a>

```text
tmux new-session -s node
```

If you have done this correctly, your screen should be reset:

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LlDqlxK8e45wuh1WH4h%2F-LmBl4UMO5NJ_jpqKFIz%2F-LmBlXPwAvumyMrNquRL%2Fimage.png?alt=media&token=5d317651-5cad-4c28-93e9-942ff2b907f2)

While connected to a live _tmux_ session, you will see a green bar at the bottom of your screen.

If you see this

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LlDqlxK8e45wuh1WH4h%2F-LmBKWJFKC8YA467cEi1%2F-LmBLUVtEwbA95jGDfDX%2Fimage.png?alt=media&token=eebca614-7184-4e4e-99da-993b7539e013)

You already have a session running. Reattach to that session using the tips below.

### Detaching from your session: <a id="detaching-from-your-session"></a>

Press **ctrl+b**. Let go of those keys. Then press **d**. This will safely disconnect you from your cloud instance and **shut down** _tmux_ while leaving your node running in your cloud instance.

```text
<CTRL+b>
<d>
```

If you have done this correctly you should see `[detached]` printed onto your screen:

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LlDqlxK8e45wuh1WH4h%2F-LmBLy7Qr9ZeO3W45D6A%2F-LmBMRJTh64UU56iyhNi%2Fimage.png?alt=media&token=6a5b3108-ecba-42e5-8357-783a4cf196d1)

{% hint style="danger" %}
**DO NOT EXIT A SESSION USING** _**CTRL+C**_**. This will only shut down your node and not close** _**tmux**_**.** If you have already entered **`ctrl+c`**, you have shut down your node. You will need to restart your node using this command\(while inside a _tmux_ session\):

`sudo ./node.sh -t`
{% endhint %}

### Reattaching to your session:  <a id="reattaching-to-your-session"></a>

You will first need to be **inside** a _tmux_ session. If you need to, **create a new** _tmux_ session using the code above. Then use:

```text
tmux attach
```

If you have done this correctly, you should see a green bar at the bottom of your screen:

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LlDqlxK8e45wuh1WH4h%2F-LmBLy7Qr9ZeO3W45D6A%2F-LmBNBpX-tpH0KTixzi0%2Fimage.png?alt=media&token=4af67c2d-6596-48c6-8a17-25657fd7d6f0)

