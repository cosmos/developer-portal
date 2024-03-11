---
title: "Integrate CosmosKit and Keplr"
order: 7
description: Introduce a dApps toolkit to connect wallets
tags:
  - guided-coding
  - cosmos-kit
---

# Integrate CosmosKit and Keplr

CosmosKit is a wallet adapter for developers to build apps that quickly and easily interact with Cosmos blockchains and wallets.

## Quickstart

🏁 Get started quickly by using [create-cosmos-app](https://github.com/cosmology-tech/create-cosmos-app) to help you build high-quality Cosmos apps fast!

## Use CosmosKit from Scratch

### 1️. Install Dependencies

```sh
yarn add @cosmos-kit/react @cosmos-kit/keplr chain-registry
```

`@cosmos-kit/react` includes default modal made with `@interchain-ui/react`. If [customized modal](/provider/chain-provider/#customize-modal-with-walletmodal) is provided, you can use `@cosmos-kit/react-lite` instead to lighter your app.

There are multiple wallets supported by CosmosKit. Details see [Integrating Wallets](/integrating-wallets)

### 2️. Wrap Provider

First, add [`ChainProvider`](/provider/chain-provider) and provider [required properties](/provider/chain-provider#required-properties).

Example:

```tsx
import * as React from 'react';

import { ChainProvider } from '@cosmos-kit/react';
import { chains, assets } from 'chain-registry';
import { wallets } from '@cosmos-kit/keplr';

// Import this in your top-level route/layout
import "@interchain-ui/react/styles";

function CosmosApp() {
  return (
    <ChainProvider
      chains={chains} // supported chains
      assetLists={assets} // supported asset lists
      wallets={wallets} // supported wallets
      walletConnectOptions={...} // required if `wallets` contains mobile wallets
    >
      <YourWalletRelatedComponents />
    </ChainProvider>
  );
}
```

### 3️. Consume with Hook

Take `useChain` as an example.

```tsx
import * as React from 'react';

import { useChain } from "@cosmos-kit/react";

function Component ({ chainName }: { chainName: string }) => {
    const chainContext = useChain(chainName);

    const {
      status,
      username,
      address,
      message,
      connect,
      disconnect,
      openView,
    } = chainContext;
}
```
