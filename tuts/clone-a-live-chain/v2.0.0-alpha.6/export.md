---
slug: export
lang: en
title: Export Chain State
---

To clone a live chain in the wild, you will need a synchronized node from which you can export the existing state. In this tutorial, we already have a node to do this, and we will use it.

## Create the export

```bash
./target/release/node-template export-state --dev --base-path /tmp/original/alice > clone-spec.json
```


I got stuck here because latest node template won't start, and old node templates don't have the `export-state`. I'll still sketch out the rest of the idea though.
