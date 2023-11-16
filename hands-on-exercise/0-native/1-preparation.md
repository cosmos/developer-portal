---
title: "Preparation"
order: 2
description: Be ready to build your application the native way
tags:
  - guided-coding
  - cosmos-sdk
---

# Preparation

In this section, you will start building a blockchain to play the checkers game. You will build it natively with Cosmos SDK v0.50. At least, you will build some initial elements.

This section uses the following repositories:

* A minimal Cosmos v0.50 app with no extra modules, found [here](https://github.com/cosmosregistry/chain-minimal).
* A minimal v0.50 module example, found [here](https://github.com/cosmosregistry/example).

Version 0.50 of the Cosmos SDK was created, in part, with a view to facilitate module integration. This section seeks to demonstrate that. In particular, you will keep your checkers module separate from the (minimal) app.

## Install

Before you can start building, you need to prepare your computer. The simplest way is to rely on [Docker](https://docs.docker.com/engine/install/), as the Cosmos team has prepared a [Docker image for developers](https://github.com/cosmos/cosmos-sdk/pkgs/container/proto-builder/119928846?tag=0.14.0) built via [this Dockerfile](https://github.com/cosmos/cosmos-sdk/blob/v0.50.1/contrib/devtools/Dockerfile). If you need a refresher on Docker, head [here](/tutorials/5-docker-intro/index.md).

You will also need to have [`make`](https://www.gnu.org/software/make/) on your computer.

## The minimal app

Since you are building off the minimal app, clone it locally:

```sh
$ git clone https://github.com/cosmosregistry/chain-minimal.git --branch mini-v050
$ cd chain-minimal
$ git checkout -b main
```

You can already confirm that it compiles and runs correctly. You need Go version 1.21. To compile it, run:

```sh
$ make install
```

Make use of `scripts/init.sh` to set it up with test values:

```sh
$ make init
```

Run it:

```sh
$ minid start
```

You should see the familiar log:

```txt
...
7:03PM INF finalized block block_app_hash=25D4020CCCE016BFC606F4904334EF3E81F60A283F694FC5F11B0F1F9850EA83 height=1 module=state num_txs_res=0 num_val_updates=0
7:03PM INF executed block app_hash=25D4020CCCE016BFC606F4904334EF3E81F60A283F694FC5F11B0F1F9850EA83 height=1 module=state
...
```

In another shell, you can confirm the list of modules that are included:

```sh
$ minid query --help
```

This confirms that your minimal chain is working as expected. You can stop it with <kbd>CTRL-C</kbd>.

## The module example

Since you are going to copy a (small) number of files from the module example, it is convenient to have a local copy somewhere:

```sh
$ git clone https://github.com/cosmosregistry/example.git --branch v0.1.0 minimal-module-example
$ cd minimal-module-example
$ git checkout -b main
```

Moreover, your checkers module is going to be named `github.com/alice/checkers`, so to make your file copying even easier you can use the provided renaming script:

```sh
$ MODULE_NAME=alice/checkers ./scripts/rename.sh
```

The updated files will come in handy when you want to copy-paste them.

<ExpansionPanel title="Troubleshooting">
<PanelListItem number="1">

The `rename.sh` files self-removes when the process is complete. If an error happens, you may have to reset hard with Git before retrying:

```sh
$ git reset --hard
```

This assumes that you have no other uncommitted changes that are worth keeping.

</PanelListItem>
<PanelListItem number="2" :last="true">

If you get something like:

```txt
 /usr/bin/env: 'bash\r': No such file or directory
```

Note that the `\r` points to line endings. Try changing the script's line endings from Windows to Unix/Linux style, or vice-versa.

</PanelListItem>
</ExpansionPanel>

## Up next

With the minimal chain and a copy of the minimal module, it is time to build your own module.
