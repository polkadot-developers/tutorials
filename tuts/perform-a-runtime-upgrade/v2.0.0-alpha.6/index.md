---
slug: index
lang: en
title: Overview
---

In this tutorial, you will perform a runtime upgrade on a live blockchain. You will use Substrate's forkless upgrade mechanism to perform the upgrade without requiring a client upgrade, and without causing a fork in the network.

This tutorial guides you step-by-step through reproducing the results demonstrated by Ricardo Rius in his [Sub0 talk, Runtime Upgrades](https://www.crowdcast.io/e/sub0-online/8).

We only expect that:

* You are generally familiar with software development, writing code, and running your code.
* You have completed the [Create Your First Substrate Chain Tutorial](/tutorials/create-your-first-substrate-chain/v2.0.0-aplha.6).
* You are open to learning about the bleeding edge of blockchain development.

If you run into an issue on this tutorial, **we are here to help!** You can [create a new
issue](https://github.com/substrate-developer-hub/tutorials/issues/new) or
contact us on [Riot](https://riot.im/app/#/room/!HzySYSaIhtyWrwiwEV:matrix.org).

## What you will be doing

Before we even get started, let's lay out what we are going to do over the course of this tutorial.
We will:

1. Launch a blockchain based on a template project.
2. Modify this template project with some custom logic, after the chain has already launched.
3. Upgrade the live chain using Substrate's forkless upgrade mechanism.
4. Discuss Chain state migrations.

Sound reasonable? Good, then let's begin!
