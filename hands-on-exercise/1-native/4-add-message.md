---
title: "Add your first message"
order: 5
description: Where you make it possible to create a game
tags:
  - guided-coding
  - cosmos-sdk
---

# Add your first message

In the [previous section](./3-add-game.md), you added a game storage space in the checkers module storage. It is now time to actually put games in storage:

* Add a message type to create a game.
* Add all the necessary keeper and message server functions.
* Add CLI commands.

## Add a game creation message

With the storage ready to receive games, it is time to add a message to let players create games:

* A game starts with an initial board and turn, as per the rules. The game creator **does not choose** how the initial pieces are placed.
* A game needs an **unused index** to be created. For simplicity's sake, the index will be passed along with the message.

You are going to:

* Create the message type.
* Compile Protobuf
* Create a message server next to the keeper.
* Handle the message.
* Register the necessary elements in the module.

### The game creation object type and Protobuf service

You define them together in a new file `proto/../tx.proto`:

```protobuf [proto/alice/checkers/v1/tx.proto]
syntax = "proto3";
package alice.checkers.v1;

option go_package = "github.com/alice/checkers";

import "cosmos/msg/v1/msg.proto";
import "gogoproto/gogo.proto";
import "amino/amino.proto";
import "alice/checkers/v1/types.proto";
import "cosmos_proto/cosmos.proto";

// Msg defines the module Msg service.
service Msg {
  option (cosmos.msg.v1.service) = true;

  // CreateGame create a game.
  rpc CreateGame(MsgCreateGame)
      returns (MsgCreateGameResponse);
}

// MsgCreateGame defines the Msg/CreateGame request type.
message MsgCreateGame {
  option (cosmos.msg.v1.signer) = "sender";
  option (amino.name) = "alice/checkers/MsgCreateGame";

  // sender is the message sender.
  string sender = 1;
  string index = 2 ;
  string black = 3 [(cosmos_proto.scalar) = "cosmos.AddressString"];
  string red = 4 [(cosmos_proto.scalar) = "cosmos.AddressString"];
}

// MsgCreateGameResponse defines the Msg/CreateGame response type.
message MsgCreateGameResponse {}
```

Note:

* How `MsgCreateGame` does not mention `Board` or `Turn` as this should not be under the control of the sender.
* That the response is empty as there is no extra information to return. For instance, here the game index is known in advance.

### Compile Protobuf

Since you have defined a new message type and an associated `service`, you should recompile the lot:

```sh
$ make proto-gen
```

In the new `tx.pg.go` you can find `type MsgCreateGame struct` and `type MsgServer interface`. Now you can use them closer to the keeper.

### New message server

Create a new file `keeper/msg_server.go` and take inspiration from `minimal-module-example`. It needs to:

* Check that the `index` is not already taken.
* Create a new board with the game rules.
* If valid, store the game into storage.

```go [keeper/msg_server.go]
package keeper

import (
    "context"
    "errors"
    "fmt"

    "cosmossdk.io/collections"
    "github.com/alice/checkers"
    "github.com/alice/checkers/rules"
)

type msgServer struct {
    k Keeper
}

var _ checkers.MsgServer = msgServer{}

// NewMsgServerImpl returns an implementation of the module MsgServer interface.
func NewMsgServerImpl(keeper Keeper) checkers.MsgServer {
    return &msgServer{k: keeper}
}

// CreateGame defines the handler for the MsgCreateGame message.
func (ms msgServer) CreateGame(ctx context.Context, msg *checkers.MsgCreateGame) (*checkers.MsgCreateGameResponse, error) {
    if _, err := ms.k.StoredGameList.Get(ctx, msg.Index); err == nil || errors.Is(err, collections.ErrEncoding) {
        return nil, fmt.Errorf("game already exists at index: %s", msg.Index)
    }

    newBoard := rules.New()
    storedGame := checkers.StoredGame{
        Index: msg.Index,
        Board: newBoard.String(),
        Turn:  rules.PieceStrings[newBoard.Turn],
        Black: msg.Black,
        Red:   msg.Red,
    }
    if err := storedGame.Validate(); err != nil {
        return nil, err
    }
    if err := ms.k.StoredGameList.Set(ctx, msg.Index, storedGame); err != nil {
        return nil, err
    }

    return &checkers.MsgCreateGameResponse{}, nil
}
```

Note how the sender is not saved in storage. This is a design decision and you may in fact decide to keep the sender.

### Register the types in the module

Now that you have message types and server, you should register the types them in the currently empty `codec.go`. Inspire yourself from what you find in `minimal-module-example`:

