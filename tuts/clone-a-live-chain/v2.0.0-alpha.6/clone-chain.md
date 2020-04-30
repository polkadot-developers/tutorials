---
slug: clone-chain
lang: en
title: Start the Clone Chain
---

Now that we've exported the state of the original chain into a chain spec file, we are able to use that as the genesis state of our new clone chain. Before starting the process, it is useful to observe some properties about the clone chain and this process.

The clone chain will:
* Have the same state (including token balances) as the original

The clone chain will not
* Have the same number of blocks or transactions as the original
* Have the same transactions as the original
* Be a hard fork of the original

Unsure need to experiment:
* Be able to execute transactions designed for the original - definitely not if it has the checkGenesis. unsure about if it doesn't have that.

## Starting the new Chain

```bash
./target/release/node-template --chain=clone-spec.json --alice --base-path /tmp/clone/alice
```

## Submitting a Transaction

Connect Apps or another UI. Ensure the balances and othe rstate are as expected. Submit a transaction and ensure it works.

## Observing the Difference

Connect Apps to the original chain and see that the transaction you submitted to the clone has not executed on the original.

Submit a transaction to the original and ensure it doesn't execute on the clone.
