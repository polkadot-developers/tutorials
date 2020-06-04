---
slug: code
lang: en
title: Code Changes
---

## Install the Node Template

You should already have version `v2.0.0-alpha.6` of the [Substrate Node
Template](https://github.com/substrate-developer-hub/substrate-node-template) compiled and running on your
computer from when you completed the [Create Your First Substrate Chain
Tutorial](/tutorials/create-your-first-substrate-chain/v2.0.0-alpha.6). If you do not, please complete that
tutorial.

> Experienced developers who truly prefer to skip that tutorial, you may install the node template according to the instructions in its readme.

Further, you should have the chain running, as it was at the end of that tutorial.

If you have completed that tutorial, but your chain is no longer currently running, `cd` into the directory you created in that tutorial, and run the command `./target/release/node-template --dev` to start your node again.

## Make a Change to the Code

Runtime upgrades are necessary when you want to change the code of a live chain. While it is generally advisable to complete the code as much as possible before launching the chain, changes after launch become necessary to do things like fix bugs or add features.

### Primary Logic Change

The Substrate Node Template contains a template pallet which accepts transactions to store a value and increment a value. In this tutorial we will add one extrinsic to clear the value, and leave `None` in its place.

Open the `substrate-node-template` in your favorite code editor. Then open the file
`pallets/template/src/lib.rs`

```
substrate-node-template
|
+-- runtime
|
+-- pallets
|   |
|   +-- template
|       |
|       +-- Cargo.toml
|       |
|       +-- src
|           |
|           +-- lib.rs     <-- Edit this file
|           |
|           +-- mock.rs
|           |
|           +-- tests.rs
|
+-- scripts
|
+-- node
|
+-- ...
```

In this file you will see this block of code containing two extrinsics used to set and increment the value.

```rust
decl_module! {
	/// The module declaration.
	pub struct Module<T: Trait> for enum Call where origin: T::Origin {
		// --snip--

		#[weight = frame_support::weights::SimpleDispatchInfo::default()]
		pub fn do_something(origin, something: u32) -> dispatch::DispatchResult {
			// --snip--
		}

		#[weight = frame_support::weights::SimpleDispatchInfo::default()]
		pub fn cause_error(origin) -> dispatch::DispatchResult {
			// --snip--
		}

		// TODO Add your code here.
	}
}
```

Add the following code at the bottom of the `decl_module!` block. This code adds a third extrinsic to clear the stored value.

```rust
/// Clears the value stored, if any
#[weight = frame_support::weights::SimpleDispatchInfo::default()]
pub fn clear_value(origin) -> dispatch::DispatchResult {
	// Check it was signed and get the signer.
	let _who = ensure_signed(origin)?;

	// Clear the storage value
	Something::kill();

	Ok(())
}
```

Confirm that your changes are correct so far by running `cargo check -p pallet-template`. If this command completes successfully, you're ready to move on. If not, stop and fix your errors or ask for help before continuing.

### Bumping the Spec Version

We've already made all of the logic changes we intend to make to our code, and our runtime is perfectly valid in its current state. However, because we will be upgrading a live chain, we need to indicate that this is a new version of the runtime. In particular, it is a new `spec_version`.

> There are multiple ways in which a runtime is versioned. Read more about runtime versioning in the rustdocs about the [`RuntimeVersion` struct](https://substrate.dev/rustdocs/v2.0.0-alpha.6/sp_version/struct.RuntimeVersion.html).

Open the file
`runtime/src/lib.rs`

```text
substrate-node-template
|
+-- runtime
|   |
|   +-- Cargo.toml
|   |
|   +-- src
|       |
|       +-- lib.rs     <-- Edit this file
|
+-- pallets
|
+-- scripts
|
+-- node
|
+-- ...
```

Look for this section of code, and change the spec version from 1 to 2.

```rust
/// This runtime version.
pub const VERSION: RuntimeVersion = RuntimeVersion {
	spec_name: create_runtime_str!("node-template"),
	impl_name: create_runtime_str!("node-template"),
	authoring_version: 1,
	spec_version: 1, //TODO Change this to 2
	impl_version: 1,
	apis: RUNTIME_API_VERSIONS,
};
```

## Recompile Your Runtime

You're now ready to recompile your runtime. To build _just_ the runtime, and not the entire node, you may run the command.

```bash
cargo build --release -p node-template-runtime
```
