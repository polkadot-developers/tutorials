---
slug: transaction
lang: en
title: Upgrade Transaction
---

In this section we will perform the on-chain runtime upgrading by submitting the new runtime logic in an extrinsic.

## Locating the Wasm Build Artifact

At the end of the last section we compiled our runtime. Substrate Runtimes are always compiles to Web Assembly (or Wasm) as well as native code so that the Wasm can be stored on the blockchain and facilitate this forkless upgrade process. Our freshly compiled runtime is stored at `./target/release/wbuild/node-template-runtime/node-template-runtime.compact.wasm`. Ensure this file exists and that it was modified recently.

## Starting the User Interface

For this upgrade, we will use the Polkadot JS Apps user interface; the same UI we used in the [Create Your First Substrate Chain](/tutorials/create-your-first-substrate-chain/v2.0.0-alpha.6) Tutorial.

In your web browser, navigate to [https://polkadot.js.org/apps](https://polkadot.js.org/apps/#/settings?rpc=ws://127.0.0.1:9944).

On the `Settings` tab ensure that you are connected to a `Local Node` or `ws://127.0.0.1:9944`.

> Some browsers, notably Firefox, will not connect to a local node from an https website. An easy work around is to try another browser, like Chromium. Another option is to [host this interface locally](https://github.com/polkadot-js/apps#development).

## Submit the Transaction

As you can imagine, in a real-world blockchain, we don't want just anyone to be able to change the runtime logic. There is a special transaction for performing these upgrades called, `system::set_code`. This special transaction cannot be called by an ordinary user, and must be called from within the blockchain itself. Substrate provides many useful pallets to provide limited access to this sensitive function as well as others like it. In a real-world blockchain you might use the Democracy or Collective pallets. Our blockchain has a very simple governance mechanism called sudo which allows a privileged user to call sensitive functions like `system::set_code`. In our case, the privileged user is the `Alice` account, so we will submit the upgrade transaction as her.

Navigate to the Sudo tab in the interface, and select `system` and `set_code` from the dropdowns. Upload the `node-template-runtime.compact.wasm` file we saw previously, and submit the transaction.

TODO screenshot

## Try out the Results

Once the upgrade transaction is included in a block, you should see a notification in the UI saying the a runtime upgrade has been performed, and the UI will need to be refreshed. Once you've refreshed, you can navigate to the "Extrinsics" tab, select "Template Module" from the dropdown, and see that our new extrinsic, `clear_value` is now available.

Congratulations, you've upgraded your blockchain's runtime! Traditionally the process of upgrading a blockchain would have required a coordinated effort from all (or at least most) of the node operators in the network. But in this case, you have performed the upgrade in a single transaction without even causing a fork!
