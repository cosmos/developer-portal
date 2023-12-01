---
title: "What is CosmosKit?"
order: 7
description: CosmosKit and what it can do for me
tags:
  - tutorial
  - concepts
  - cosmos-kit
---

# What is CosmosKit?

<HighlightBox type="learning">

A wallet adapter for developers to build apps that quickly and easily interact with Cosmos blockchains and wallets.

## Packages

CosmosKit is a library that consists of many smaller npm packages within the [@cosmos-kit namespace](https://www.npmjs.com/org/cosmos-kit), a so-called "monorepo".

Generally people only need the `react` and `WALLET` packages such as `keplr` as they contain the main functionality to interact with Cosmos SDK chains version 0.40 and higher.

### [@cosmos-kit/core](https://github.com/cosmology-tech/cosmos-kit/tree/main/packages/core)

Core package of CosmosKit, including types, utils and base classes.

### [@cosmos-kit/react](https://github.com/cosmology-tech/cosmos-kit/tree/main/packages/react)

A wallet adapter for React with mobile WalletConnect support for the Cosmos ecosystem.

### [@cosmos-kit/react-lite](https://github.com/cosmology-tech/cosmos-kit/tree/main/packages/react-lite)

A lighter version of `@cosmos-kit/react` without the default modal.

### [@cosmos-kit/ins](https://github.com/cosmology-tech/cosmos-kit/tree/main/packages/ins)

Interchain Name System implementation

### [@cosmos-kit/walletconnect](https://github.com/cosmology-tech/cosmos-kit/tree/main/packages/walletconnect)

[Wallet Connect V2](https://walletconnect.com/) support tools (usually used when integrating mobile wallets).

### [WALLETS](https://docs.cosmoskit.com/integrating-wallets)

Wallets integrated in CosmosKit.

## Related Projects

### [create-cosmos-app](https://github.com/cosmology-tech/create-cosmos-app)

Set up a modern Cosmos app by running one command ⚛️

### [chain-registry](https://github.com/cosmology-tech/chain-registry)

An npm module for the official Cosmos chain-registry

<HighlightBox type="reading">

Some additional reading or video material is available as well:

* [CosmosKit Documentation](https://docs.cosmoskit.com/)
* [Cosmology - Create a Cosmos App with create-cosmos-app](https://www.youtube.com/watch?v=-jJqeS47c3k)

</HighlightBox>
