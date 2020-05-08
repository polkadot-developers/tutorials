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
  --unsafe-rpc-expose \
  --name MyNode01
```

Here are some differences from when we launched as Alice.

- I've omitted the `--alice` flag. Instead we will insert our own custom keys into the keystore
  through the RPC shortly.
- The `--chain` flag has changed to use our custom chain spec.
- I've added the optional `--name` flag. You may use it to give your node a human-readable name in
  the telemetry UI.
- The optional `--unsafe-rpc-expose` flag has been added. As the name indicates, this flag is not
  safe to use in a production setting, but it allows this tutorial to stay focused on the topic at
  hand. In production, you should use a
  [JSON-RPC proxy](/kb/getting-started/glossary#json-rpc-proxy-crate), but that topic is out of the
  scope of this tutorial.

You should see the console outputs something as follows:

```bash
2020-05-07 09:07:45 Substrate Node
2020-05-07 09:07:45 ✌️  version 2.0.0-alpha.7-e7f3167-x86_64-linux-gnu
2020-05-07 09:07:45 ❤️  by Anonymous, 2017-2020
2020-05-07 09:07:45 📋 Chain specification: Local Testnet
2020-05-07 09:07:45 🏷  Node name: MyNode01
2020-05-07 09:07:45 👤 Role: AUTHORITY
2020-05-07 09:07:45 💾 Database: RocksDb at /tmp/node01/chains/local_testnet/db
2020-05-07 09:07:45 ⛓  Native runtime: node-template-1 (node-template-1.tx1.au1)
2020-05-07 09:07:45 🔨 Initializing Genesis block/state (state: 0x7589…693e, header-hash: 0xcc31…39ca)
2020-05-07 09:07:45 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.
2020-05-07 09:07:45 ⏱  Loaded block-time = 6000 milliseconds from genesis on first-launch
2020-05-07 09:07:45 📦 Highest known block at #0
2020-05-07 09:07:45 Using default protocol ID "sup" because none is configured in the chain specs
2020-05-07 09:07:45 🏷  Local node identity is: QmNdJYf7Ed5wPMMuM1ZiLviXWkdW1Qfeemw7AytqxGQFFP
2020-05-07 09:07:45 〽️ Prometheus server started at 127.0.0.1:9615
2020-05-07 09:07:50 💤 Idle (0 peers), best: #0 (0xcc31…39ca), finalized #0 (0xcc31…39ca), ⬇ 0 ⬆ 0
2020-05-07 09:07:55 💤 Idle (0 peers), best: #0 (0xcc31…39ca), finalized #0 (0xcc31…39ca), ⬇ 0 ⬆ 0
2020-05-07 09:08:00 💤 Idle (0 peers), best: #0 (0xcc31…39ca), finalized #0 (0xcc31…39ca), ⬇ 0 ⬆ 0

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
  --unsafe-rpc-expose \
  --name MyNode02 \
  --bootnodes /ip4/<IP Address>/tcp/<Port>/p2p/<Peer ID>
```

As before, we specify another `base-path`, give it another `name`, and also specify this node as a
`validator`.

Once the second node is up, you should see them authoring:

```
2020-05-07 09:32:12 Substrate Node
2020-05-07 09:32:12 ✌️  version 2.0.0-alpha.7-e7f3167-x86_64-linux-gnu
2020-05-07 09:32:12 ❤️  by Anonymous, 2017-2020
2020-05-07 09:32:12 📋 Chain specification: Local Testnet
2020-05-07 09:32:12 🏷  Node name: MyNode02
2020-05-07 09:32:12 👤 Role: AUTHORITY
2020-05-07 09:32:12 💾 Database: RocksDb at /tmp/node02/chains/local_testnet/db
2020-05-07 09:32:12 ⛓  Native runtime: node-template-1 (node-template-1.tx1.au1)
2020-05-07 09:32:12 🔨 Initializing Genesis block/state (state: 0x7589…693e, header-hash: 0xcc31…39ca)
2020-05-07 09:32:12 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.
2020-05-07 09:32:12 ⏱  Loaded block-time = 6000 milliseconds from genesis on first-launch
2020-05-07 09:32:12 📦 Highest known block at #0
2020-05-07 09:32:12 Using default protocol ID "sup" because none is configured in the chain specs
2020-05-07 09:32:12 🏷  Local node identity is: QmaNeCVM1VhMgcULyVyxbXRwnZJ2MgYZJkyCNfj7dsHdWk
2020-05-07 09:32:13 🔍 Discovered new external address for our node: /ip4/127.0.0.1/tcp/30334/p2p/12D3KooW9qZUN5v5dQYEB6YQ7khJdDJLzZtkvtNxehPNcjpXcFY3
2020-05-07 09:32:17 💤 Idle (1 peers), best: #0 (0xcc31…39ca), finalized #0 (0xcc31…39ca), ⬇ 0.7kiB/s ⬆ 0.7kiB/s
2020-05-07 09:32:22 💤 Idle (1 peers), best: #0 (0xcc31…39ca), finalized #0 (0xcc31…39ca), ⬇ 56 B/s ⬆ 65 B/s
2020-05-07 09:32:24 ✨ Imported #1 (0xbd47…1412)
2020-05-07 09:32:27 💤 Idle (1 peers), best: #1 (0xbd47…1412), finalized #0 (0xcc31…39ca), ⬇ 0.1kiB/s ⬆ 77 B/s
```

The final lines shows that your node has peered with another (**`1 peers`**), and they have produced
a block (**`best: #1 (0xbd47…1412)`**).

