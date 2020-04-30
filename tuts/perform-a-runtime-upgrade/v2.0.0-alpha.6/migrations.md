---
slug: migrations
lang: en
title: Storage Migrations
---

The runtime upgrade we just performed completed successfully simply by adding our new feature and incrementing our `spec_version`. In may simple and moderately complex cases this is all that's necessary. In other cases, you may restructure the way data is stored in the blockchain as well as modifying the logic. In these cases, it is necessary to include a migration path for the existing data. In this section we'll explore data migrations.

## Primary Code Change

For this example we will rename our storage struct from `TemplateModule` to `UpgradedTemplateModule`. To begin we open the `pallets/template/src/lib.rs` file and modify the the following line
```rust
trait Store for Module<T: Trait> as TemplateModule {
```

so that it says

```rust
trait Store for Module<T: Trait> as UpgradedTemplateModule {
```

## Bumping the Spec Version

As before, we will open the `runtime/src/lib.rs` file and change our `spec_version`. This time we change it to 3.
```rust
/// This runtime version.
pub const VERSION: RuntimeVersion = RuntimeVersion {
	spec_name: create_runtime_str!("node-template"),
	impl_name: create_runtime_str!("node-template"),
	authoring_version: 1,
	spec_version: 2, //TODO change this to 3
	impl_version: 1,
	apis: RUNTIME_API_VERSIONS,
};
```

## Writing the Migration

Any time you change the storage struct, the name of a storage item, or the way a key is calculated in a storage map, you need to write a migration, or else the old data will not be accessible by the new code.

As you can see from the `decl_storage!` block of our template pallet, we are only dealing with a single storage item that was, and still is called `Something`.

The complete code for our migration looks like this, and can be inserted at the bottom of the `decl_module!` block, just like the dispatchable call we aded in the previous upgrade.

```rust
fn on_runtime_upgrade() -> frame_support::weights::Weight {
			use frame_support::storage::migration::{get_storage_value, put_storage_value};

			let value_to_migrate: Option<u32> = get_storage_value(b"TempalteModule", b"Something", &[]);
			put_storage_value(b"UpgradedTemplateModule", b"Something", &[], value_to_migrate);

			1_000 // In reality the weight of migration should be determined by benchmarking
		}
```

First, his code `use`s two helper functions from frame support that are designed specifically for storage migrations. You will always need to do this.

Next, it grabs the value out of the old storage location. Notice that we need an explicit type annotation when grabbing the old data. This will always be necessary, and if you are unsure, just check the `decl_storage!` block to learn the type. As parameters, we have supplied the _old_ storage struct name, the storage item name, and an empty array. The third parameter will be necessary when working with storage maps, but we are using a plain storage value, so we leave it blank.

Once the old value is retrieved, we put it into the new storage locations. This time we supply the _new_ storage struct name, the same storage value name, and, again, an empty array. The final parameter if the value to store that we grabbed out of the old storage location.

TODO Do we need to kill the old storage here? Probably.

Finally we return a weight to quantify the time it takes to execute this logic. In this tutorial I have included an arbitrary weight. But in a real network, it is important to benchmark the execution time of the function, and choose a weight appropriately.

## Perform the Upgrade

You're now ready to recompile your runtime, and perform the upgrade using the sudo pallet as we did with the previous upgrade.

## That's it!

In this tutorial, you performed two runtime upgrades on a live blockchain without causing any forks. Congratulations! You also learned how and when to write storage migrations.
