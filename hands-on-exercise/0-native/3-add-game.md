---
title: "Add Your First Object"
order: 4
description: Where you make the checkers module interesting
tags:
  - guided-coding
  - cosmos-sdk
---

# Add Your First Object

After the [previous section](./2-build-module.md), you have a somewhat empty checkers module integrated in a minimal app. It is now time to:

* Add a game storage type.
* Compile Protobuf.
* Add the necessary keeper functions.

## The game rules

A good start to developing a checkers blockchain is to define the rule set of the game. There are many versions of the rules. Choose [a very simple set of basic rules](https://www.ducksters.com/games/checkers_rules.php) to avoid getting lost in the rules of checkers or the proper implementation of the board state.

Use [a ready-made implementation](https://github.com/batkinson/checkers-go/blob/a09daeb/checkers/checkers.go) with the additional rule that the board is 8x8, is played on black cells, and black plays first. This code will not need adjustments. Copy this rules file into a new `rules` folder inside the root folder of your `checkers-minimal` module. Change its package from `checkers` to `rules`. You can do this by command-line:

```sh
$ mkdir rules
$ curl https://raw.githubusercontent.com/batkinson/checkers-go/a09daeb1548dd4cc0145d87c8da3ed2ea33a62e3/checkers/checkers.go | sed 's/package checkers/package rules/' > rules/checkers.go
```

## A stored game object

With the rules in place, begin with the minimum game information needed to be stored.

### The game object type

You need:

* **Index:** A string, so it can be identified and retrieved from storage.
* **Black player:** A string, the serialized address.
* **Red player:** A string, the serialized address.
* **Board proper:** A string, the board as it is serialized by the _rules_ file.
* **Player to play next:** A string, specifying whose _turn_ it is.

<HighlightBox type="remember">

When you save strings, it makes it easier to understand what comes straight out of storage but at the expense of storage space. As an advanced consideration, you could store the same information in binary.

</HighlightBox>

As ever, taking inspiration from `minimal-module-example`, this can be described in `proto/../types.proto` with:

```diff-protobuf [https://github.com/b9lab/checkers-minimal/blob/13760f3af095f61d3874042eb4dc9ec2bdbe38ec/proto/alice/checkers/v1/types.proto#L18-L23]
    message GenesisState {
      // params defines all the parameters of the module.
      Params params = 1 [ (gogoproto.nullable) = false ];
    }

+  message StoredGame {
+    string board = 1;
+    string turn = 2;
+    string black = 3 [(cosmos_proto.scalar) = "cosmos.AddressString"];
+    string red = 4 [(cosmos_proto.scalar) = "cosmos.AddressString"];
+  }
```

<HighlightBox type="note">

Since **Index** is used to store and retrieve the stored game, it does not need to be saved to storage. This saves precious storage space.

</HighlightBox>

Compile it:

```sh
$ make proto-gen
```

Now that you have the individual stored game structure, you can define how it is kept in storage.

### The storage structure

The way that makes sense is to keep a **map** of games in storage (and controled by the keeper), and rely on the SDK's basic structures to optimize saving and retrieving.

However, the genesis takes a **list**, and in this list, the games need to be identified by their index.

To achieve that, you create a composed type that is used in the genesis state.

Update your `GenesisState` in `types.proto`:

```diff-protobuf [https://github.com/b9lab/checkers-minimal/blob/c8b9d68ecb5b8031c99f9a0acd0fdb048623df40/proto/alice/checkers/v1/types.proto#L27-L30]
    message GenesisState {
      // params defines all the parameters of the module.
      Params params = 1 [ (gogoproto.nullable) = false];

+    repeated IndexedStoredGame indexedStoredGameList = 2 [(gogoproto.nullable) = false];
    }

    message StoredGame {
      ...
    }

+  message IndexedStoredGame {
+    string index = 1;
+    StoredGame storedGame = 2 [(gogoproto.nullable) = false];
+  }

```

Now recompile.

### Add validation

You can consider adding functions on the `StoredGame`, for instance a `Validate` one.

Additionally, you need to:

* Make sure that the `Index` byte count remains within reasonable bounds, for instance less than 256 bytes.
* Make sure that the genesis does not pass games with conflicting indices.

Start with a new `errors.go` file in the root folder of your `checkers-minimal` module and a new `stored-game.go` to contain relevant functions:

<CodeGroup>
<CodeGroupItem title="keys.go">

```diff-go [https://github.com/b9lab/checkers-minimal/blob/93f66271d1c54dd7d8e89fd33d53390863650d49/keys.go#L6]
    const ModuleName = "checkers"
+  const MaxIndexLength = 256
```

</CodeGroupItem>
<CodeGroupItem title="errors.go">

```go [https://github.com/b9lab/checkers-minimal/blob/93f66271d1c54dd7d8e89fd33d53390863650d49/errors.go]
package checkers

import "cosmossdk.io/errors"

var (
    ErrIndexTooLong     = errors.Register(ModuleName, 2, "index too long")
    ErrDuplicateAddress = errors.Register(ModuleName, 3, "duplicate address")
    ErrInvalidBlack     = errors.Register(ModuleName, 4, "black address is invalid: %s")
    ErrInvalidRed       = errors.Register(ModuleName, 5, "red address is invalid: %s")
    ErrGameNotParseable = errors.Register(ModuleName, 6, "game cannot be parsed")
)
```

</CodeGroupItem>
<CodeGroupItem title="stored-game.go">

Next, in a new `stored-game.go` file:

```go [https://github.com/b9lab/checkers-minimal/blob/93f66271d1c54dd7d8e89fd33d53390863650d49/stored-game.go]
package checkers

import (
    fmt "fmt"

    "cosmossdk.io/errors"
    "github.com/alice/checkers/rules"
    sdk "github.com/cosmos/cosmos-sdk/types"
)

func (storedGame StoredGame) GetBlackAddress() (black sdk.AccAddress, err error) {
    black, errBlack := sdk.AccAddressFromBech32(storedGame.Black)
    return black, errors.Wrapf(errBlack, ErrInvalidBlack.Error(), storedGame.Black)
}

func (storedGame StoredGame) GetRedAddress() (red sdk.AccAddress, err error) {
    red, errRed := sdk.AccAddressFromBech32(storedGame.Red)
    return red, errors.Wrapf(errRed, ErrInvalidRed.Error(), storedGame.Red)
}

func (storedGame StoredGame) ParseGame() (game *rules.Game, err error) {
    board, errBoard := rules.Parse(storedGame.Board)
    if errBoard != nil {
        return nil, errors.Wrapf(errBoard, ErrGameNotParseable.Error())
    }
    board.Turn = rules.StringPieces[storedGame.Turn].Player
    if board.Turn.Color == "" {
        return nil, errors.Wrapf(fmt.Errorf("turn: %s", storedGame.Turn), ErrGameNotParseable.Error())
    }
    return board, nil
}

func (storedGame StoredGame) Validate() (err error) {
    _, err = storedGame.GetBlackAddress()
    if err != nil {
        return err
    }
    _, err = storedGame.GetRedAddress()
    if err != nil {
        return err
    }
    _, err = storedGame.ParseGame()
    return err
}
```

</CodeGroupItem>
</CodeGroup>

With these additions, you can validate the games in `genesis.go`:

```diff-go [https://github.com/b9lab/checkers-minimal/blob/93f66271d1c54dd7d8e89fd33d53390863650d49/genesis.go#L16-L27]
    func (gs *GenesisState) Validate() error {
        if err := gs.Params.Validate(); err != nil {
            return err
        }

+      unique := make(map[string]bool)
+      for _, indexedStoredGame := range gs.IndexedStoredGameList {
+          if length := len([]byte(indexedStoredGame.Index)); MaxIndexLength < length || length < 1 {
+              return ErrIndexTooLong
+          }
+          if _, ok := unique[indexedStoredGame.Index]; ok {
+              return ErrDuplicateAddress
+          }
+          if err := indexedStoredGame.StoredGame.Validate(); err != nil {
+              return err
+          }
+      }

        return nil
    }
```

With the basics of genesis and validation handled, shift focus to the keeper to have it handle this storage too.

### Adjust the keeper files

In order to declare the stored games as a map, you first need to define a map key in `keys.go`:

```diff-go [https://github.com/b9lab/checkers-minimal/blob/67bc8d15a7c25a358e499737670d91526de6f894/keys.go#L10]
    var (
        ParamsKey      = collections.NewPrefix("Params")
+      StoredGamesKey = collections.NewPrefix("StoredGames/value/")
    )
```

Then declare its type in the keeper struct in `keeper/keeper.go`:

```diff-go [https://github.com/b9lab/checkers-minimal/blob/67bc8d15a7c25a358e499737670d91526de6f894/keeper/keeper.go#L25]
    type Keeper struct {
        ...
        Params      collections.Item[checkers.Params]
+      StoredGames collections.Map[string, checkers.StoredGame]
    }
```

And then initialize the storage access, taking inspiration from `minimal-module-example`:

```diff-go [https://github.com/b9lab/checkers-minimal/blob/67bc8d15a7c25a358e499737670d91526de6f894/keeper/keeper.go#L40-L42]
    ...
    k := Keeper{
        ...
        Params:      collections.NewItem(sb, checkers.ParamsKey, "params", codec.CollValue[checkers.Params](cdc)),
+      StoredGames: collections.NewMap(sb,
+          checkers.StoredGamesKey, "storedGames", collections.StringKey,
+          codec.CollValue[checkers.StoredGame](cdc)),
    }
    ...
```

<HighlightBox type="tip">

What this initialization does is explained [here](https://docs.cosmos.network/v0.50/packages/collections):

> Collections is a library meant to simplify your experience with respect to module state handling.

The `codec.CollValue` construct is covered [in the documentation](https://docs.cosmos.network/v0.50/packages/collections#key-and-value-codecs).

</HighlightBox>

Do not forget the genesis manipulation to and from storage in `keeper/genesis.go`, again taking inspiration from `minimal-module-example`:

```diff-go [https://github.com/b9lab/checkers-minimal/blob/67bc8d15a7c25a358e499737670d91526de6f894/keeper/genesis.go]
    func (k *Keeper) InitGenesis(ctx context.Context, data *checkers.GenesisState) error {
        if err := k.Params.Set(ctx, data.Params); err != nil {
            return err
        }

+      for _, indexedStoredGame := range data.IndexedStoredGameList {
+          if err := k.StoredGames.Set(ctx, indexedStoredGame.Index, indexedStoredGame.StoredGame); err != nil {
+              return err
+          }
+      }
+
        return nil
    }

    // ExportGenesis exports the module state to a genesis state.
    func (k *Keeper) ExportGenesis(ctx context.Context) (*checkers.GenesisState, error) {
        params, err := k.Params.Get(ctx)
        if err != nil {
            return nil, err
        }

+      var indexedStoredGames []checkers.IndexedStoredGame
+      if err := k.StoredGames.Walk(ctx, nil, func(index string, storedGame checkers.StoredGame) (bool, error) {
+          indexedStoredGames = append(indexedStoredGames, checkers.IndexedStoredGame{
+              Index:      index,
+              StoredGame: storedGame,
+          })
+          return false, nil
+      }); err != nil {
+          return nil, err
+      }
+
        return &checkers.GenesisState{
            Params:                params,
+          IndexedStoredGameList: indexedStoredGames,
        }, nil
    }
```

## Test again

Just like you did in the [previous section](./1-preparation.md), compile the minimal chain, re-initialize, and start it. You need to re-initialize because your genesis has changed once again:

```sh
$ make install
$ make init
$ minid start
```

Now your minimal chain not only has a checkers module, but also a games storage area. After stopping it with <kbd>CTRL-C</kbd>, confirm this by calling up:

<CodeGroup>
<CodeGroupItem title="Straight">

```sh
$ minid export
```

</CodeGroupItem>
<CodeGroupItem title="Clean">

```sh
$ minid export | tail -n 1 | jq
```

</CodeGroupItem>
</CodeGroup>

In there, you can find:

```diff-json
    {
        ...
        "app_state: {
            ...
            "checkers": {
                "params": {},
+              "indexedStoredGameList": []
            },
            ...
        }
    }
```

## Up next

You have an on-chain game storage area, but it is empty. In the next section, you will start populating it with the use of a transaction message.