Now you're ready to add keys to its keystore by following the process (in the previous section) just
like you did for the first node.

> If you're inserting keys with the UI, you must connect the UI to the second node's WebSocket
> endpoint before inserting the second node's keys.

> Reminder: All validators must be using identical chain specifications in order to peer. You should
> see the same genesis block and state root hashes.

You will notice that even after you add the keys for the second node no block finalization has
happened (**`finalized #0 (0xcc31…39ca)`**). Substrate nodes require a restart after inserting a
grandpa key. Kill your nodes and restart them with the same commands you used previously. Now blocks
should be finalized.

```
2020-05-07 09:42:00 Substrate Node
2020-05-07 09:42:00 ✌️  version 2.0.0-alpha.7-e7f3167-x86_64-linux-gnu
2020-05-07 09:42:00 ❤️  by Anonymous, 2017-2020
2020-05-07 09:42:00 📋 Chain specification: Local Testnet
2020-05-07 09:42:00 🏷  Node name: MyNode02
2020-05-07 09:42:00 👤 Role: AUTHORITY
2020-05-07 09:42:00 💾 Database: RocksDb at /tmp/node02/chains/local_testnet/db
2020-05-07 09:42:00 ⛓  Native runtime: node-template-1 (node-template-1.tx1.au1)
2020-05-07 09:42:00 📦 Highest known block at #64
2020-05-07 09:42:00 Using default protocol ID "sup" because none is configured in the chain specs
2020-05-07 09:42:00 🏷  Local node identity is: QmaNeCVM1VhMgcULyVyxbXRwnZJ2MgYZJkyCNfj7dsHdWk
2020-05-07 09:42:01 🔍 Discovered new external address for our node: /ip4/127.0.0.1/tcp/30334/p2p/12D3KooW9qZUN5v5dQYEB6YQ7khJdDJLzZtkvtNxehPNcjpXcFY3
2020-05-07 09:42:01 🔍 Discovered new external address for our node: /ip4/192.168.0.120/tcp/30334/p2p/12D3KooW9qZUN5v5dQYEB6YQ7khJdDJLzZtkvtNxehPNcjpXcFY3
2020-05-07 09:42:06 🙌 Starting consensus session on top of parent 0xdcc80da2b9963a531cfb7c3205a5f405f36477542c6e11524d741cb743bfe7c8
2020-05-07 09:42:06 💤 Idle (1 peers), best: #64 (0xdcc8…e7c8), finalized #61 (0xdee8…7395), ⬇ 2.1kiB/s ⬆ 2.3kiB/s
2020-05-07 09:42:06 Timeout fired waiting for transaction pool to be ready. Proceeding to block production anyway.
2020-05-07 09:42:06 🎁 Prepared block for proposing at 65 [hash: 0x92b608ed1fc86e20244e20a8ffd91f810ad5d24359434220418e545a4c3a0583; parent_hash: 0xdcc8…e7c8; extrinsics (1): [0x2c6d…800b]]
2020-05-07 09:42:06 🔖 Pre-sealed block for proposal at 65. Hash now 0x178b067921b0803e46d3c1305bf8932c59946c017080aa173ed80713ea3a32a0, previously 0x92b608ed1fc86e20244e20a8ffd91f810ad5d24359434220418e545a4c3a0583.
2020-05-07 09:42:06 ✨ Imported #65 (0x178b…32a0)
2020-05-07 09:42:11 💤 Idle (1 peers), best: #65 (0x178b…32a0), finalized #63 (0xd8d4…aa81), ⬇ 1.1kiB/s ⬆ 1.2kiB/s
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