```diff-go [codec.go]
    package checkers

    import (
        "github.com/cosmos/cosmos-sdk/codec"
+      "github.com/cosmos/cosmos-sdk/codec/legacy"
        types "github.com/cosmos/cosmos-sdk/codec/types"
+      sdk "github.com/cosmos/cosmos-sdk/types"
+      "github.com/cosmos/cosmos-sdk/types/msgservice"
    )

    // RegisterLegacyAminoCodec registers the necessary interfaces and concrete types
    // on the provided LegacyAmino codec. These types are used for Amino JSON serialization.
    func RegisterLegacyAminoCodec(cdc *codec.LegacyAmino) {
+      legacy.RegisterAminoMsg(cdc, &MsgCreateGame{}, "checkers/MsgCreateGame")
    }

    // RegisterInterfaces registers the interfaces types with the interface registry.
    func RegisterInterfaces(registry types.InterfaceRegistry) {
+      registry.RegisterImplementations((*sdk.Msg)(nil),
+          &MsgCreateGame{},
+      )
+      msgservice.RegisterMsgServiceDesc(registry, &_Msg_serviceDesc)
    }
```

And into `module/module.go`, you register the new service. The lines were previously commented out:

```diff-go [module/module.go]
    ...
    func (am AppModule) RegisterServices(cfg module.Configurator) {
        // Register servers
-      // checkers.RegisterMsgServer(cfg.MsgServer(), keeper.NewMsgServerImpl(am.keeper))
+      checkers.RegisterMsgServer(cfg.MsgServer(), keeper.NewMsgServerImpl(am.keeper))
        // checkers.RegisterQueryServer(cfg.QueryServer(), keeper.NewQueryServerImpl(am.keeper))
        ...
    }
    ...
```

### Add the CLI commands

At this stage, your module is able to handle messages passed to it by the app. But you are not yet able to craft such messages in the first place. When working from the command line, that crafting is handled by the CLI client. The CLI client usually tells you what messages and transactions it can create, when you run `minid tx --help`. Go ahead and check:

```sh
$ make install
$ minid tx --help
```

You can see that `checkers` is missing from the list of available commands. You fix that by entering your desired command in `module/autocli.go`. Taking inspiration from `minimal-module-example`:

```diff-go
    ...
-  Tx: nil,
+  Tx: &autocliv1.ServiceCommandDescriptor{
+      Service: checkersv1.Msg_ServiceDesc.ServiceName,
+      RpcCommandOptions: []*autocliv1.RpcCommandOptions{
+          {
+              RpcMethod: "CreateGame",
+              Use:       "create index black red",
+              Short:     "Creates a new checkers game at the index for the black and red players",
+              PositionalArgs: []*autocliv1.PositionalArgDescriptor{
+                  {ProtoField: "index"},
+                  {ProtoField: "black"},
+                  {ProtoField: "red"},
+              },
+          },
+      },
+  },
    ...
```

Note that the `create` in `"create index black red"` is parsed out and used as the command in the `minid tx checkers create` command-line.

## Test again

Back in the minimal chain, compile to see the command that was added:

```sh
$ make install
```

Now, you should see the `create` command:

<CodeGroup>
<CodeGroupItem title="tx">

```sh
$ minid tx --help
```

Lists:

```txt
...
checkers            Transactions commands for the checkers module
...
```

</CodeGroupItem>
<CodeGroupItem title="checkers">

```sh
$ minid tx checkers --help
```

Returns:

```txt
...
Available Commands:
  create      Creates a new checkers game at the index for the black and red players
...
```

</CodeGroupItem>
<CodeGroupItem title="create">

```sh
$ minid tx checkers create --help
```

Returns:

```txt
Creates a new checkers game at the index for the black and red players

Usage:
  minid tx checkers create index black red [flags]

Flags:
...
```

</CodeGroupItem>
</CodeGroup>

---

Just like you did in the [previous section](./1-preparation.md), you re-initialize and start it. You need to re-initialize because your genesis has changed once again.

```sh
$ make init
$ minid start
```

Now your minimal chain not only has a checkers module, but also a games storage area. After stopping it with <kbd>CTRL-C</kbd>, confirm that by calling up:

<CodeGroup>
<CodeGroupItem title="Straight">

```sh
$ minid export --height 1 --modules-to-export checkers
```

</CodeGroupItem>
<CodeGroupItem title="Clean">

```sh
$ minid export --height 1 --modules-to-export checkers | tail -n 1 | jq
```

</CodeGroupItem>
</CodeGroup>

## Conclusion

If you run `minid query --help`, you can see that there is no `checkers` in the list of commands. So although you can create games, you cannot retrieve them yet. This is the object of the next section.