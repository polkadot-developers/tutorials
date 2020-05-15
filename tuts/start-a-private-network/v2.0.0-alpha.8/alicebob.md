---
slug: alicebob
lang: en
title: Alice and Bob Start Blockchain
---

Before we generate our own keys, and start a truly unique Substrate network, let's learn the
fundamentals by starting with a pre-defined network specification called `local` with two
pre-defined (and definitely not private!) keys known as Alice and Bob.

> This portion of the tutorial should be run on a single workstation with a single Substrate binary.
> If you've followed the tutorial up to this point, you have the correct setup.

## Alice Starts First

Alice (or whomever is playing her) should run these commands from node-template repository root.

> Here we've explicitly shown the `purge-chain` command. In the future we will omit this You should
> purge old chain data any time you're trying to start a new network.

```bash
# Purge any chain data from previous runs
# You will be prompted to type `y`
./target/release/node-template purge-chain --base-path /tmp/alice --chain local
```

```bash
# Start Alice's node
./target/release/node-template \
  --base-path /tmp/alice \
  --chain local \
  --alice \
  --port 30333 \
  --ws-port 9944 \
  --rpc-port 9933 \
  --telemetry-url 'ws://telemetry.polkadot.io:1024 0' \
  --validator
```

Let's look at those flags in detail:

| Flags             | Descriptions                                                                                                                                                                                                                                                                                                                               |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `--base-path`     | Specifies a directory where Substrate should store all the data related to this chain. If the directory does not exist it will be created for you. If other blockchain data already exists there you will get an error. Either clear the directory or choose a different one. If this value is not specified, a default path will be used. |
| `--chain local`   | Specifies which chain specification to use. There are a few pre-packaged options including `local`, `development`, and `staging` but generally one specifies their own chainspec file. We'll specify our own file in a later step.                                                                                                         |
| `--alice`         | Puts the pre-defined Alice keys (both for block production and finalization) in the node's keystore. Generally one should generate their own keys and insert them with an RPC call. We'll generate our own keys in a later step. This flag also makes Alice a validator.                                                                   |
| `--port 30333`    | Specifies the port that your node will listen for p2p traffic on. `30333` is the default and this flag can be omitted if you're happy with the default. If Bob's node will run on the same physical system, you will need to explicitly specify a different port for it.                                                                   |
| `--ws-port 9944`  | Specifies the port that your node will listen for incoming web socket traffic on. `9944` is the default, so it can also be omitted.                                                                                                                                                                                                        |
| `--rpc-port 9933` | Specifies the port that your node will listen for incoming RPC traffic on. `9933` is the default, so it can also be omitted.                                                                                                                                                                                                               |
| `--telemetry-url` | Tells the node to send telemetry data to a particular server. The one we've chosen here is hosted by Parity and is available for anyone to use. You may also host your own (beyond the scope of this article) or omit this flag entirely.                                                                                                  |
| `--validator`     | Means that we want to participate in block production and finalization rather than just sync the network.                                                                                                                                                                                                                                  |

When the node starts you should see output similar to this.

```
2020-05-07 07:17:47 Substrate Node
2020-05-07 07:17:47 ✌️  version 2.0.0-alpha.7-e7f3167-x86_64-linux-gnu
2020-05-07 07:17:47 ❤️  by Anonymous, 2017-2020
2020-05-07 07:17:47 📋 Chain specification: Local Testnet
2020-05-07 07:17:47 🏷  Node name: Alice
2020-05-07 07:17:47 👤 Role: AUTHORITY
2020-05-07 07:17:47 💾 Database: RocksDb at /tmp/alice/chains/local_testnet/db
2020-05-07 07:17:47 ⛓  Native runtime: node-template-1 (node-template-1.tx1.au1)
2020-05-07 07:17:47 🔨 Initializing Genesis block/state (state: 0x8035…184f, header-hash: 0x00ff…211f)
2020-05-07 07:17:47 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.
2020-05-07 07:17:47 ⏱  Loaded block-time = 6000 milliseconds from genesis on first-launch
2020-05-07 07:17:47 📦 Highest known block at #0
2020-05-07 07:17:47 Using default protocol ID "sup" because none is configured in the chain specs
2020-05-07 07:17:47 🏷  Local node identity is: QmRBS1wLLS5u7MVGxJTFGJHAfiSWqh2CgaLZ67D4Pait2T
2020-05-07 07:17:47 〽️ Prometheus server started at 127.0.0.1:9615
2020-05-07 07:17:52 💤 Idle (0 peers), best: #0 (0x00ff…211f), finalized #0 (0x00ff…211f), ⬇ 0 ⬆ 0
2020-05-07 07:17:57 💤 Idle (0 peers), best: #0 (0x00ff…211f), finalized #0 (0x00ff…211f), ⬇ 0 ⬆ 0
...
```

