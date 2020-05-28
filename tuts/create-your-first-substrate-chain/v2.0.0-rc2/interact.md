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

2020-05-15 14:20:12 Running in --dev mode, RPC CORS has been disabled.
2020-05-15 14:20:12 Substrate Node
2020-05-15 14:20:12 ✌️  version 2.0.0-alpha.8-4588f92-x86_64-linux-gnu
2020-05-15 14:20:12 ❤️  by Substrate DevHub <https://github.com/substrate-developer-hub>, 2017-2020
2020-05-15 14:20:12 📋 Chain specification: Development
2020-05-15 14:20:12 🏷  Node name: absorbing-disgust-9031
2020-05-15 14:20:12 👤 Role: AUTHORITY
2020-05-15 14:20:12 💾 Database: RocksDb at /home/dan/.local/share/node-template/chains/dev/db
2020-05-15 14:20:12 ⛓  Native runtime: node-template-1 (node-template-1.tx1.au1)
2020-05-15 14:20:12 🔨 Initializing Genesis block/state (state: 0xcb0d…ea04, header-hash: 0x7a0a…2d1a)
2020-05-15 14:20:12 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.
2020-05-15 14:20:12 ⏱  Loaded block-time = 6000 milliseconds from genesis on first-launch
2020-05-15 14:20:12 📦 Highest known block at #0
2020-05-15 14:20:12 Using default protocol ID "sup" because none is configured in the chain specs
2020-05-15 14:20:12 🏷  Local node identity is: QmfHEbdmVZHCBwKJFvczRt5owAzEbtF7Ao7oPQLvBq645c
2020-05-15 14:20:12 〽️ Prometheus server started at 127.0.0.1:9615
2020-05-15 14:20:17 💤 Idle (0 peers), best: #0 (0x7a0a…2d1a), finalized #0 (0x7a0a…2d1a), ⬇ 0 ⬆ 0
2020-05-15 14:20:18 🙌 Starting consensus session on top of parent 0x7a0a487a455519ea551b3e9b87982761f0612e0e95946e66b79c4b7ac3c22d1a
2020-05-15 14:20:18 🎁 Prepared block for proposing at 1 [hash: 0x0d9d9425352cdd06916572a6bddf920385e5d605f8ecbd3ffda3caea249ceead; parent_hash: 0x7a0a…2d1a; extrinsics (1): [0xdb72…dc32]]
2020-05-15 14:20:18 🔖 Pre-sealed block for proposal at 1. Hash now 0xe3f19ac504d7347378a526170d477849608c8fedae53cd7760ef0685a4f7365a, previously 0x0d9d9425352cdd06916572a6bddf920385e5d605f8ecbd3ffda3caea249ceead.
2020-05-15 14:20:18 ✨ Imported #1 (0xe3f1…365a)
2020-05-15 14:20:22 💤 Idle (0 peers), best: #1 (0xe3f1…365a), finalized #0 (0x7a0a…2d1a), ⬇ 0 ⬆ 0
2020-05-15 14:20:24 🙌 Starting consensus session on top of parent 0xe3f19ac504d7347378a526170d477849608c8fedae53cd7760ef0685a4f7365a
2020-05-15 14:20:24 🎁 Prepared block for proposing at 2 [hash: 0xf9449a1c18971c426ab21ba79770d883527e82eebf3321490d31caa3f5acec6c; parent_hash: 0xe3f1…365a; extrinsics (1): [0x8560…e04f]]
2020-05-15 14:20:24 🔖 Pre-sealed block for proposal at 2. Hash now 0x884a6aa7001905f7eaf0d0d21be52f93132f33d3eeac26b46a75f4014f9f302c, previously 0xf9449a1c18971c426ab21ba79770d883527e82eebf3321490d31caa3f5acec6c.
2020-05-15 14:20:24 ✨ Imported #2 (0x884a…302c)
2020-05-15 14:20:27 💤 Idle (0 peers), best: #2 (0x884a…302c), finalized #0 (0x7a0a…2d1a), ⬇ 0 ⬆ 0
2020-05-15 14:20:30 🙌 Starting consensus session on top of parent 0x884a6aa7001905f7eaf0d0d21be52f93132f33d3eeac26b46a75f4014f9f302c
2020-05-15 14:20:30 🎁 Prepared block for proposing at 3 [hash: 0xf6d4ce51755cc638d93f662c877a46bac58c2cea32748781393c1a9489f15aeb; parent_hash: 0x884a…302c; extrinsics (1): [0x300a…8635]]
2020-05-15 14:20:30 🔖 Pre-sealed block for proposal at 3. Hash now 0x012793b140237843a45c9228393a3318c0f7df027bbe2d16a4277259331c53d8, previously 0xf6d4ce51755cc638d93f662c877a46bac58c2cea32748781393c1a9489f15aeb.
2020-05-15 14:20:30 ✨ Imported #3 (0x0127…53d8)
2020-05-15 14:20:32 💤 Idle (0 peers), best: #3 (0x0127…53d8), finalized #1 (0xe3f1…365a), ⬇ 0 ⬆ 0
```

If the number after `finalized:` is increasing, your blockchain is producing new blocks and reaching
consensus about the state they describe!

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
  [Start a Private Network](/tutorials/start-a-private-network/v2.0.0-alpha.8) tutorial.
- Add custom functionality in the [Build a dApp](/tutorials/build-a-dapp/v2.0.0-alpha.8) tutorial.

If you experienced any issues with this tutorial or want to provide feedback, feel free to
[open a GitHub issue](https://github.com/substrate-developer-hub/tutorials/issues/new) or reach out
on [Riot](https://riot.im/app/#/room/!HzySYSaIhtyWrwiwEV:matrix.org).
