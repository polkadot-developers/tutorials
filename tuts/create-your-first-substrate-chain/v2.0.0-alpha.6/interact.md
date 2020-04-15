---
slug: interact
lang: en
title: Interacting with Your Node
---

Now that your node has finished compiling, let's show you how everything works out of the box.

## Starting Your Node

Run the following commands to start your node:

```bash
# Purge chain cleans up any old data from running a `dev` node in the past
# You will be prompted to type `y`
./target/release/node-template purge-chain --dev

# Run your actual node in "development" mode
./target/release/node-template --dev
```

You should see something like this if your node is running successfully:

```
$ ./target/release/node-template --dev

2020-04-15 13:46:00 Running in --dev mode, RPC CORS has been disabled.
2020-04-15 13:46:00 Substrate Node
2020-04-15 13:46:00 ✌️  version 2.0.0-alpha.6-c1b33f8-x86_64-linux-gnu
2020-04-15 13:46:00 ❤️  by Anonymous, 2017-2020
2020-04-15 13:46:00 📋 Chain specification: Development
2020-04-15 13:46:00 🏷  Node name: abrasive-sand-1388
2020-04-15 13:46:00 👤 Role: AUTHORITY
2020-04-15 13:46:00 ⛓  Native runtime: node-template-1:1(node-template-1)
2020-04-15 13:46:00 🔨 Initializing Genesis block/state (state: 0x4603…c0c6, header-hash: 0x7fd5…54f7)
2020-04-15 13:46:00 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.
2020-04-15 13:46:00 ⏱ Loaded block-time = 6000 milliseconds from genesis on first-launch
2020-04-15 13:46:00 📦 Highest known block at #0
2020-04-15 13:46:00 Using default protocol ID "sup" because none is configured in the chain specs
2020-04-15 13:46:00 🏷  Local node identity is: QmStkPyqTUosZQvYyGzsQgbAnUv7xqE7VBYfThkePMzvuR
2020-04-15 13:46:00 〽 Prometheus server started at 127.0.0.1:9615
2020-04-15 13:46:05 💤 Idle (0 peers), best: #0 (0x7fd5…54f7), finalized #0 (0x7fd5…54f7), ⬇ 0 ⬆ 0
2020-04-15 13:46:06 🙌 Starting consensus session on top of parent 0x7fd5647d42940dd960cd19612679eed979c542f42efb4b5078e8d00dbfd754f7
2020-04-15 13:46:06 🎁 Prepared block for proposing at 1 [hash: 0x1ff8fd2f064782f65adabc3e79f75754b0cbb31ba5ea87e6ec37adcc2aa0d9fc; parent_hash: 0x7fd5…54f7; extrinsics (1): [0x851d…c946]]
2020-04-15 13:46:06 🔖 Pre-sealed block for proposal at 1. Hash now 0x2fc528b959cad12872c478a397526ad119ff1d309fd3ddd33f7e15d1dee3d4da, previously 0x1ff8fd2f064782f65adabc3e79f75754b0cbb31ba5ea87e6ec37adcc2aa0d9fc.
2020-04-15 13:46:06 🙌 Starting consensus session on top of parent 0x2fc528b959cad12872c478a397526ad119ff1d309fd3ddd33f7e15d1dee3d4da
2020-04-15 13:46:06 ✨ Imported #1 (0x2fc5…d4da)
2020-04-15 13:46:06 🎁 Prepared block for proposing at 2 [hash: 0x08ec27919011a0ce00ea94e1d3b9647b13392ce0643c8b10f292415b81c34b4f; parent_hash: 0x2fc5…d4da; extrinsics (1): [0xd56e…2407]]
2020-04-15 13:46:06 🔖 Pre-sealed block for proposal at 2. Hash now 0x6e845b7ebeec35587cc0ff89c815d0930e30ec43e5271d4fa9ecedfbcd159c63, previously 0x08ec27919011a0ce00ea94e1d3b9647b13392ce0643c8b10f292415b81c34b4f.
2020-04-15 13:46:06 ✨ Imported #2 (0x6e84…9c63)
2020-04-15 13:46:10 💤 Idle (0 peers), best: #2 (0x6e84…9c63), finalized #0 (0x7fd5…54f7), ⬇ 0 ⬆ 0
```

If the number after `best:` is increasing, your blockchain is producing new blocks!

## Start the Front End

To interact with the local node we will use the Polkadot-js Apps user interface, often known as
"Apps" for short. Despite the name, Apps will work with any Substrate-based blockchain including ours, not just Polkadot.

In your web browser, navigate to [https://polkadot.js.org/apps](https://polkadot.js.org/apps/#/settings?rpc=ws://127.0.0.1:9944).

On the `Settings` tab ensure that you are connected to a `Local Node` or `ws://127.0.0.1:9944`.

> Some browsers, notably Firefox, will not connect to a local node from an https website. An easy work around is to try another browser, like Chromium. Another option is to [host this interface locally](https://github.com/polkadot-js/apps#development).

## Interact

Select the **Accounts** tab, you will see test accounts that you have access to. Some, like Alice
and Bob, already have funds!

![Apps UI with pre-funded accounts](../assets/apps-prefunded.png)

You can try to transfer some funds from Alice to Charlie by clicking the "send" button.

![Balance Transfer](../assets/apps-transfer.png)

If everything went successfully, you should see some popup notifications claiming "Extrinsic
Success", and of course Charlie's balance will increase.

## Create Your Own Account

You can create your own account by selecting the `+ Add Account` button. It won't have any tokens
yet, but you can send some from Alice or any other pre-funded account. Only you (and your
browser) will know the private key for your own account which means nobody can transfer those tokens
except you.

## Next Steps

This is the end of your journey to launching your first blockchain with Substrate.

You have launched a working Substrate-based blockchain, attached a user interface to that chain, and made token transfers among users. We hope you'll continue learning about Substrate.

Your next step may be:

* Decentralize your network with more nodes in the [Start a Private Network](/tutorials/start-a-private-network/v2.0.0-alpha.6) tutorial.
* Add custom functionality in the [Build a dApp](/tutorials/build-a-dapp/v2.0.0-alpha.6) tutorial.

If you experienced any issues with this tutorial or want to provide feedback, feel free to [open a
GitHub
issue](https://github.com/substrate-developer-hub/tutorials/issues/new) or reach out on [Riot](https://riot.im/app/#/room/!HzySYSaIhtyWrwiwEV:matrix.org).
