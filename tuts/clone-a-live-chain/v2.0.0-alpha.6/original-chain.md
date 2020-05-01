---
slug: original chain
lang: en
title: Start the Original Chain
---

Before we can perform any cloning, we need an original chain that will be coned.

## Start the chain

`./target/release/node-template --dev --base-path /tmp/original/alice`

The custom base path will help us avoid mixing up our two chains.

## Accumulate some State

There's no use forking a chain that doesn't yet have any interesting state. IT would be easier to jsut start a new chain. Let's build up some interesting state. The exact state is up to you. But for example you might.

* Transfer tokens to various accounts
* Create your own account and get some tokens
* Store a value in the template pallet
* Perform some kind of runtime upgrade (would need to test whether this works)

## Kill your chain

It isn't truly necessary to kill it. In the wild, the original chain wouldn't die. But it may help prevent confusion or mixing up the two chains to not have it actively running.
