---
slug: index
lang: en
title: Clone a Live Chain
---

In this tutorial we will learn to start a Substrate chain that takes its initial state from an existing live blockchain. This technique is useful for investigating bugs, testing runtime upgrades, performing airdrops, and "forking" a community during a contentious disagreement.

## Install the Node Template

You should already have version `v2.0.0-alpha.6` of the [Substrate Node
Template](https://github.com/substrate-developer-hub/substrate-node-template) compiled on your
computer from when you completed the [Create Your First Substrate Chain
Tutorial](/tutorials/create-your-first-substrate-chain/v2.0.0-alpha.6). If you do not, please complete that
tutorial.

> Experienced developers who truly prefer to skip that tutorial, you may install the node template according to the instructions in its readme.

## What you will be doing

Before we even get started, let's lay out what we are going to do over the course of this tutorial.
We will:

1. Launch a Substrate blockchain  based on a template project.
3. Export that chain's state to a file.
4. Use the file to create a second chain that is a clone of the first.

Sound reasonable? Good, then let's begin!
