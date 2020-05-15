---
slug: customchain
lang: en
title: Launch Your Private Network
---

With you custom chain spec created and distributed to all participants, you're ready to launch your
own custom chain. In this section it is no longer required to use a single physical machine or a
single binary.

## First Participant Starts a Bootnode

You've completed all the necessary prep work and you're now ready to launch your chain. This process
is very similar to when you launched a chain earlier as Alice and Bob. It's important to start with
a clean base path, so if you plan to use the same path that you've used previously, please delete
all contents from that directory.

The first participant can launch her node with

```bash
./target/release/node-template \
  --base-path /tmp/node01 \
  --chain=./customSpecRaw.json \
  --port 30333 \
  --ws-port 9944 \
  --rpc-port 9933 \
  --telemetry-url 'ws://telemetry.polkadot.io:1024 0' \
  --validator \
  --rpc-methods=Unsafe \
  --name MyNode01
```

Here are some differences from when we launched as Alice.

- I've omitted the `--alice` flag. Instead we will insert our own custom keys into the keystore
  through the RPC shortly.
- The `--chain` flag has changed to use our custom chain spec.
- I've added the optional `--name` flag. You may use it to give your node a human-readable name in
  the telemetry UI.
- The optional `--rpc-methods=Unsafe` flag has been added. As the name indicates, this flag is not
  safe to use in a production setting, but it allows this tutorial to stay focused on the topic at
  hand. In production, you should use a
  [JSON-RPC proxy](/kb/getting-started/glossary#json-rpc-proxy-crate), but that topic is out of the
  scope of this tutorial.

You should see the console outputs something as follows:

```bash
2020-05-15 16:09:11 Substrate Node
2020-05-15 16:09:11 ✌️  version 2.0.0-alpha.8-405717b-x86_64-linux-gnu
2020-05-15 16:09:11 ❤️  by Substrate DevHub <https://github.com/substrate-developer-hub>, 2017-2020
2020-05-15 16:09:11 📋 Chain specification: Local Testnet
2020-05-15 16:09:11 🏷  Node name: MyNode01
2020-05-15 16:09:11 👤 Role: AUTHORITY
2020-05-15 16:09:11 💾 Database: RocksDb at /tmp/node01/chains/local_testnet/db
2020-05-15 16:09:11 ⛓  Native runtime: node-template-1 (node-template-1.tx1.au1)
2020-05-15 16:09:11 🔨 Initializing Genesis block/state (state: 0x248c…760b, header-hash: 0x3596…bbc9)
2020-05-15 16:09:11 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.
2020-05-15 16:09:11 ⏱  Loaded block-time = 6000 milliseconds from genesis on first-launch
2020-05-15 16:09:11 📦 Highest known block at #0
2020-05-15 16:09:11 Using default protocol ID "sup" because none is configured in the chain specs
2020-05-15 16:09:11 🏷  Local node identity is: QmUy8Mpwiwh89ap66xwRK8BT2MdKr4575pwnZHuP22SsNT
2020-05-15 16:09:11 〽️ Prometheus server started at 127.0.0.1:9615
2020-05-15 16:09:16 💤 Idle (0 peers), best: #0 (0x3596…bbc9), finalized #0 (0x3596…bbc9), ⬇ 0 ⬆ 0
2020-05-15 16:09:21 💤 Idle (0 peers), best: #0 (0x3596…bbc9), finalized #0 (0x3596…bbc9), ⬇ 0 ⬆ 0
2020-05-15 16:09:26 💤 Idle (0 peers), best: #0 (0x3596…bbc9), finalized #0 (0x3596…bbc9), ⬇ 0 ⬆ 0
```

## Add Keys to Keystore

Once your node is running, you will again notice that no blocks are being produced. At this point,
you need to add your keys into the keystore. Remember you will need to complete these steps for each
node in your network.

### Add Keys with the Polkadot-JS App UI

You can use the Apps UI to insert your keys into the keystore. Navigate to the "Toolbox" tab and the
"RPC Call" sub-tab. Choose "author" and "insertKey". The fields can be filled like this:

![Inserting a Aura key using Apps](../assets/private-network-apps-insert-key-aura.png)

```
keytype: aura
suri: <your mnemonic phrase> (eg. clip organ olive upper oak void inject side suit toilet stick narrow)
publicKey: <your raw sr25519 key> (eg. 0x9effc1668ca381c242885516ec9fa2b19c67b6684c02a8a3237b6862e5c8cd7e)
```

> If you generated your keys with the Apps UI you will not know your raw public key. In this case
> you may use your SS58 address (`5FfBQ3kwXrbdyoqLPvcXRp7ikWydXawpNs2Ceu3WwFdhZ8W4`) instead.

You've now successfully inserted your **aura** key. You can repeat those steps to insert your
**grandpa** key (the **ed25519** key)

![Inserting a Grandpa key using Apps](../assets/private-network-apps-insert-key.png)

```
keytype: gran
suri: <your mnemonic phrase> (eg. clip organ olive upper oak void inject side suit toilet stick narrow)
publicKey: <your raw ed25519 key> (eg. 0xb48004c6e1625282313b07d1c9950935e86894a2e4f21fb1ffee9854d180c781)
```

> If you generated your keys with the Apps UI you will not know your raw public key. In this case
> you may use your SS58 address (`5G9NWJ5P9uk7am24yCKeLZJqXWW6hjuMyRJDmw4ofqxG8Js2`) instead.

> If you are following these steps for the _second_ node in the network, you must connect the UI to
> the second node before inserting the keys.

## Subsequent Participants Join

Subsequent validators can now join the network. This can be done by specifying the `--bootnodes`
paramter as Bob did previously.

```bash
./target/release/node-template \
  --base-path /tmp/node02 \
  --chain ./customSpecRaw.json \
  --port 30334 \
  --ws-port 9945 \
  --rpc-port 9934 \
  --telemetry-url 'ws://telemetry.polkadot.io:1024 0' \
  --validator \
  --rpc-methods=Unsafe \
  --name MyNode02 \
  --bootnodes /ip4/<IP Address>/tcp/<Port>/p2p/<Peer ID>
```

As before, we specify another `base-path`, give it another `name`, and also specify this node as a
`validator`.

Once the second node is up, you should see them authoring:

```
2020-05-15 16:12:07 Substrate Node
2020-05-15 16:12:07 ✌️  version 2.0.0-alpha.8-405717b-x86_64-linux-gnu
2020-05-15 16:12:07 ❤️  by Substrate DevHub <https://github.com/substrate-developer-hub>, 2017-2020
2020-05-15 16:12:07 📋 Chain specification: Local Testnet
2020-05-15 16:12:07 🏷  Node name: MyNode02
2020-05-15 16:12:07 👤 Role: AUTHORITY
2020-05-15 16:12:07 💾 Database: RocksDb at /tmp/node02/chains/local_testnet/db
2020-05-15 16:12:07 ⛓  Native runtime: node-template-1 (node-template-1.tx1.au1)
2020-05-15 16:12:07 🔨 Initializing Genesis block/state (state: 0x248c…760b, header-hash: 0x3596…bbc9)
2020-05-15 16:12:07 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.
2020-05-15 16:12:07 ⏱  Loaded block-time = 6000 milliseconds from genesis on first-launch
2020-05-15 16:12:07 📦 Highest known block at #0
2020-05-15 16:12:07 Using default protocol ID "sup" because none is configured in the chain specs
2020-05-15 16:12:07 🏷  Local node identity is: QmVLjNfifpgz1NjBgVSrpKEqGsTZ5XfUjjCd6g3Wv2BN3d
2020-05-15 16:12:08 🔍 Discovered new external address for our node: /ip4/127.0.0.1/tcp/30334/p2p/12D3KooWSqkyaaQv2mPezCMoDiCquD4ui9qoGvBFVJkHsKjkmMdC
2020-05-15 16:12:12 🙌 Starting consensus session on top of parent 0x35961ac2b8d4c62a1aeb83f4f6ceb12c4db73b816f9c64330eea07979095bbc9
2020-05-15 16:12:12 🎁 Prepared block for proposing at 1 [hash: 0xb0a018360c2ea3f7d69c5e0965d66f72ec82fa3a0a6656b23d8566713c027665; parent_hash: 0x3596…bbc9; extrinsics (1): [0x15d9…e653]]
2020-05-15 16:12:12 Encountered consensus error: CannotSign([254, 40, 20, 241, 117, 216, 125, 217, 163, 123, 173, 42, 164, 116, 3, 0, 28, 120, 208, 207, 172, 81, 210, 127, 78, 172, 243, 157, 254, 169, 245, 121], "An unknown keystore error occurred: No such file or directory (os error 2)")
2020-05-15 16:12:12 ✨ Imported #1 (0x26be…df99)
2020-05-15 16:12:12 💤 Idle (1 peers), best: #1 (0x26be…df99), finalized #0 (0x3596…bbc9), ⬇ 0.7kiB/s ⬆ 0.6kiB/s
```

The final lines shows that your node has peered with another (**`1 peers`**), and they have produced
a block (**`best: #1 (0x26be…df99)`**).

Now you're ready to add keys to its keystore by following the process (in the previous section) just
like you did for the first node.

> If you're inserting keys with the UI, you must connect the UI to the second node's WebSocket
> endpoint before inserting the second node's keys.

> Reminder: All validators must be using identical chain specifications in order to peer. You should
> see the same genesis block and state root hashes.

You will notice that even after you add the keys for the second node no block finalization has
happened (**`finalized #0 (0x3596…bbc9)`**). Substrate nodes require a restart after inserting a
grandpa key. Kill your nodes and restart them with the same commands you used previously. Now blocks
should be finalized.

```
2020-05-15 16:15:27 Substrate Node
2020-05-15 16:15:27 ✌️  version 2.0.0-alpha.8-405717b-x86_64-linux-gnu
2020-05-15 16:15:27 ❤️  by Substrate DevHub <https://github.com/substrate-developer-hub>, 2017-2020
2020-05-15 16:15:27 📋 Chain specification: Local Testnet
2020-05-15 16:15:27 🏷  Node name: MyNode02
2020-05-15 16:15:27 👤 Role: AUTHORITY
2020-05-15 16:15:27 💾 Database: RocksDb at /tmp/node02/chains/local_testnet/db
2020-05-15 16:15:27 ⛓  Native runtime: node-template-1 (node-template-1.tx1.au1)
2020-05-15 16:15:27 📦 Highest known block at #18
2020-05-15 16:15:27 Using default protocol ID "sup" because none is configured in the chain specs
2020-05-15 16:15:27 🏷  Local node identity is: QmVLjNfifpgz1NjBgVSrpKEqGsTZ5XfUjjCd6g3Wv2BN3d
2020-05-15 16:15:30 🔍 Discovered new external address for our node: /ip4/127.0.0.1/tcp/30334/p2p/12D3KooWSqkyaaQv2mPezCMoDiCquD4ui9qoGvBFVJkHsKjkmMdC
2020-05-15 16:15:32 💤 Idle (1 peers), best: #18 (0x6bcd…501d), finalized #16 (0xa3f1…7988), ⬇ 1.5kiB/s ⬆ 1.6kiB/s
2020-05-15 16:15:36 🙌 Starting consensus session on top of parent 0x6bcda46690c678a8f18cf343e499b35ddc59f5bf4d735082bb5fd866a693501d
2020-05-15 16:15:36 Timeout fired waiting for transaction pool to be ready. Proceeding to block production anyway.
2020-05-15 16:15:36 🎁 Prepared block for proposing at 19 [hash: 0x52fcbd882a3a393990d79e837839e2d449ceaf029fb6c31b514dba55f21f7382; parent_hash: 0x6bcd…501d; extrinsics (1): [0x79d4…050c]]
2020-05-15 16:15:36 Encountered consensus error: CannotSign([254, 40, 20, 241, 117, 216, 125, 217, 163, 123, 173, 42, 164, 116, 3, 0, 28, 120, 208, 207, 172, 81, 210, 127, 78, 172, 243, 157, 254, 169, 245, 121], "An unknown keystore error occurred: No such file or directory (os error 2)")
2020-05-15 16:15:36 ✨ Imported #19 (0xd4f3…a7a0)
2020-05-15 16:15:37 💤 Idle (1 peers), best: #19 (0xd4f3…a7a0), finalized #16 (0xa3f1…7988), ⬇ 1.1kiB/s ⬆ 1.1kiB/s
```

## You're Finished

Congratulations! You've started your own blockchain!

In this tutorial you've learned to generate your own keypairs, create a custom chain spec that uses
those keypairs, and start a private network based on your custom chain spec.

<!-- TODO link to the followup tutorial about starting a 3 node network using the demo substrate node
Details in https://github.com/substrate-developer-hub/tutorials/issues/16-->

### Learn More

That big Wasm blob we encountered in the chain spec was was necessary to enable forkless upgrades.
Learn more about how the [executor](/kb/advanced/executor) uses on-chain Wasm.
