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
./target/release/node-template purge-chain

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

| <div style="min-width:110pt"> Flags </div> | Descriptions                                                                                                                                                                                                                                                                                                                               |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `--base-path`                              | Specifies a directory where Substrate should store all the data related to this chain. If the directory does not exist it will be created for you. If other blockchain data already exists there you will get an error. Either clear the directory or choose a different one. If this value is not specified, a default path will be used. |
| `--chain local`                            | Specifies which chain specification to use. There are a few pre-packaged options including `local`, `development`, and `staging` but generally one specifies their own chainspec file. We'll specify our own file in a later step.                                                                                                         |
| `--alice`                                  | Puts the pre-defined Alice keys (both for block production and finalization) in the node's keystore. Generally one should generate their own keys and insert them with an RPC call. We'll generate our own keys in a later step. This flag also makes Alice a validator.                                                                   |
| `--port 30333`                             | Specifies the port that your node will listen for p2p traffic on. `30333` is the default and this flag can be omitted if you're happy with the default. If Bob's node will run on the same physical system, you will need to explicitly specify a different port for it.                                                                   |
| `--ws-port 9944`                           | Specifies the port that your node will listen for incoming web socket traffic on. `9944` is the default, so it can also be omitted.                                                                                                                                                                                                        |
| `--rpc-port 9933`                          | Specifies the port that your node will listen for incoming RPC traffic on. `9933` is the default, so it can also be omitted.                                                                                                                                                                                                               |
| `--telemetry-url`                          | Tells the node to send telemetry data to a particular server. The one we've chosen here is hosted by Parity and is available for anyone to use. You may also host your own (beyond the scope of this article) or omit this flag entirely.                                                                                                  |
| `--validator`                              | Means that we want to participate in block production and finalization rather than just sync the network.                                                                                                                                                                                                                                  |

When the node starts you should see output similar to this.

```
2020-04-15 13:15:49 Substrate Node
2020-04-15 13:15:49 ✌️  version 2.0.0-alpha.6-c1b33f8-x86_64-linux-gnu
2020-04-15 13:15:49 ❤️  by Anonymous, 2017-2020
2020-04-15 13:15:49 📋 Chain specification: Local Testnet
2020-04-15 13:15:49 🏷  Node name: Alice
2020-04-15 13:15:49 👤 Role: AUTHORITY
2020-04-15 13:15:49 ⛓  Native runtime: node-template-1:1(node-template-1)
2020-04-15 13:15:50 🔨 Initializing Genesis block/state (state: 0x5ed3…5500, header-hash: 0x4377…90f6)
2020-04-15 13:15:50 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.
2020-04-15 13:15:50 ⏱ Loaded block-time = 6000 milliseconds from genesis on first-launch
2020-04-15 13:15:50 📦 Highest known block at #0
2020-04-15 13:15:50 Using default protocol ID "sup" because none is configured in the chain specs
2020-04-15 13:15:50 🏷  Local node identity is: QmSPiPNhFyDUEugSPM9dN2TbD9qzsA1xbiHkCeLsrPZ47m
2020-04-15 13:15:50 〽️ Prometheus server started at 127.0.0.1:9615
2020-04-15 13:15:55 💤 Idle (0 peers), best: #0 (0x4377…90f6), finalized #0 (0x4377…90f6), ⬇ 0.6kiB/s ⬆ 0.5kiB/s
2020-04-15 13:15:58 🔍 Discovered new external address for our node: /ip4/127.0.0.1/tcp/30333/p2p/12D3KooWDVn31np2cEsoNRKnMTgLfAF8L32qofNpua5Dk8qY6c6p
2020-04-15 13:16:00 💤 Idle (0 peers), best: #0 (0x4377…90f6), finalized #0 (0x4377…90f6), ⬇ 0.4kiB/s ⬆ 0.4kiB/s
2020-04-15 13:16:05 💤 Idle (0 peers), best: #0 (0x4377…90f6), finalized #0 (0x4377…90f6), ⬇ 0.3kiB/s ⬆ 0.2kiB/s

...
```

> **Notes**
>
> - `🔨 Initializing Genesis block/state (state: 0x5ed3…5500, header-hash: 0x4377…90f6)` tells which
>   genesis block the node is using. Bob's node must have the same hashes or they will not connect
>   to one another.
> - `🏷 Local node identity is: QmSPiPNhFyDUEugSPM9dN2TbD9qzsA1xbiHkCeLsrPZ47m` shows the Peer ID
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
  - Alice's Peer ID, copied from her log output. (`QmSPiPNhFyDUEugSPM9dN2TbD9qzsA1xbiHkCeLsrPZ47m`
    in the example output above.)

