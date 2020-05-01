---
slug: customspec
lang: en
title: Create a Custom Chain Spec
---

Now that each participant has their own keys generated, you're ready to create a custom chain
specification. We will use this custom chain spec instead of the built-in `local` spec that we used
previously.

In this example we will create a two-node network, but the process generalizes to more nodes in a
straight-forward manner.

## Create a Chain Specification

Last time around, we used `--chain local` which is a predefined "chain spec" that has Alice and Bob
specified as validators along with many other useful defaults.

Rather than writing our chain spec completely from scratch, we'll just make a few modifications to
the one we used before. To start, we need to export the chain spec to a file named
`customSpec.json`. Remember, further details about all of these commands are available by running
`node-template --help`.

```bash
# Export the local chainspec to json
$ ./target/release/node-template build-spec --disable-default-bootnode --chain local > customSpec.json
2020-01-10 15:39:04 Building chain spec
```

The file we just created contains several fields, and you can learn a lot by exploring them. By far
the largest field is a single binary blob that is the Wasm binary of our runtime. It is part of what
you built earlier when you ran the `cargo build` command.

The portion of the file we're interested in is the Aura authorities used for creating blocks,
indicated by **"aura"** field below, and GRANDPA authorities used for finalizing blocks, indicated
by **"grandpa"** field. That section looks like this

```json
{
  //-- snip --
  "genesis": {
    "runtime": {
      "system": {
        "changesTrieConfig": null
        //-- snip --
      },
      "aura": {
        "authorities": [
          "5FfBQ3kwXrbdyoqLPvcXRp7ikWydXawpNs2Ceu3WwFdhZ8W4",
          "5FHneW46xGXgs5mUiveU4sbTyGBzmstUspZC92UhjJM694ty"
        ]
      },
      "grandpa": {
        "authorities": [
          ["5G9NWJ5P9uk7am24yCKeLZJqXWW6hjuMyRJDmw4ofqxG8Js2", 1],
          ["5GoNkf6WdbxCFnPdAnYYQyCjAKPJgLNxXwPjwTh6DGg6gN3E", 1]
        ]
      }
      //-- snip --
    }
  }
}
```

> These instructions are written for the node template. They will work with other Substrate-based
> nodes with few modifications. In Substrate nodes that include the session pallet. You should leave
> the Aura and Grandpa configs empty and instead insert this information in the session config. All
> of this is demonstrated in the chain spec you export from your desired node.

The format for the grandpa data is more complex because the grandpa protocol supports weighted
votes. In this case we have given each validator a weight of **1**.

All we need to do is change the authority addresses listed (currently Alice and Bob) to our own
addresses that we generated in the previous step. The **sr25519** addresses go in the **aura**
section, and the **ed25519** addresses in the **grandpa** section. You may add as many validators as
you like. For additional context, read about
[keys in Substrate](https://substrate.dev/docs/en/next/conceptual/cryptography/keys).

> Validators should not share the same keys, even for learning purposes. If two validators have the
> same keys, they will produce conflicting blocks.

Once the chain spec is prepared, convert it to a "raw" chain spec. The raw chain spec contains all
the same information, but it contains the encoded storage keys that the node will use to reference
the data in its local storage. Distributing a raw spec ensures that each node will store the data at
the proper storage keys.

```bash
./target/release/node-template build-spec --chain=customSpec.json --raw --disable-default-bootnode > customSpecRaw.json
```

Finally share the `customSpecRaw.json` with your all the other validators in the network.

> A single person should create the chain spec and share the resulting **`customSpecRaw.json`** file
> with their fellow validators.
>
> Because Rust -> Wasm optimized builds aren't "reproducible", each person will get a slightly
> different Wasm blob which will break consensus if each participant generates the file themselves.
