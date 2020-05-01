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
  --name MyNode01
```

Here are some differences from when we launched as Alice.

- I've omitted the `--alice` flag. Instead we will insert our own custom keys into the keystore
  through the RPC shortly.
- The `--chain` flag has changed to use our custom chain spec.
- I've added the optional `--name` flag. You may use it to give your node a human-readable name in
  the telemetry UI.

You should see the console outputs something as follows:

```bash
2020-01-10 16:50:24 Substrate Node
2020-01-10 16:50:24   version 2.0.0-alpha.5-2ddc70d-x86_64-linux-gnu
2020-01-10 16:50:24   by Anonymous, 2017, 2018
2020-01-10 16:50:24 Chain specification: Local Testnet
2020-01-10 16:50:24 Node name: MyNode01
2020-01-10 16:50:24 Roles: AUTHORITY
2020-01-10 16:50:25 Initializing Genesis block/state (state: 0x4408…ff44, header-hash: 0x7268…81b7)
2020-01-10 16:50:25 Loading GRANDPA authority set from genesis on what appears to be first startup.
2020-01-10 16:50:25 Loaded block-time = 6000 milliseconds from genesis on first-launch
2020-01-10 16:50:25 Highest known block at #0
2020-01-10 16:50:25 Using default protocol ID "sup" because none is configured in the chain specs
2020-01-10 16:50:25 Local node identity is: QmXPbznQUxJWXa4NwtVyLHtVtXnYH2h7utCH4LDN2sPXK1
2020-01-10 16:50:25 Grafana data source server started at 127.0.0.1:9955
2020-01-10 16:50:30 Idle (0 peers), best: #0 (0x7268…81b7), finalized #0 (0x7268…81b7), ⬇ 0.7kiB/s ⬆ 0.7kiB/s
2020-01-10 16:50:35 Idle (0 peers), best: #0 (0x7268…81b7), finalized #0 (0x7268…81b7), ⬇ 0.2kiB/s ⬆ 0.2kiB/s
2020-01-10 16:50:40 Idle (0 peers), best: #0 (0x7268…81b7), finalized #0 (0x7268…81b7), ⬇ 0.2kiB/s ⬆ 0.2kiB/s
```

## Add Keys to Keystore

Once your node is running, you will again notice that no blocks are being produced. At this point,
you need to add your keys into the keystore. Remember you will need to complete these steps for each
node in your network.

### Option 1: Using Polkadot-JS App UI

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

### Option 2: Using CLI

You can also insert a key to the keystore by command line.

```bash
# Submit a new key via RPC, connect to where your `rpc-port` is listening
$ curl http://localhost:9933 -H "Content-Type:application/json;charset=utf-8" -d \
  '{
    "jsonrpc":"2.0",
    "id":1,
    "method":"author_insertKey",
    "params": [
      "<aura/gran>",
      "<mnemonic phrase>",
      "<public key>"
    ]
  }'
```

If you enter the command and parameters correctly, the node will return a JSON response as follows.

```json
{ "jsonrpc": "2.0", "result": null, "id": 1 }
```

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
  --name MyNode02 \
  --bootnodes /ip4/<IP Address>/tcp/<Port>/p2p/<Peer ID>
```

As before, we specify another `base-path`, give it another `name`, and also specify this node as a
`validator`.

Once the node is up, you're ready to add keys to its keystore by following the process (in the
previous section) just like you did for the first node.

> If you're inserting keys with the UI, you must connect the UI to the second node's WebSocket
> endpoint before inserting the second node's keys.

> Reminder: All validators must be using identical chain specifications in order to peer. You should
> see the same genesis block and state root hashes.

Once both nodes have their keys, you should see them authoring

```
...
2020-01-10 14:30:28 Idle (0 peers), best: #0 (0x05e7…019e), finalized #0 (0x05e7…019e), ⬇ 0 ⬆ 0
2020-01-10 14:30:30 Discovered new external address for our node: /ip4/127.0.0.1/tcp/30333/p2p/QmZbVTa3n3CJUDMYCDqGPpDGwMmQJBkyGc5gRGkmYw88Pp
2020-01-10 14:30:33 Idle (2 peers), best: #0 (0x05e7…019e), finalized #0 (0x05e7…019e), ⬇ 0.7kiB/s ⬆ 0.7kiB/s
2020-01-10 14:30:36 Starting consensus session on top of parent 0x05e71d9c5e414bbe6f72e3b045eebfb5d9af22d59070bc29de4084544cef019e
2020-01-10 14:30:36 Prepared block for proposing at 1 [hash: 0xbeba14c5c8ad5ffa91a007c2b84554f3e74151b87444f389d719fff60aecd616; parent_hash: 0x05e7…019e; extrinsics: [0xe4ff…6bd7]]
2020-01-10 14:30:36 Pre-sealed block for proposal at 1. Hash now 0x1368112db340f6470a2edec9d8fb52031c8c63af4dc1ed86116ad048da93fe12, previously 0xbeba14c5c8ad5ffa91a007c2b84554f3e74151b87444f389d719fff60aecd616.
2020-01-10 14:30:36 Imported #1 (0x1368…fe12)
2020-01-10 14:30:38 Idle (3 peers), best: #1 (0x1368…fe12), finalized #0 (0x05e7…019e), ⬇ 0.6kiB/s ⬆ 0.7kiB/s
2020-01-10 14:30:42 Imported #2 (0xe4ee…8863)
2020-01-10 14:30:43 Idle (3 peers), best: #2 (0xe4ee…8863), finalized #1 (0x05e7…019e), ⬇ 0.8kiB/s ⬆ 0.7kiB/s
2020-01-10 14:30:48 Starting consensus session on top of parent 0xe4ee9d721d1b179ba83bc253871c504f48a0d4d02f304acfdccd29bfb1798863
2020-01-10 14:30:48 Prepared block for proposing at 3 [hash: 0x83f211e9e2d24101acf8097d46c6d348308a4a354fd0d60a7768ad51e4b0cc6b; parent_hash: 0xe4ee…8863; extrinsics: [0x81fa…3ab1]]
2020-01-10 14:30:48 Pre-sealed block for proposal at 3. Hash now 0x42f2fcfdb65c9e26700ef779a0782f6de30cb9531f8de54043e1a3b3469a0aff, previously 0x83f211e9e2d24101acf8097d46c6d348308a4a354fd0d60a7768ad51e4b0cc6b.
2020-01-10 14:30:48 Imported #3 (0x42f2…0aff)
2020-01-10 14:30:48 Idle (3 peers), best: #3 (0x42f2…0aff), finalized #2 (0x1368…fe12), ⬇ 1.0kiB/s ⬆ 0.9kiB/s
...
```

This line shows that your node has peered with another (**`1 peers`**), and they have produced a
block (**`best: #1 (0x1368…fe12)`**).

You may notice that no block finalization has happened yet (**`finalized #0 (0xf437…e7fe)`**).
Substrate nodes require a restart after inserting a grandpa key. Kill your nodes and restart them
with the same commands you used previously. Now blocks should be finalized.

## You're Finished

Congratulations! You've started your own blockchain!

In this tutorial you've learned to generate your own keypairs, create a custom chain spec that uses
those keypairs, and start a private network based on your custom chain spec.

### Learn More

That big Wasm blob we encountered in the chain spec was was necessary to enable forkless upgrades.
Learn more about how the [executor](/kb/advanced/executor) uses on-chain Wasm.
