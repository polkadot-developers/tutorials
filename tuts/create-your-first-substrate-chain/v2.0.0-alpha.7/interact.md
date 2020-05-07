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

2020-05-07 03:43:52 Running in --dev mode, RPC CORS has been disabled.
2020-05-07 03:43:52 Substrate Node
2020-05-07 03:43:52 ✌️  version 2.0.0-alpha.7-6c1d115-x86_64-linux-gnu
2020-05-07 03:43:52 ❤️  by Anonymous, 2017-2020
2020-05-07 03:43:52 📋 Chain specification: Development
2020-05-07 03:43:52 🏷  Node name: paltry-boy-8166
2020-05-07 03:43:52 👤 Role: AUTHORITY
2020-05-07 03:43:52 💾 Database: RocksDb at /home/dan/.local/share/node-template/chains/dev/db
2020-05-07 03:43:52 ⛓  Native runtime: node-template-1 (node-template-1.tx1.au1)
2020-05-07 03:43:52 🔨 Initializing Genesis block/state (state: 0x9589…5057, header-hash: 0x0cab…f8d4)
2020-05-07 03:43:52 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.
2020-05-07 03:43:52 ⏱  Loaded block-time = 6000 milliseconds from genesis on first-launch
2020-05-07 03:43:52 📦 Highest known block at #0
2020-05-07 03:43:52 Using default protocol ID "sup" because none is configured in the chain specs
2020-05-07 03:43:52 🏷  Local node identity is: QmfHEbdmVZHCBwKJFvczRt5owAzEbtF7Ao7oPQLvBq645c
2020-05-07 03:43:52 〽️ Prometheus server started at 127.0.0.1:9615
2020-05-07 03:43:54 🙌 Starting consensus session on top of parent 0x0cab0b39fa5d8acdf24e9572544ec7a1d06d37a733ad69e75345fa7ac35ff8d4
2020-05-07 03:43:54 🎁 Prepared block for proposing at 1 [hash: 0xf1c9d2b7e1ff6700fc37a58241f61a4dab6e02519c631f33d993378b1769f083; parent_hash: 0x0cab…f8d4; extrinsics (1): [0xce73…6b9f]]
2020-05-07 03:43:54 🔖 Pre-sealed block for proposal at 1. Hash now 0xa8b6ea89bfff3e0ba7a105de396d0120a72f4b03c4910afc1789a9775a7633d8, previously 0xf1c9d2b7e1ff6700fc37a58241f61a4dab6e02519c631f33d993378b1769f083.
2020-05-07 03:43:54 ✨ Imported #1 (0xa8b6…33d8)
2020-05-07 03:43:57 💤 Idle (0 peers), best: #1 (0xa8b6…33d8), finalized #0 (0x0cab…f8d4), ⬇ 0 ⬆ 0
2020-05-07 03:44:00 🙌 Starting consensus session on top of parent 0xa8b6ea89bfff3e0ba7a105de396d0120a72f4b03c4910afc1789a9775a7633d8
2020-05-07 03:44:00 🎁 Prepared block for proposing at 2 [hash: 0xd788901fe9a3c6d396bd9547c4c4e3c97269790bb66c20d450989d6a7bd9c83e; parent_hash: 0xa8b6…33d8; extrinsics (1): [0x8501…c337]]
2020-05-07 03:44:00 🔖 Pre-sealed block for proposal at 2. Hash now 0x6be12309179ec9135ae7c822ac3d2899a80acf93e4f77a2c0eabb00e405adb59, previously 0xd788901fe9a3c6d396bd9547c4c4e3c97269790bb66c20d450989d6a7bd9c83e.
2020-05-07 03:44:00 ✨ Imported #2 (0x6be1…db59)
2020-05-07 03:44:02 💤 Idle (0 peers), best: #2 (0x6be1…db59), finalized #0 (0x0cab…f8d4), ⬇ 0 ⬆ 0
```

If the number after `best:` is increasing, your blockchain is producing new blocks!

## Start the Front End

To interact with the local node we will use the Polkadot-js Apps user interface, often known as
"Apps" for short. Despite the name, Apps will work with any Substrate-based blockchain including
ours, not just Polkadot.

In your web browser, navigate to
[https://polkadot.js.org/apps](https://polkadot.js.org/apps/#/settings?rpc=ws://127.0.0.1:9944).

On the `Settings` tab ensure that you are connected to a `Local Node` or `ws://127.0.0.1:9944`.

> Some browsers, notably Firefox, will not connect to a local node from an https website. An easy
> work around is to try another browser, like Chromium. Another option is to
> [host this interface locally](https://github.com/polkadot-js/apps#development).

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
yet, but you can send some from Alice or any other pre-funded account. Only you (and your browser)
will know the private key for your own account which means nobody can transfer those tokens except
you.

## Next Steps

This is the end of your journey to launching your first blockchain with Substrate.

You have launched a working Substrate-based blockchain, attached a user interface to that chain, and
made token transfers among users. We hope you'll continue learning about Substrate.

Your next step may be:

- Decentralize your network with more nodes in the
  [Start a Private Network](/tutorials/start-a-private-network/v2.0.0-alpha.7) tutorial.
- Add custom functionality in the [Build a dApp](/tutorials/build-a-dapp/v2.0.0-alpha.7) tutorial.

If you experienced any issues with this tutorial or want to provide feedback, feel free to
[open a GitHub issue](https://github.com/substrate-developer-hub/tutorials/issues/new) or reach out
on [Riot](https://riot.im/app/#/room/!HzySYSaIhtyWrwiwEV:matrix.org).