If all is going well, after a few seconds, the nodes should peer together and start producing
blocks. You should see some lines like the following in the console that started Alice node.

```
...
2020-04-15 13:30:00 💤 Idle (0 peers), best: #0 (0x4377…90f6), finalized #0 (0x4377…90f6), ⬇ 0.2kiB/s ⬆ 0.2kiB/s
2020-04-15 13:30:05 💤 Idle (0 peers), best: #0 (0x4377…90f6), finalized #0 (0x4377…90f6), ⬇ 0 ⬆ 0
2020-04-15 13:30:10 💤 Idle (1 peers), best: #0 (0x4377…90f6), finalized #0 (0x4377…90f6), ⬇ 1.1kiB/s ⬆ 1.1kiB/s
2020-04-15 13:30:12 🙌 Starting consensus session on top of parent 0x437725b769571fab5211ed117dd5934672f04667f1cbaf001511300ebb2a90f6
2020-04-15 13:30:12 🎁 Prepared block for proposing at 1 [hash: 0x878559ae53e2ab6a51a35e165a943b947d28bcf6afaca9845aa8d798f0fe7d7e; parent_hash: 0x4377…90f6; extrinsics (1): [0xe92e…528c]]
2020-04-15 13:30:12 🔖 Pre-sealed block for proposal at 1. Hash now 0xa78cd6979a20bacb8c99fa86c0d31d34e3ad9e4bceb61b0cb781e5b736aa882e, previously 0x878559ae53e2ab6a51a35e165a943b947d28bcf6afaca9845aa8d798f0fe7d7e.
2020-04-15 13:30:12 ✨ Imported #1 (0xa78c…882e)
2020-04-15 13:30:15 💤 Idle (1 peers), best: #1 (0xa78c…882e), finalized #0 (0x4377…90f6), ⬇ 0.6kiB/s ⬆ 0.7kiB/s
2020-04-15 13:30:18 ✨ Imported #2 (0x211a…cf39)
2020-04-15 13:30:20 💤 Idle (1 peers), best: #2 (0x211a…cf39), finalized #0 (0x4377…90f6), ⬇ 0.6kiB/s ⬆ 0.6kiB/s
2020-04-15 13:30:24 🙌 Starting consensus session on top of parent 0x211aa6080dcd3cf790a0282fb70d000c702d0591f7b2f57ffce1fb1bdd85cf39
2020-04-15 13:30:24 🎁 Prepared block for proposing at 3 [hash: 0xafe36442eda791160810ab76f6280da33fcdaaf37afb9f2fa6887842c34aea24; parent_hash: 0x211a…cf39; extrinsics (1): [0x3a80…201c]]
2020-04-15 13:30:24 🔖 Pre-sealed block for proposal at 3. Hash now 0x93e6608e31188dff6f00cb3a5cfec438461aa0a185da5b5a62606bc9ff0e2b91, previously 0xafe36442eda791160810ab76f6280da33fcdaaf37afb9f2fa6887842c34aea24.
2020-04-15 13:30:24 ✨ Imported #3 (0x93e6…2b91)
2020-04-15 13:30:25 💤 Idle (1 peers), best: #3 (0x93e6…2b91), finalized #0 (0x4377…90f6), ⬇ 0.8kiB/s ⬆ 0.9kiB/s
2020-04-15 13:30:30 ✨ Imported #4 (0x969c…2267)
2020-04-15 13:30:30 💤 Idle (1 peers), best: #4 (0x969c…2267), finalized #1 (0xa78c…882e), ⬇ 1.0kiB/s ⬆ 1.0kiB/s
...
```

These lines shows that Bob has peered with Alice (**`1 peers`**), they have produced some blocks
(**`best: #4 (0x969c…2267)`**), and blocks are being finalized (**`finalized #1 (0xa78c…882e)`**).

Looking at the console that started Bob's node, you should see something similar.

Once you've verified that both nodes are running as expected, you can shut them down. The next
section of this tutorial will include commands to restart the nodes when necessary.

## References

- [Configure ssh local port forwarding](https://www.booleanworld.com/guide-ssh-port-forwarding-tunnelling/)