> **Notes**
>
> - `🔨 Initializing Genesis block/state (state: 0x8035…184f, header-hash: 0x00ff…211f)` tells which
>   genesis block the node is using. When you start the next node, verify that these values are
>   equal.
> - `🏷 Local node identity is: QmRBS1wLLS5u7MVGxJTFGJHAfiSWqh2CgaLZ67D4Pait2T` shows the Peer ID
>   that Bob will need when booting from Alice's node.

You'll notice that no blocks are being produced yet. Blocks will start being produced once another
node joins the network.

More details about all of these flags and others that I haven't mentioned are available by running
`./target/release/node-template --help`.

## Attach a UI

You can tell a lot about your node by watching the output it produces in your terminal. There is
also a nice graphical user interface known as the Polkadot-JS Apps, or just "Apps" for short.

In your web browser, navigate to
[https://polkadot.js.org/apps/#/settings?rpc=ws://127.0.0.1:9944](https://polkadot.js.org/apps/#/settings?rpc=ws://127.0.0.1:9944).

> Some browsers, notably Firefox, will not connect to a local node from an https website. An easy
> work around is to try another browser, like Chromium. Another option is to
> [host this interface locally](https://github.com/polkadot-js/apps#development).

The link provided above includes the `rpc` URL parameter, which instructs the Apps UI to connect to
the URL that was provided as its value (in this case, your local node). To manually configure Apps
UI to connect to another node:

- Click on the top left network icon

  ![Top Left Network Icon](../assets/private-network-top-left-network-icon.png)

- A popup dropdown appears. Choose the last entry, which is a local node using default port 9944

  ![Select Network](../assets/private-network-select-network.png)

- To connect to a custom node and port, you just need to specify the endpoint by choosing
  `custom endpoint` and type in your own endpoint. In this way you can use a single instance of Apps
  UI to connect to various nodes.

  ![Custom Endpoint](../assets/private-network-custom-endpoint.png)

You should now see something like this.

![No blocks in polkadot-js-apps](../assets/private-network-no-blocks.png)

> **Notes**
>
> If you do not want to run your hosted version of Polkadot-JS Apps UI while connecting to Substrate
> node you have deployed remotely, you can configure ssh local port forwarding to forward local
> request to the `ws-port` listened by the remote host. This is beyond the scope of this tutorial
> but is referenced at the bottom.

## Bob Joins

Now that Alice's node is up and running, Bob can join the network by bootstrapping from her node.
His command will look very similar.

```bash
./target/release/node-template purge-chain --base-path /tmp/bob --chain local
```

```bash
./target/release/node-template \
  --base-path /tmp/bob \
  --chain local \
  --bob \
  --port 30334 \
  --ws-port 9945 \
  --rpc-port 9934 \
  --telemetry-url 'ws://telemetry.polkadot.io:1024 0' \
  --validator \
  --bootnodes /ip4/<Alices IP Address>/tcp/<Alices Port>/p2p/<Alices Peer ID>
```

Most of these options are already explained above, but there are a few points worth mentioning.

- Because these two nodes are running on the same physical machine, Bob must specify a different
  `--base-path`, `--port`, `--ws-port`, and `--rpc-port`.
- Bob has added the `--bootnodes` flag and specified a single boot node, namely Alice's. He must
  correctly specify these three pieces of information which Alice can supply for him.
  - Alice's IP Address, probably `127.0.0.1`
  - Alice's Port, probably `30333`
  - Alice's Peer ID, copied from her log output. (`QmRBS1wLLS5u7MVGxJTFGJHAfiSWqh2CgaLZ67D4Pait2T`
    in the example output above.)

If all is going well, after a few seconds, the nodes should peer together and start producing
blocks. You should see some lines like the following in the console that started Alice node.

```
...
2020-05-07 07:18:49 Substrate Node
2020-05-07 07:18:49 ✌️  version 2.0.0-alpha.7-e7f3167-x86_64-linux-gnu
2020-05-07 07:18:49 ❤️  by Anonymous, 2017-2020
2020-05-07 07:18:49 📋 Chain specification: Local Testnet
2020-05-07 07:18:49 🏷  Node name: Bob
2020-05-07 07:18:49 👤 Role: AUTHORITY
2020-05-07 07:18:49 💾 Database: RocksDb at /tmp/bob/chains/local_testnet/db
2020-05-07 07:18:49 ⛓  Native runtime: node-template-1 (node-template-1.tx1.au1)
2020-05-07 07:18:49 🔨 Initializing Genesis block/state (state: 0x8035…184f, header-hash: 0x00ff…211f)
2020-05-07 07:18:49 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.
2020-05-07 07:18:49 ⏱  Loaded block-time = 6000 milliseconds from genesis on first-launch
2020-05-07 07:18:49 📦 Highest known block at #0
2020-05-07 07:18:49 Using default protocol ID "sup" because none is configured in the chain specs
2020-05-07 07:18:49 🏷  Local node identity is: QmfWiCkdbC5iNDDRD2MN9N45CkXzK1ccRXSyn2PZzoL7GJ
2020-05-07 07:18:49 🔍 Discovered new external address for our node: /ip4/127.0.0.1/tcp/30334/p2p/12D3KooWEh7VzjW4XN9y6g665WdpSxY49okoFxWM4S6rF7uf5csN
2020-05-07 07:18:49 🔍 Discovered new external address for our node: /ip4/192.168.0.120/tcp/30334/p2p/12D3KooWEh7VzjW4XN9y6g665WdpSxY49okoFxWM4S6rF7uf5csN
2020-05-07 07:18:54 🙌 Starting consensus session on top of parent 0x00ff505992c79d9c9414f4f1d6ec9746b48a17e7b940b2b64c35e665f434211f
2020-05-07 07:18:54 🎁 Prepared block for proposing at 1 [hash: 0xc73bf53478daeb180581c3cde161ed2f68ee4fa948c6d16317e9062da998c06f; parent_hash: 0x00ff…211f; extrinsics (1): [0x8220…614f]]
2020-05-07 07:18:54 🔖 Pre-sealed block for proposal at 1. Hash now 0x945406768fb0d727ceb2a92c8a4a8bd8604dcb0794f67f87dde66d538a66dc8c, previously 0xc73bf53478daeb180581c3cde161ed2f68ee4fa948c6d16317e9062da998c06f.
2020-05-07 07:18:54 ✨ Imported #1 (0x9454…dc8c)
2020-05-07 07:18:54 💤 Idle (1 peers), best: #1 (0x9454…dc8c), finalized #0 (0x00ff…211f), ⬇ 1.9kiB/s ⬆ 1.9kiB/s
2020-05-07 07:18:59 💤 Idle (1 peers), best: #1 (0x9454…dc8c), finalized #0 (0x00ff…211f), ⬇ 0.6kiB/s ⬆ 0.6kiB/s
2020-05-07 07:19:00 ✨ Imported #2 (0xa49b…1bc8)
2020-05-07 07:19:04 💤 Idle (1 peers), best: #2 (0xa49b…1bc8), finalized #0 (0x00ff…211f), ⬇ 0.8kiB/s ⬆ 0.7kiB/s
2020-05-07 07:19:06 🙌 Starting consensus session on top of parent 0xa49b8afb4bd1449371675978f2ff0f8f990bbef22f4d98a450720cb2a2221bc8
2020-05-07 07:19:06 🎁 Prepared block for proposing at 3 [hash: 0x61985274e7bef49ae0083c7a1f24652ef8fddffbe0f58fd48cc68a8bdee82348; parent_hash: 0xa49b…1bc8; extrinsics (1): [0xc1c2…6936]]
2020-05-07 07:19:06 🔖 Pre-sealed block for proposal at 3. Hash now 0x53615ab6a7c3c866ebc812b24781fc772251becc9a321696e42b0cba9c44ab7a, previously 0x61985274e7bef49ae0083c7a1f24652ef8fddffbe0f58fd48cc68a8bdee82348.
2020-05-07 07:19:06 ✨ Imported #3 (0x5361…ab7a)
2020-05-07 07:19:09 💤 Idle (1 peers), best: #3 (0x5361…ab7a), finalized #1 (0x9454…dc8c), ⬇ 1.0kiB/s ⬆ 1.2kiB/s
...
```

These lines shows that Bob has peered with Alice (**`1 peers`**), they have produced some blocks
(**`best: #3 (0x5361…ab7a)`**), and blocks are being finalized (**`finalized #1 (0x9454…dc8c)`**).

Looking at the console that started Bob's node, you should see something similar.

Once you've verified that both nodes are running as expected, you can shut them down. The next
section of this tutorial will include commands to restart the nodes when necessary.

## References

- [Configure ssh local port forwarding](https://www.booleanworld.com/guide-ssh-port-forwarding-tunnelling/)
